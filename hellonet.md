https://www.cs.princeton.edu/courses/archive/fall21/cos461/schedule.html
https://www.cs.princeton.edu/courses/archive/fall21/cos461/schedule.html

Supplementary Textbooks: (Excerpts on Blackboard)

Computer Networking (5th edition), by Andrew Tanenbaum and David Wetherall
Computer Networking: A Top-Down Approach (6th edition), by Jim Kurose and Keith Ross
Recommended Books: Other recommended textbooks for reference include:

Computer Networks: A Systems Approach, Larry Peterson and Bruce Davie
TCP/IP Illustrated, Volume 1: The Protocols and Unix Network Programming, Volume 1: The Sockets Networking API (3rd Edition)
Purchasing these books is not necessary; they will be on reserve in the engineering library, and online resources will be made available.


Key Concepts in Networking
• Naming
– What to call computers, services, protocols, …
• Layering
– Abstraction is the key to managing complexity
• Protocols
– Speaking the same language
– Syntax and semantics
• Resource allocation
– Dividing scare resources among competing parties
– Memory, link bandwidth, wireless spectrum, paths



Learning the Material: Books (secondary)
• Main textbook
– Computer Networks: A Systems Approach, by Peterson and Davie
– Also online: https://book.systemsapproach.org/
• Additional books (may be of interest)
– Networking textbooks
• Computer Networking: A Top-Down Approach Featuring the Internet, by Kurose and Ross
• Computer Networks, by Tanenbaum
– Network programming references
• TCP/IP Illustrated, Volume 1: The Protocols, by Stevens
• Unix Network Programming, Vol 1: Sockets Networking API, by Stevens, Fenner, & Rudolf


Link-Layer Services
• Encoding
– Represent the 0s and 1s
• Framing
– Encapsulate packet into frame, adding header/trailer
• Error detection
– Receiver detecting errors with checksums
• Error correction
– Receiver optionally correcting errors
• Flow control
– Pacing between sending and receiving nodes





medium access control address
Why Not Just Use IP Addresses?
• Links can support any network protocol
– Not just for IP (e.g., IPX, Appletalk, X.25, …)
– Different addresses on different kinds of links
• An adapter may move to a new location 
– So, cannot simply assign a static IP address
– Instead, must reconfigure the adapter’s IP address
• Must identify the adapter during bootstrap
– Need to talk to the adapter to assign it an IP address



Error control in the Internet stack
29
LL header LL payload LL CRC
• Transport layer
– Internet Checksum (IC) over 
TCP/UDP header, data
• Network layer (L3)
– IC over IP header only
• Link layer (L2)
– Cyclic Redundancy Check (CRC)
• Physical layer (PHY)
– Error Control Coding (ECC),or
– Forward Error Correction (FEC)


包转发机制
Is Best Effort Good Enough?
• Packet loss and delay
– Sender can resend
• Packet corruption
– Receiver can detect, 
and sender can resend
• Out-of-order delivery
– Receiver can put the 
data back in order
• Packets follow 
different paths
– Doesn’t matter
• Network failure
– Drop the packet
• Network congestion
– Drop the packet


Packet (Y) vs. Circuit Switching (A)?
• Predictable performance
• Network never blocks senders
• Reliable, in-order delivery
• Low delay to send data
• Simple forwarding
• No overhead for packet headers
• High utilization under most workloads
• No per-connection network state
9
Circuit
Packet
Circuit
Packet
Circuit
Circuit
Packet
Packet


Grouping Related Hosts
• The Internet is an “inter-network”
– Used to connect networks together, not hosts
– Need to address a network (i.e., group of hosts)
LAN = Local Area Network
WAN = Wide Area Network


为了便捷管理地址   分成网络段  主机段
经典地址
Classful Addressing
• In the olden days, only fixed allocation sizes
– Class A: 0*
• Very large /8 blocks (e.g., MIT has 18.0.0.0/8)
– Class B: 10*
• Large /16 blocks (e.g,. Princeton has 128.112.0.0/16)
– Class C: 110*
• Small /24 blocks (e.g., AT&T Labs has 192.20.225.0/24)
– Class D: 1110* for multicast groups
– Class E: 11110* reserved for future use
• This is why folks use dotted-quad notation

无类域间地址
避免广播暴风
避免地址浪费


Classless Inter-Domain Routing (CIDR)
19
IP Address : 12.4.0.0 IP Mask: 255.254.0.0
00001100 00000100 00000000 00000000
11111111 11111110 00000000 00000000
Address 
Mask
Network Prefix for hosts 
Written as 12.4.0.0/15
• Use two 32-bit numbers to represent network:
Network number = IP address + Mask


Hierarchical Address Allocation
• Hierarchy is key to scalability
– Address allocated in contiguous chunks (prefixes)
– Today, the Internet has about 600-800,000 prefixes


Obtaining a Block of Addresses
• Internet Corporation for Assigned Names and 
Numbers (ICANN)
– Allocates large blocks to Regional Internet Registries
• Regional Internet Registries (RIRs)
– E.g., ARIN (American Registry for Internet Numbers)
– Allocates to ISPs and large institutions
• Internet Service Providers (ISPs)
– Allocate address blocks to their customers
– Who may, in turn, allocate to their customers…

路由转发
Hop-by-Hop Packet Forwarding
• Each router has a forwarding table
– Maps destination address to outgoing interface
• Upon receiving a packet
– Inspect the destination address in the header
– Index into the table
– Determine the outgoing interface
– Forward the packet out that interface
• Then, the next router in the path repeats

CIDR让寻址变得复杂 
两个网段匹配， 选最长的匹配
CIDR Makes Packet Forwarding Harder
• Forwarding table may have many matches
– E.g., entries for 201.10.0.0/21 and 201.10.6.0/23
– The IP address 201.10.6.17 would match both!

Longest Prefix Match Forwarding
• Destination-based forwarding
– Packet has a destination address
– Router identifies longest-matching prefix
– Cute algorithmic problem: very fast lookups



转发表， 静态的问题， 容易 出错， 时时更新
所以出现路由协议
Creating a Forwarding Table
• Entries can be statically configured
– E.g., “map 12.34.158.0/24 to Serial0/0.1”
• But, this doesn’t adapt 
– To failures
– To new equipment
– To the need to balance load
• That is where the control plane comes in
– Routing protocols


路由三层 planes
data层
control 层
management 层


ip包结构 
包类型 包版本  包长度   头长度  最长跳数 校验和   双方地址 TCP UDP


IP Header: 
Version, Length, ToS
• Version number (4 bits)
– Necessary to know what other fields to expect
– Typically “4” (for IPv4), and sometimes “6” (for IPv6)
• Header length (4 bits)
– Number of 32-bit words in the header
– Typically “5” (for a 20-byte IPv4 header)
– Can be more when “IP options” are used
• Type-of-Service (8 bits)
– Allow different packets to be treated differently
– Low delay for audio, high bandwidth for bulk transfer


IP Header: 
Length, Fragments, TTL
• Total length (16 bits)
– Number of bytes in the packet
– Max size is 63,535 bytes (216 -1)
– … though most links impose smaller limits
• Time-To-Live (8 bits)
– Used to identify packets stuck in forwarding loops
– … and eventually discard them from the network
• Fragmentation information (32 bits)
– Supports dividing a large IP packet into fragments
– … in case a link cannot handle a large IP packet



Self Learning: Building the Table
• When a frame arrives
– Inspect the source MAC address
– Associate the address with the incoming interface
– Store the mapping in the switch table
– Use a timer to eventually forget the mapping

自学习， 当不知道目标， 发给所有接口， 除了来源
Self Learning: Handling Misses
• When frame arrives with unfamiliar destination
– Forward the frame out all of the interfaces
– … except for the one where the frame arrived
– Hopefully, this case won’t happen very often


Basic Router Architecture
• Each switch/router has a forwarding table
– Maps destination address to outgoing interface


 Basic operation
1. Receive packet
2. Look at header to determine destination address
3. Look in forwarding table to determine output interface
4. Modify packet header (e.g., decr TTL, update chksum)
5. Send packet to output interface


路由器基本功能
查表转发
• Basic operation
1. Receive packet
2. Look at header to determine destination address
3. Look in forwarding table to determine output interface
4. Modify packet header (e.g., decr TTL, update chksum)
5. Send packet to output interface



不同协议的查表方法
Ethernet (48 bits)
MPLS
ATM
Exact Match
• Direct lookup
• Associative lookup
• Hashing
• Binary tree
IPv4 (32 bits)
IPv6 (128 bits) Longest-Prefix Match
• Radix trie
• Compressed trie
• TCAM


Software LPM lookup using trie
• Prefixes “spelled out” by following path from root
前缀树
• To find the best prefix spell out address in trie

硬件查找 技术， 不太懂
Hardware for LPM lookup
• Content-Address Memory (CAM)
– Input: tag (address)
– Output: value (port
– Exact match, but O(1) in hardware
• Ternary CAM
– Can have wildcards: 0, 1, * 
– “value” memory cell and “mask” (care / don’t care) cell
• LPM via TCAM
– In parallel, search all prefixes for all matches
– Then choose longest match
• Trick: choose first match, but already sorted by prefix length


Crossbar switching
• Every input port has connection to every output port
• In each timeslot, each input connected to zero or 
more outputs


Conclusions
• Physical devices sharing L2 & L3 networks have many 
common features
– Forward table lookups
– Queueing and backplane switching
– Fast vs. slow paths
• Switches and routers separate routing decisions (control 
plane) from forwarding actions (data plane)
• High speed necessitates innovation
– Specialized hardware 
– Software algorithms


Transport Protocols
消息到segement, segment到消息
• Logical communication between processes
– Sender divides a message into segments
– Receiver reassembles segments into message 
流控制 
• Transport services
– (De)multiplexing packets
–Detecting corrupted data
–Optionally: reliable delivery, flow control, …


User Datagram Protocol (UDP)
• Lightweight communication 
between processes
– Send and receive messages
– Avoid overhead of ordered, 
reliable delivery
• No connection setup delay, no 
in-kernel connection state
• Used by popular apps
– Query/response for DNS
– Real-time data in VoIP

udp优点  尽快发包
Advantages of UDP
• Fine-grain control
– UDP sends as soon as the application writes 
• No connection set-up delay
– UDP sends without establishing a connection
• No connection state in host OS
– No buffers, parameters, sequence #s, etc.
• Small header overhead
– UDP header is only eight-bytes long

TCP两个基本特征
Two Basic Transport Features

基于端口的多路复用
二 错误检测


TCP协议特点
ransmission Control Protocol (TCP)
• Stream-of-bytes service
– Sends and receives a 
stream of bytes
• Reliable, in-order delivery
– Corruption: checksums
– Detect loss/reordering: 
sequence numbers
– Reliable delivery: 
acknowledgments and 
retransmissions
• Connection oriented
– Explicit set-up and teardown of TCP connection
• Flow control
– Prevent overflow of the 
receiver’s buffer space
• Congestion control
– Adapt to network 
congestion for the 
greater good


breaking a stream of bytes into tcp segments. 

通过窗口控制 
Sequence Number


TCP 类比 
 Alice and Bob are talking
– What if Alice couldn’t understand Bob?
– Bob asks Alice to repeat what she said
• What if Bob hasn’t heard Alice for a while?
– Is Alice just being quiet? Has she lost reception?
– How long should Bob just keep on talking?
– Maybe Alice should periodically say “uh huh”
– … or Bob should ask “Can you hear me now?”


TCP Support for Reliable Delivery
• Detect bit errors: checksum
– Used to detect corrupted data at the receiver
– …leading the receiver to drop the packet
• Detect missing data: sequence number
– Used to detect a gap in the stream of bytes
– ... and for putting the data back in order
• Recover from lost data: retransmission
– Sender retransmits lost or corrupted data
– Two main ways to detect lost packets

划动窗口
Flow Control:
TCP Sliding Window

三次握手
a一发一收 B一发一收  最小三次
Establishing a TCP Connection
• Three-way handshake to establish connection
– Host A sends a SYN (open) to the host B
– Host B returns a SYN acknowledgment (SYN ACK)
– Host A sends an ACK to acknowledge the SYN ACK

关闭收到  收到。 
Closing (each end of) the connection
– Finish (FIN) to close and receive remaining bytes
– And other host sends a FIN ACK to acknowledge
– Reset (RST) to close and not receive remaining bytes


拥塞控制 
Network Congestion: Context
• Best-effort network does not “block” calls
– So, they can easily become overloaded
– Congestion == “Load higher than capacity”
• Examples of congestion
– Link layer: Ethernet frame collisions
– Network layer: full IP packet buffers 
• Excess packets are simply dropped
– And the sender can simply retransmit

检测和响应拥塞
 What does the end host see?
• What can the end host change?
• Distributed Resource Sharing

拥塞控制 
TCP Congestion Control
• Additive increase, multiplicative decrease
– On packet loss, divide congestion window in half
– On success for last window, increase window linearly

速度响应不利情况
Why Multiplicative?
• Respond aggressively to bad news
– Congestion is (very) bad for everyone
– Need to react aggressively
Examples of exponential backoff:
– TCP: divide sending rate in half
– Ethernet: double retransmission timer
• Nice theoretical properties
– Makes efficient use of network resources


如何判断拥塞
How it Looks to the End Host
• Delay: Packet experiences high delay
• Loss: Packet gets dropped along path
• How does TCP sender learn this?
– Delay: Round-trip time estimate
– Loss: Timeout and/or duplicate acknowledgments

拥塞窗口

TCP Congestion Window
优点   避免处理响应窗口大小
缺点 不是过高就是过低
• Each TCP sender maintains a congestion window
– Max number of bytes to have in transit (not yet ACK’d)
• Adapting the congestion window
– Decrease upon losing a packet: backing off
– Increase upon success: optimistically exploring
– Always struggling to find right transfer rate
• Tradeoff
– Pro: avoids needing explicit network feedback
– Con: continually under- and over-shoots “right” rate


窗口调整策略
Additive Increase, Multiplicative Decrease 
• How much to adapt?
– Additive increase: On success of last window of 
data, increase window by 1 Max Segment Size (MSS)
– Multiplicative decrease: On loss of packet, divide 
congestion window in half
• Much quicker to slow down than speed up?
– Over-sized windows (causing loss) are much worse 
than under-sized windows (causing lower thruput)
– AIMD: A necessary condition for stability of TCP

慢启动， 快增加（指数式增加）
“Slow Start” Phase
• Start with a small congestion window
– Initially, CWND is 1 MSS
– So, initial sending rate is MSS / RTT
• Could be pretty wasteful
– Might be much less than actual bandwidth
– Linear increase takes a long time to accelerate
• Slow-start phase (really “fast start”)
– Sender starts at a slow rate (hence the name)
– … but increases rate exponentially until the first loss


Slow Start in Action
29
Double CWND per round-trip time


用旧窗口可能会出现问题
Repeating Slow Start After Idle Period
• Suppose a TCP connection goes idle for a while
• Eventually, the network conditions change
– Maybe many more flows are traversing the link
• Dangerous to start transmitting at the old rate
– Previously-idle TCP sender might blast network
– … causing excessive congestion and packet loss
• So, some TCP implementations repeat slow start
– Slow-start restart after an idle period

TCP 拥塞总结
Conclusions
• Congestion is inevitable
– Internet does not reserve resources in advance
– TCP actively tries to push the envelope
• Congestion can be handled
– Additive increase, multiplicative decrease
– Slow start and slow-start restart
• Fundamental tensions
– Feedback from the network?
– Enforcement of “TCP friendly” behavior?


Line Cards (Interface Cards, Adaptors)
• Packet handling
– Packet forwarding
– Buffer management
– Link scheduling
– Packet filtering
– Rate limiting
– Packet marking
– Measurement

包控制 
Queue Management Issues
• Scheduling discipline
– Which packet to send?
– Some notion of fairness? Priority?
• Drop policy
– When should you discard a packet?
– Which packet to discard?
• Goal: balance throughput and delay
– Huge buffers minimize drops, but add to queuing 
delay (thus higher RTT, longer slow start, …)

过载控制 
Explicit Congestion Notification
• Needs support by router, sender, AND receiver
– End-hosts check ECN-capable during TCP handshake
• ECN protocol (repurposes 4 header bits)
1. Sender marks “ECN-capable” when sending
2. If router sees “ECN-capable” and congested, marks 
packet as “ECN congestion experienced”
3. If receiver sees “congestion experienced”, marks 
“ECN echo” flag in responses until congestion ACK’d
4. If sender sees “ECN echo”, reduces cwnd and marks 
“congestion window reduced” flag in next packet
公平与效率
链路调度
Link Scheduling
1 先到先得
2 高低级通道
Strict Priority Queuing
• Multiple levels of priority
– Always transmit high-priority traffic (when present)
• Isolation for the high-priority traffic
– Almost like it has a dedicated link
– Except for (small) delay for packet transmission
• But, lower-priority traffic may starve

另一种算法
有权重的队列算法
Generalized processor sharing (GPS) is an ideal scheduling algorithm for process schedulers and network schedulers. It is related to the fair-queuing principle which groups packets into classes and shares the service capacity between them. GPS shares this capacity according to some fixed weights.[1]

In process scheduling, GPS is "an idealized scheduling algorithm that achieves perfect fairness. All practical schedulers approximate GPS and use it as a reference to measure fairness."[2]

Generalized processor sharing assumes that traffic is fluid (infinitesimal packet sizes), and can be arbitrarily split. There are several service disciplines which track the performance of GPS quite closely such as weighted fair queuing (WFQ),[3] also known as packet-by-packet generalized processor sharing (PGPS).

Justification
In a network such as the internet, different application types require different levels of performance. For example, email is a genuinely store and forward kind of application, but videoconferencing isn't since it requires low latency. When packets are queued up on one end of a congested link, the node usually has some freedom in deciding the order in which it should send the queued packets. One example ordering is simply first-come, first-served, which works fine if the sizes of the queues are small, but can result in problems if there are latency sensitive packets being blocked by packets from bursty, higher bandwidth applications.


In GPS, and all protocols inspired by GPS, the choice of the weights is left to the network administrator.

Generalized processor sharing assumes that the traffic is fluid, i.e., infinitely divisible so that whenever an application type has packets in the queue, it will receive exactly the fraction of the server given by the formula above. However, traffic is not fluid and consists of packets, possibly of variable sizes. Therefore, GPS is mostly a theoretical idea, and several scheduling algorithms have been developed to approximate this GPS ideal: PGPS, aka Weighted fair queuing, is the most known implementation of GPS, but it has some drawbacks, and several other implementations have been proposed, as Deficit round robin or WF2Q.[4]

GPS is considered as a fair ideal, and all its approximations "use it as a reference to measure fairness."[2] Nevertheless, several Fairness measures exist.

GPS is insensible to packet sizes, since it assumes a fluid model.


Weighted Fair Queuing
• If non-work conserving (resources can go idle)
– Each flow gets at most its allocated weight
• WFQ is work-conserving
– Send extra traffic from one queue if others are idle
– Results in (Y) higher or (M) lower utilization than nonwork conserving?
• WFQ results in max-min fairness
– Maximize the minimum rate of each flow


均数， 取最小
Max-Min Fairness
• Maximize the minimum rate of each flow
1. Allocate in order of increasing demand
2. No flow gets more than demand
3. The excess, if any, is equally shared


Max-Min Fairness
• Maximize the minimum rate of each flow
1. Allocate in order of increasing demand
2. No flow gets more than demand
3. The excess, if any, is equally shared



f = 4: 
min(8, 4) = 4
min(10, 4) = 4
min(2, 4) = 2


另一种带宽分配策略
bit by bit fair queuing
Question: What is a “flow”? 
Flow 5-tuple: protocol, IP source/dest, port src/dest
Question: How can we give weights?
Protocol class, bit markings, prefixes, etc


位实现上没有可能 用包代替
Packet vs. “Fluid” System
• Bit-by-bit FQ is not implementable:
…In real packet-based systems:
– One queue is served at any given time
– Packet transmission cannot be preempted
• Goal: A packet scheme close to fluid system
– Bound performance w.r.t. fluid system


First Cut: Simple Round Robin
• Serve a packet from non-empty queues in turn
– Let’s assume all flows have equal weight
• Variable packet length à get more service by 
sending bigger packets
• Unfair instantaneous service rate (especially with 
variable weights)
– What if a packet arrives right after its “turn”

实现权重流量队列 
Implementing WFQ
Challenge: Determining finish time is hard
Idea: Don’t need finish time. Need finish order.
The finish order is a lot easier to calculate.

流量区分
Distinguishing Traffic
• Applications compete for bandwidth
– E-mail traffic can cause congestion/losses for VoIP
• Principle 1: Packet marking 
– So router can distinguish between classes
– E.g., Type of Service (ToS) bits in IP header


网络理想 点与点可连接
Internet Ideal: Simple Network Model
• Globally unique identifiers
– Each node has a unique, fixed IP address
– … reachable from everyone and everywhere
• Simple packet forwarding
– Network nodes simply forward packets
– … rather than modifying or filtering them

网络现实
Internet Reality
• Host mobility 主机移动
– Host changing address 
as it moves
• IP address depletion  地址冲突
– Multiple hosts using 
the same address
• Security concerns 安全问题 非法流量
– Detecting and blocking 
unwanted traffic
• Replicated services  负载
– Load balancing over 
server replicas
• Performance concerns  
– Allocating bandwidth, 
caching content, …
• Incremental deployment
– New technology 
deployed in stages

网络中的插件
地址转换， 防火墙， 流量定型， 攻击检测， 透明代理， 应用加速 
Middleboxes BREAK the Simple Network Model
• Middleboxes are intermediaries
– Interposed between communicating hosts
– Often without knowledge of one or both parties
• Myriad uses
– Address translators
– Firewalls
– Traffic shapers
– Intrusion detection
– Transparent proxies
– Application accelerators


防火墙
 A firewall filters packet-by-packet, based on:
– Source and destination IP addresses and port #’s
– TCP SYN and ACK bits; ICMP message type
– Deep packet inspection of packet contents (DPI)

包过滤方法
Packet Filtering Examples
• Block all packets with IP protocol field = 17 and 
with either source or dest port = 23.
– All incoming and outgoing UDP flows blocked
– All Telnet connections are blocked
• Block inbound TCP packets with SYN but no ACK
– Prevents external clients from making TCP 
connections with internal clients
– But allows internal clients to connect to outside

匹配规则  决定丢弃还是保持
Firewall Configuration
• Firewall applies a set of rules to each packet
– To decide whether to permit or deny the packet
• Each rule is a test on the packet
– Comparing headers, deciding whether to allow/deny
• Rule order matters
– Once packet matches rule, drop/keep decision is made

有状态防火墙
Stateful Firewall
• Stateless firewall: 
– Treats each packet independently
• Stateful firewall
– Remembers connection-level information
– E.g., client initiating connection with a server
– … allows the server to send return traffic

流量管理
A Variation: Traffic Management

• Permit vs. deny is too binary a decision
– Classify traffic using rules, handle classes differently
流量控制 
• Traffic shaping (rate limiting)
– Limit the amount of bandwidth for certain traffic
• Separate queues
– Use rules to group related packets
– And then do weighted fair scheduling across groups

nat解决v4地址不足问题
History of NATs
• IP address space depletion
– Clear in early 90s that 232 addresses not enough
– Work began on a successor to IPv4
• In the meantime…
– Share addresses among numerous devices
– … without requiring changes to existing hosts
• Meant as a short-term remedy
– Now: NAT is widely deployed, much more than IPv6


nat重写地址， 重写校验和。 
NAT rewrites the IP addresses
• Make “inside” look like single IP addr
• Change header checksums accordingly


重写源地址， 重写目标地址

通过端口转换
Port-Translating NAT
• Two hosts communicate with same destination
– Destination needs to differentiate the two
• Map outgoing packets
– Change source IP address and source port
• Maintain a translation table
– Map of (src addr, port #) to (local addr, old port #) 
• Map incoming (reply) packets
– Map (dst addr, port #) to (local addr, old port #)



端口对应  端口转换


Maintaining the Mapping Table
• Create an entry upon seeing an outgoing packet
– Packet with new (local addr, source port) pair
• Eventually, need to delete entries to free up #’s
– When? If no packets arrive before a timeout
– (At risk of disrupting a temporarily idle connection)
• An example of “soft state”
– i.e., removing state if not refreshed for a while

那儿用到Nat

Where is NAT Implemented?
• Home wireless router
– Integrates router, Wi-Fi, DHCP server, NAT, etc.
– Use single IP address from the service provider
• Campus or corporate network
– NAT at the connection to the Internet
– Share a collection of public IP addresses
– Avoid complexity of renumbering hosts/routers 
when changing ISP (w/ provider-allocated IP prefix)


Not just servers: peer-to-peer traffic
1. Peer “registers” with rendezvous
2. Other peer contacts rendezvous
3. Rendezvous sends to each peer 
the others’ IP:port
4. Try to connect in each direction. 
If one succeeds, done
5. Otherwise, proxy through relay


Principled Objections Against NAT
• Routers are not supposed to look at port #s
– Network layer should care only about IP header
– … and not be looking at the port numbers at all
• NAT violates the end-to-end argument
– Network nodes should not modify the packets
• IPv6 is a cleaner solution
– Better to migrate than to limp along with a hack
24
That’s what happens when network 
puts power in hands of end users


Replicated Servers
• One site, many servers

提高网络效率方法
 Improve end-to-end performance
– Through buffering, compression, caching, …
• Incrementally deployable
– No changes to end hosts or the rest of the Internet

ip tunneling
IP包里封ip包
IP Tunneling
• IP tunnel is a virtual point-to-point link
– Illusion of a direct link between two nodes
• Encapsulation of the packet inside IP datagram
– Node B sends a packet to node E
– … containing another packet as the payload

在ipv4上应用Ipv6
6Bone: Deploying IPv6 over IP4

包里封闭包
vpn 

路由
Routing

点对点到实际路径
Routing: Mapping End-to-End 
Communication to Path


Routing vs. Forwarding
控制层， 负责更新转发表
• Routing: control plane
– Computing paths the packets will follow
– Routers talk among themselves
à Create the forwarding tables
• Forwarding: data plane
–Directing a data packet to an outgoing link
–Using the forwarding tables

路由的三个主要问题
Three Issues to Address
找最短路
• What does the protocol compute?
– E.g., shortest paths

• What algorithm does the protocol run?
– E.g., link-state routing
• How do routers learn end-host locations?
– E.g., injecting into the routing protocol


End-to-End Signaling
• Establish end-to-end path in advance
– Learn the topology (as in link-state routing)
– End host or router computes and signals a path
• Signaling: install entry for each circuit at each hop
• Forwarding: look up the circuit id in the table


学习 与广播
Learning and Flooding
• When a frame arrives
– Inspect the source address
– Associate address with the 
incoming interface
• When the frame has an 
unfamiliar destination
– Forward out all interfaces
– … except incoming interface


不同网不同路由方式
Conclusions: Many Different Solutions
• Ethernet LAN and home networks
– Spanning tree, MAC learning, flooding
• Enterprise
– Link-state routing, injecting subnet addresses
• Backbone
– Link-state routing inside, path-vector routing with 
neighboring domains, and iBGP dissemination
• Data centers
– Many different solutions, still in flu


路由汇聚    
拓扑变化， 主机转移 
• Topology changes: new route to the same place
• Host mobility: route to a different place

拓扑变化两大原因
Two Types of Topology Changes
• Planned
– Maintenance: shut down a node or link
– Energy savings: shut down a node or link 
– Traffic engineering: change routing configuration
• Unplanned Failures
– Fiber cut,
faulty equipment,
power outage, 
software bugs, …


Convergence Delay
• Sources of convergence delay
– Detection latency
– Updating control-plane information
– Computing and install new forwarding tables
• Performance during convergence period
– Lost packets due to blackholes and TTL expiry
– Looping packets consuming resources
– Out-of-order packets reaching the destination
• Very bad for VoIP, online gaming, and video

Distance Vector: Link Cost Changes
• Link cost decreases and recovery
– Node updates the distance table 
– Rule: Least-cost path’s cost changed? notify neighbors


path-Vector Routing
• Extension of distance-vector routing
– Support flexible routing policies
– Avoid count-to-infinity problem
• Key idea: advertise the entire path
– Distance vector: send distance metric per dest d
– Path vector: send the entire path for each dest d

BGP, 在可靠协议上做。 直接取路由表。 Border Gateway Protocol (BGP) i
BGP Session Failure 
• BGP runs over TCP
– BGP only sends updates when changes occur
– TCP doesn’t detect lost connectivity on its own
• Detecting a failure
– Keep-alive: 60 seconds
– Hold timer: 180 seconds
• Reacting to a failure
– Discard all routes learned from neighbor
– Send new updates for any routes that change

as自治域
BGP Converges Slowly
• Path vector avoids count-to-infinity
– But, ASes still must explore many alternate paths to 
find highest-ranked available path
• Fortunately, in practice
– Most popular destinations have stable BGP routes
– Most instability lies in a few unpopular destinations
• Still, lower BGP convergence delay is a goal
– Can be tens of seconds to tens of minutes




AS (Autonomous System)
Business Relationships