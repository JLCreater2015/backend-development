# TCP的粘包问题

## ✏ TCP选项之MSS

MSS（Maximum Segment Size，最大报文段大小）的概念是指TCP层所能够接收的最大段大小，该值只包括TCP段的数据部分，不包括选项部分。在TCP首部有一个MSS选项，在三次握手过程中，TCP发送端使用该选项告诉对方自己所能接受的最大段大小。

