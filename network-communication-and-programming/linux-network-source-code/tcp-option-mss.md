# TCP选项之MSS

## \*\*\*\*✏ **1、三次握手**

### \*\*\*\*🖋 **1.1、客户端**

客户端处理`MSS`的机会为：

1. 发送SYN段时告诉服务器端本端能够接收的`MSS`；
2. 收到`SYN+ACK`后接收服务器端通告的`MSS`。

#### 🐹 **1.1.1、**SYN段MSS选项值

SYN段也是通过`tcp_transmit_skb()`发送的，在该函数中会调用`tcp_syn_build_options()`构造SYN段携带的选项，代码如下：

```cpp
static int tcp_transmit_skb(struct sock *sk, struct sk_buff *skb, int clone_it,
			    gfp_t gfp_mask)
{
...
	//如果发送的是SYN段，则调用tcp_syn_build_options()构造要携带的TCP选项，其中
	//MSS选项的值就是tcp_advertise_mss()的返回值
	if (unlikely(tcb->flags & TCPCB_FLAG_SYN)) {
		tcp_syn_build_options((__be32 *)(th + 1),
				      tcp_advertise_mss(sk),
				      (sysctl_flags & SYSCTL_FLAG_TSTAMPS),
				      (sysctl_flags & SYSCTL_FLAG_SACK),
				      (sysctl_flags & SYSCTL_FLAG_WSCALE),
				      tp->rx_opt.rcv_wscale,
				      tcb->when,
				      tp->rx_opt.ts_recent,
#ifdef CONFIG_TCP_MD5SIG
				      md5 ? &md5_hash_location :
#endif
				      NULL);
	}
...
}
```

**`tcp_advertise_mss()`**

advertise为广告的意思，该函数用来计算要告诉对端的`MSS`值，对应到TCB中，其实就是根据本端设备的`MTU`计算`tp->advmss`的值。

```cpp
static __u16 tcp_advertise_mss(struct sock *sk)
{
	struct tcp_sock *tp = tcp_sk(sk);
	//获取路由
	struct dst_entry *dst = __sk_dst_get(sk);
	//用tp->advmss初始化临时变量mss
	int mss = tp->advmss;

	//如果路由有效并且路由中的MSS比当前值小，那么用路由中的MSS更新tp->advmss
	//因为路由中的MSS是根据网络设备的MTU得来的，必须尊重，可以认为路由中的MSS
	//取值为min(65536-40, MTU-40)，其中65535为IP报文的最大长度
	if (dst && dst_metric(dst, RTAX_ADVMSS) < mss) {
		mss = dst_metric(dst, RTAX_ADVMSS);
		tp->advmss = mss;
	}
	//返回确定的mss
	return (__u16)mss;
}
```

从上面可以看出，`tcp_advertise_mss()`实际上就是取当前`tp->advmss`和路由表`MSS`二者中的最小值，而后者就是本地网卡的`MTU-40`，`tp->advmss`的初始化是在`tcp_connect_init()`中完成的，该函数被`tcp_connect()`调用，也就是说该值的初始化是在SYN段的发送过程中进行的，代码如下：

```cpp
static void tcp_connect_init(struct sock *sk)
{
...
	//即也是来源于本端MTU
	tp->advmss = dst_metric(dst, RTAX_ADVMSS);
...
}
```

**SYN段中携带的`MSS`选项值实际上就是本端网络设备的`MTU-40`。**

#### 🐹 **1.1.2、**接收SYN+ACK段

接收`SYN+ACK`段是在`tcp_rcv_synsent_state_process()`中处理的，其中会调用`tcp_paser_option()`解析SYN段中携带的选项，其中和`MSS`选项相关的代码如下：

```cpp
void tcp_parse_options(struct sk_buff *skb, struct tcp_options_received *opt_rx,
		       int estab)
{
...
	switch (opcode) {
	case TCPOPT_MSS:
		if (opsize == TCPOLEN_MSS && th->syn && !estab) {
			//取得选项中携带的MSS值记录到临时变量in_mss中
			u16 in_mss = ntohs(get_unaligned((__be16 *)ptr));
			if (in_mss) {
				//user_mss是应用程序通过TCP选项TCP_MAXSEG设定的，如果不设置，默认为0；
				//如果设定了user_mss并且设定的值小于对端通告的，那么调整in_mss为两者中的最小值。
				if (opt_rx->user_mss &&
					opt_rx->user_mss < in_mss)
					in_mss = opt_rx->user_mss;
				//将min(user_mss, in_mss)记录到mss_clamp中
				opt_rx->mss_clamp = in_mss;
			}
		}
		break;
	}
	...
}
```

总结：客户端在收到服务器端通告的`MSS`后，将其与应用程序通过`TCP_MAXSEG`设定的`MSS`值比较，将二者中的较小值保存在`tp->rx_opt.mss_clamp`中，该值会影响到客户端发送`MSS`的确定。

### 🖋 1.2、服务端

服务器端处理`MSS`的机会为：

1. 收到SYN段后对客户端通告的`MSS`的处理；
2. 发送`SYN+ACK`时携带的`MSS`选项值的确定；

#### 🐹 1.2.1、接收SYN段

SYN段的核心处理在`tcp_v4_conn_request()`中完成的，其中和`MSS`选项解析相关的内容如下：

```cpp
int tcp_v4_conn_request(struct sock *sk, struct sk_buff *skb)
{
...
	//将SYN段中携带的选项先解析到临时变量tmp_opt中
	struct tcp_options_received tmp_opt;
	struct request_sock *req;
...

	//先清空临时变量
	tcp_clear_options(&tmp_opt);
	//将临时变量中的mss_clamp初始化为536
	tmp_opt.mss_clamp = 536;
	//user_mss设置为用户通过套接字选项TCP_MAXSEG设定的值
	tmp_opt.user_mss  = tcp_sk(sk)->rx_opt.user_mss;
	//该函数上面已经见过，它会比较SYN段中携带的MSS和user_mss，
	//然后取二者中较小者保存在tmp_opt.mss_clamp中
	tcp_parse_options(skb, &tmp_opt, 0);
...
	//初始化连接请求块
	tcp_openreq_init(req, &tmp_opt, skb);
...
}

static inline void tcp_openreq_init(struct request_sock *req,
				    struct tcp_options_received *rx_opt,
				    struct sk_buff *skb)
{
...
	//最终确定下来的MSS记录到了连接请求块的mss字段中
	req->mss = rx_opt->mss_clamp;
...
}
```

记录到`req`中的`mss`，在三次握手完成后，创建子套接字的`TCB`时会赋值给`tp->rx_opt.mss_clamp`，代码如下：

