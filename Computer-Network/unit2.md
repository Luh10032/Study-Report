##### p10
非持续并行连接的HTTP协议传输：
一个下载对象分为两步骤 HTTP连接建立请求，发送请求. 
HTTP连接建立请求：请求连接+响应连接总共花费时间2*200bit/150bps
发送请求中：请求发送+响应请求 时间  200bit/150bps+100k/150bps
10个引用对象 为并行连接
1下载对象+10个引用对象总时间 T = (3* 200bit/150bps +100k/150bps)+(3* 200bit/15bps +100k/15bps)=7377s

持续的HTTP协议传输
中 只有一个连接  不能并行请求
则 总时间=连接请求时间+1个对象时间+10个引用对象时间=7351s

总结 没有意义，由于带宽太小 两种方式都没什么区别

##### p22
算最小分发时间
CS模式：  和客户上载速度无关
分发时间distribution time: D_cs=max{NF/U_s，F/d_min }
当N大于一定数后，瓶颈从客户端转到服务器，分发时间随着N的增加线行增加
P2P模式：
D_p2p=max{F/u_S,F/d_min,NF/(u_s+sum(u_client)客户端上载速度总和)}
随着上载带宽的增加，分发时间减少。随着上载能力的增加，上载能力已经起不到什么作用

##### P9
没有缓存：
一个对象时间=L/R=850k/15Mbps=0.0567s
流量强度I=16*850K/15Mbps=0.907
平均接入时延t_access=L/R*1/(1-I)=0.57 s
总时间t=0.57s+3s=3.57s
加缓存器：命中率0.4  
I=16*850k*0.6/15Mbps=0.544
t_access=0.124s
没有命中的请求的总时延t_hit_miss=0.124+3=3.124s
平均总延时=3.124s *0.6+ 0.01s(假设命中的请求延时为10ms)*0.4≈1.874s


##### p23
a). 如果U_s/N<=d_min 所有clien的下载速度大于服务器平均分配给client的下载速度，
这时候服务器上载能力是瓶颈，每个客户以u_s/N的速率接收，此时每个客户端的接收时间
是NF/u_s,由于所有客户端都在这个时间中接收，所以整体分发时间也是NF/u_s
b).同理，客户端下载速率小于服务器平均的分发速率，所以接收时间为F/d_min,整体分发时间为F/d_min
c).
D_cs=NF/u_s (u_s/N < d_min)
D_cs=F/d_min (u_s/N > d_min)
总和以上 D_cs=max{NF/U_s,F/d_min}

##### p24
a). u_s<= (u_s+u_client_download_sum)/N
服务器上载速度小于等于系统的平均上载速度，该情况下，服务器把文件
根据节点速度u_i在总速度u_sum中的比例划分数据块
服务器根据用户的上载能力u_i，给第i个节点分发数据块提供的上载速度为u_s * u_i/u_sum
从每个peer节点的上载来看
第i个节点向其他服务器提供的上载能力速度总和(N-1)*(u_i/u_sum)*u_s <=u_i 小于i节点本身的上载能力速度
由于u_s<= (u_s+u_sum)/N，该条件满足
从服务器的上载角度来看
sum(u_s*u_i/u_sum)其中i为1~N=u_s
从客户端下载角度来看:
第i个客户端从每个其他节点和服务器下载数据块
u_i+ sum( (ui/u_sum)*u_s )(不包括u_i) 
所以分发时间取决于服务器上载速度 即F/u_s

b). u_s>= (u_s+u_client_download_sum)/N
把文件F 分成N+1份 数据块
根据 
u_s>=(u_s+u_sum)/N   ->  u_s>=u_sum(N-1)
设服务器分给节点i的带宽 r_i=u_i/(N-1)
sum(r_i)=u_sum(N-1)
把第i块数据块分给第i个节点，其中第N+1块同时分给所有节点
则第N+1块的带宽r_(N+1)= ( u_s-u_sum/(N-1) )/N

服务器的所有上载带宽=
r_1+r_2...r_N +N*r_(N+1)
=u_sum/(N-1)+N*  ( u_s-u_sum/(N-1) )/N
=u_sum/(N-1)+u_s-u_sum/(N-1)
=u_s

从客户端peer角度来看 获得的带宽能力=
u_i_get= u_sum/(N-1)+r_(N+1)=(u_s+u_sum)/N
下载时间=F/ u_i_get=FN/(u_sum+u_s)
