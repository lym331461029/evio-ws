`evio-ws`是一个事件驱动的websocket网络框架，基于 [evio](https://github.com/tidwall/evio) 修改而来，协议升级和报文解析以及控制报文均在框架层处理，应用层只需关注业务即可。

选择基于evio而不是go标准网络库，是因为evio一个线程一个事件循环-单线程管理多连接的模型和现实中的某些业务有天然的契合。比如一个游戏房间内的多个玩家，任一时刻只能有一个玩家活跃，那让房间内所有玩家被同一个事件循环管理就非常适合。这种情况下使用go std阻塞式net.Conn然后在业务层并发控制就是舍近求远。当然了，大部分情况下还是使用go std网络库更方便。

因为evio支持多个事件循环（推荐cpu核心数），所以可能会有连接在事件循环之间迁移的需求（比如1号房间的所有玩家均被事件循环1管理，但其中一个玩家想到2号房价去，2好房间由事件循环2管理），后续会添加该功能。


`echo`
```shell
tcpkali -c 100 --ws --message "hello world"  --message-rate=10 127.0.0.1:5000

Destination: [127.0.0.1]:5000
Interface lo0 address [127.0.0.1]:0
Using interface lo0 to connect to [127.0.0.1]:5000
Ramped up to 100 connections.
Total data sent:     158.8 KiB (162605 bytes)
Total data received: 121.4 KiB (124357 bytes)
Bandwidth per channel: 0.002⇅ Mbps (0.3 kBps)
Aggregate bandwidth: 0.099↓, 0.130↑ Mbps
Packet rate estimate: 955.4↓, 955.4↑ (1↓, 1↑ TCP MSS/op)
Test duration: 10.0027 s.

```



