<!-- vim-markdown-toc GitLab -->

* [Glossary](#glossary)
* [Description](#description)
* [Converting a CounterSet Path to a CloudWatch Metric](#converting-a-counterset-path-to-a-cloudwatch-metric)
* [CounterSets with their Paths](#countersets-with-their-paths)
    * [List of a few CounterSets with Paths](#list-of-a-few-countersets-with-paths)
        * [Processor Information](#processor-information)
        * [Memory](#memory)
        * [Logical Disk](#logical-disk)
        * [Physical Disk](#physical-disk)
    * [Network counters](#network-counters)
* [All CounterSets](#all-countersets)
    * [Commands](#commands)
    * [Output - List of All CounterSets](#output-list-of-all-countersets)

<!-- vim-markdown-toc -->

# Glossary

|Term|Description| Example(s) |
| ------------- |:-------------:|---|
| CounterSet    | A broad category or grouping of related Counters| "Processor Information" <br/><br/> "Logical Disk"<br/><br/> "Memory" |
| CounterSet Path | Paths of specific counters inside the CounterSet| `\Processor Information(*)\Performance Limit Flags`<br/>`\Processor Information(*)\% Performance Limit` <br/><br/> `\LogicalDisk(*)\% Free Space`|

# Description

This page details a list of all Windows Performance CounterSets using [`Get-Counter` cmdlet](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-counter?view=powershell-7.3)

The paths contained with a subset of the Counter Sets is then shown for perusal.

# Converting a CounterSet Path to a CloudWatch Metric

A `CounterSet Path` directly relates to a metric that can be made visible on the Cloudwatch Dashboard.

In the table below, note how a CounterSet Path is translated into a metric in Cloudwatch Agent configuration:

1. The CounterSet Name (such as 'Logical Disk', 'Memory') is used as the metric name
1. The CounterSet Path (such as '% Free Space', '% Committed Bytes In Use') is used in `measurement`

| CounterSet Path | Corresponding CW Agent Configuration Snippet |
|---|---|
|`\LogicalDisk(*)\% Free Space`&emsp;|<code>metrics_collected":&emsp;{<br/>&emsp;&emsp;&emsp;&emsp;"LogicalDisk":&emsp;{<br/>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;"measurement":&emsp;["%&emsp;Free&emsp;Space"],<br/>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;"resources":&emsp;["*"]<br/>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},<br/>....</code>|
|`\Memory\% Committed Bytes In Use`&emsp;|<code>metrics_collected":&emsp;{<br/>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;"Memory":&emsp;{<br/>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;"measurement":&emsp;["%&emsp;Committed&emsp;Bytes&emsp;In&emsp;Use"]<br/>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},<br/>....</code>|

# CounterSets with their Paths

This section lists CounterSet Paths of some of the performance counters:

1. 'Processor Information'
1. 'memory'
1. 'Logical Disk'
1. 'Physical Disk'
1. 'Network'

The paths of each of the above are stored in a `$counters` variable and the value is then echoed to `.\Counters-and-paths.txt`

```
PS> $counters = (Get-Counter -ListSet "Processor Information").paths ; $counters += (Get-Counter -listSet "memory").paths ; $counters += (Get-Counter -listSet "LogicalDisk").paths ; $counters += (Get-Counter -listSet "PhysicalDisk").paths ; $counters += (Get-Counter -listSet "*network*").paths; ($counters | Measure-Object).count
235

PS> Get-Variable counters -ValueOnly  | Out-File .\Counters-and-paths.txt
```

## List of a few CounterSets with Paths

Content of `.\Counters-and-paths.txt` separated by CounterSet

### Processor Information

```
\Processor Information(*)\Performance Limit Flags
\Processor Information(*)\% Performance Limit
\Processor Information(*)\% Privileged Utility
\Processor Information(*)\% Processor Utility
\Processor Information(*)\% Processor Performance
\Processor Information(*)\Idle Break Events/sec
\Processor Information(*)\Average Idle Time
\Processor Information(*)\Clock Interrupts/sec
\Processor Information(*)\Processor State Flags
\Processor Information(*)\% of Maximum Frequency
\Processor Information(*)\Processor Frequency
\Processor Information(*)\Parking Status
\Processor Information(*)\% Priority Time
\Processor Information(*)\C3 Transitions/sec
\Processor Information(*)\C2 Transitions/sec
\Processor Information(*)\C1 Transitions/sec
\Processor Information(*)\% C3 Time
\Processor Information(*)\% C2 Time
\Processor Information(*)\% C1 Time
\Processor Information(*)\% Idle Time
\Processor Information(*)\DPC Rate
\Processor Information(*)\DPCs Queued/sec
\Processor Information(*)\% Interrupt Time
\Processor Information(*)\% DPC Time
\Processor Information(*)\Interrupts/sec
\Processor Information(*)\% Privileged Time
\Processor Information(*)\% User Time
\Processor Information(*)\% Processor Time
```

### Memory

```
\Memory\Page Faults/sec
\Memory\Available Bytes
\Memory\Committed Bytes
\Memory\Commit Limit
\Memory\Write Copies/sec
\Memory\Transition Faults/sec
\Memory\Cache Faults/sec
\Memory\Demand Zero Faults/sec
\Memory\Pages/sec
\Memory\Pages Input/sec
\Memory\Page Reads/sec
\Memory\Pages Output/sec
\Memory\Pool Paged Bytes
\Memory\Pool Nonpaged Bytes
\Memory\Page Writes/sec
\Memory\Pool Paged Allocs
\Memory\Pool Nonpaged Allocs
\Memory\Free System Page Table Entries
\Memory\Cache Bytes
\Memory\Cache Bytes Peak
\Memory\Pool Paged Resident Bytes
\Memory\System Code Total Bytes
\Memory\System Code Resident Bytes
\Memory\System Driver Total Bytes
\Memory\System Driver Resident Bytes
\Memory\System Cache Resident Bytes
\Memory\% Committed Bytes In Use
\Memory\Available KBytes
\Memory\Available MBytes
\Memory\Transition Pages RePurposed/sec
\Memory\Free & Zero Page List Bytes
\Memory\Modified Page List Bytes
\Memory\Standby Cache Reserve Bytes
\Memory\Standby Cache Normal Priority Bytes
\Memory\Standby Cache Core Bytes
\Memory\Long-Term Average Standby Cache Lifetime (s)
```

### Logical Disk

```
\LogicalDisk(*)\% Free Space
\LogicalDisk(*)\Free Megabytes
\LogicalDisk(*)\Current Disk Queue Length
\LogicalDisk(*)\% Disk Time
\LogicalDisk(*)\Avg. Disk Queue Length
\LogicalDisk(*)\% Disk Read Time
\LogicalDisk(*)\Avg. Disk Read Queue Length
\LogicalDisk(*)\% Disk Write Time
\LogicalDisk(*)\Avg. Disk Write Queue Length
\LogicalDisk(*)\Avg. Disk sec/Transfer
\LogicalDisk(*)\Avg. Disk sec/Read
\LogicalDisk(*)\Avg. Disk sec/Write
\LogicalDisk(*)\Disk Transfers/sec
\LogicalDisk(*)\Disk Reads/sec
\LogicalDisk(*)\Disk Writes/sec
\LogicalDisk(*)\Disk Bytes/sec
\LogicalDisk(*)\Disk Read Bytes/sec
\LogicalDisk(*)\Disk Write Bytes/sec
\LogicalDisk(*)\Avg. Disk Bytes/Transfer
\LogicalDisk(*)\Avg. Disk Bytes/Read
\LogicalDisk(*)\Avg. Disk Bytes/Write
\LogicalDisk(*)\% Idle Time
\LogicalDisk(*)\Split IO/Sec
```

### Physical Disk

```
\PhysicalDisk(*)\Current Disk Queue Length
\PhysicalDisk(*)\% Disk Time
\PhysicalDisk(*)\Avg. Disk Queue Length
\PhysicalDisk(*)\% Disk Read Time
\PhysicalDisk(*)\Avg. Disk Read Queue Length
\PhysicalDisk(*)\% Disk Write Time
\PhysicalDisk(*)\Avg. Disk Write Queue Length
\PhysicalDisk(*)\Avg. Disk sec/Transfer
\PhysicalDisk(*)\Avg. Disk sec/Read
\PhysicalDisk(*)\Avg. Disk sec/Write
\PhysicalDisk(*)\Disk Transfers/sec
\PhysicalDisk(*)\Disk Reads/sec
\PhysicalDisk(*)\Disk Writes/sec
\PhysicalDisk(*)\Disk Bytes/sec
\PhysicalDisk(*)\Disk Read Bytes/sec
\PhysicalDisk(*)\Disk Write Bytes/sec
\PhysicalDisk(*)\Avg. Disk Bytes/Transfer
\PhysicalDisk(*)\Avg. Disk Bytes/Read
\PhysicalDisk(*)\Avg. Disk Bytes/Write
\PhysicalDisk(*)\% Idle Time
\PhysicalDisk(*)\Split IO/Sec
```

## Network counters

```
\Network QoS Policy(*)\Packets dropped/sec
\Network QoS Policy(*)\Packets dropped
\Network QoS Policy(*)\Bytes transmitted/sec
\Network QoS Policy(*)\Bytes transmitted
\Network QoS Policy(*)\Packets transmitted/sec
\Network QoS Policy(*)\Packets transmitted

\Per Processor Network Activity Cycles(*)\Interrupt DPC Latency Cycles/sec
\Per Processor Network Activity Cycles(*)\Stack Send Complete Cycles/sec
\Per Processor Network Activity Cycles(*)\Miniport RSS Indirection Table Change Cycles
\Per Processor Network Activity Cycles(*)\Build Scatter Gather Cycles/sec
\Per Processor Network Activity Cycles(*)\NDIS Send Complete Cycles/sec
\Per Processor Network Activity Cycles(*)\Miniport Send Cycles/sec
\Per Processor Network Activity Cycles(*)\NDIS Send Cycles/sec
\Per Processor Network Activity Cycles(*)\Miniport Return Packet Cycles/sec
\Per Processor Network Activity Cycles(*)\NDIS Return Packet Cycles/sec
\Per Processor Network Activity Cycles(*)\Stack Receive Indication Cycles/sec
\Per Processor Network Activity Cycles(*)\NDIS Receive Indication Cycles/sec
\Per Processor Network Activity Cycles(*)\Interrupt Cycles/sec
\Per Processor Network Activity Cycles(*)\Interrupt DPC Cycles/sec
\Per Processor Network Interface Card Activity(*)\Packets Coalesced/sec
\Per Processor Network Interface Card Activity(*)\DPCs Deferred/sec
\Per Processor Network Interface Card Activity(*)\Tcp Offload Send bytes/sec
\Per Processor Network Interface Card Activity(*)\Tcp Offload Receive bytes/sec
\Per Processor Network Interface Card Activity(*)\Tcp Offload Send Request Calls/sec
\Per Processor Network Interface Card Activity(*)\Tcp Offload Receive Indications/sec
\Per Processor Network Interface Card Activity(*)\Low Resource Received Packets/sec
\Per Processor Network Interface Card Activity(*)\Low Resource Receive Indications/sec
\Per Processor Network Interface Card Activity(*)\RSS Indirection Table Change Calls/sec
\Per Processor Network Interface Card Activity(*)\Build Scatter Gather List Calls/sec
\Per Processor Network Interface Card Activity(*)\Sent Complete Packets/sec
\Per Processor Network Interface Card Activity(*)\Sent Packets/sec
\Per Processor Network Interface Card Activity(*)\Send Complete Calls/sec
\Per Processor Network Interface Card Activity(*)\Send Request Calls/sec
\Per Processor Network Interface Card Activity(*)\DPCs Queued on Other CPUs/sec
\Per Processor Network Interface Card Activity(*)\Returned Packets/sec
\Per Processor Network Interface Card Activity(*)\Received Packets/sec
\Per Processor Network Interface Card Activity(*)\Return Packet Calls/sec
\Per Processor Network Interface Card Activity(*)\Receive Indications/sec
\Per Processor Network Interface Card Activity(*)\Interrupts/sec
\Per Processor Network Interface Card Activity(*)\DPCs Queued/sec

\Physical Network Interface Card Activity(*)\Low Power Transitions (Lifetime)
\Physical Network Interface Card Activity(*)\% Time Suspended (Lifetime)
\Physical Network Interface Card Activity(*)\% Time Suspended (Instantaneous)
\Physical Network Interface Card Activity(*)\Device Power State

\RemoteFX Network(*)\Total Received Bytes
\RemoteFX Network(*)\Total Sent Bytes
\RemoteFX Network(*)\Current UDP Bandwidth
\RemoteFX Network(*)\Current UDP RTT
\RemoteFX Network(*)\Base UDP RTT
\RemoteFX Network(*)\FEC Rate
\RemoteFX Network(*)\Retransmission Rate
\RemoteFX Network(*)\Loss Rate
\RemoteFX Network(*)\Sent Rate P3
\RemoteFX Network(*)\Sent Rate P2
\RemoteFX Network(*)\Sent Rate P1
\RemoteFX Network(*)\Sent Rate P0
\RemoteFX Network(*)\UDP Packets Sent/sec
\RemoteFX Network(*)\UDP Sent Rate
\RemoteFX Network(*)\TCP Sent Rate
\RemoteFX Network(*)\Total Sent Rate
\RemoteFX Network(*)\UDP Packets Received/sec
\RemoteFX Network(*)\UDP Received Rate
\RemoteFX Network(*)\TCP Received Rate
\RemoteFX Network(*)\Total Received Rate
\RemoteFX Network(*)\Current TCP Bandwidth
\RemoteFX Network(*)\Current TCP RTT
\RemoteFX Network(*)\Base TCP RTT

\.NET CLR Networking\Connections Established
\.NET CLR Networking\Bytes Received
\.NET CLR Networking\Bytes Sent
\.NET CLR Networking\Datagrams Received
\.NET CLR Networking\Datagrams Sent
\.NET CLR Networking 4.0.0.0(*)\Connections Established
\.NET CLR Networking 4.0.0.0(*)\Bytes Received
\.NET CLR Networking 4.0.0.0(*)\Bytes Sent
\.NET CLR Networking 4.0.0.0(*)\Datagrams Received
\.NET CLR Networking 4.0.0.0(*)\Datagrams Sent
\.NET CLR Networking 4.0.0.0(*)\HttpWebRequests Created/Sec
\.NET CLR Networking 4.0.0.0(*)\HttpWebRequests Average Lifetime
\.NET CLR Networking 4.0.0.0(*)\HttpWebRequests Queued/Sec
\.NET CLR Networking 4.0.0.0(*)\HttpWebRequests Average Queue Time
\.NET CLR Networking 4.0.0.0(*)\HttpWebRequests Aborted/Sec
\.NET CLR Networking 4.0.0.0(*)\HttpWebRequests Failed/Sec


\Network Interface(*)\Bytes Total/sec
\Network Interface(*)\Packets/sec
\Network Interface(*)\Packets Received/sec
\Network Interface(*)\Packets Sent/sec
\Network Interface(*)\Current Bandwidth
\Network Interface(*)\Bytes Received/sec
\Network Interface(*)\Packets Received Unicast/sec
\Network Interface(*)\Packets Received Non-Unicast/sec
\Network Interface(*)\Packets Received Discarded
\Network Interface(*)\Packets Received Errors
\Network Interface(*)\Packets Received Unknown
\Network Interface(*)\Bytes Sent/sec
\Network Interface(*)\Packets Sent Unicast/sec
\Network Interface(*)\Packets Sent Non-Unicast/sec
\Network Interface(*)\Packets Outbound Discarded
\Network Interface(*)\Packets Outbound Errors
\Network Interface(*)\Output Queue Length
\Network Interface(*)\Offloaded Connections
\Network Interface(*)\TCP Active RSC Connections
\Network Interface(*)\TCP RSC Coalesced Packets/sec
\Network Interface(*)\TCP RSC Exceptions/sec
\Network Interface(*)\TCP RSC Average Packet Size
\Network Adapter(*)\Bytes Total/sec
\Network Adapter(*)\Packets/sec
\Network Adapter(*)\Packets Received/sec
\Network Adapter(*)\Packets Sent/sec
\Network Adapter(*)\Current Bandwidth
\Network Adapter(*)\Bytes Received/sec
\Network Adapter(*)\Packets Received Unicast/sec
\Network Adapter(*)\Packets Received Non-Unicast/sec
\Network Adapter(*)\Packets Received Discarded
\Network Adapter(*)\Packets Received Errors
\Network Adapter(*)\Packets Received Unknown
\Network Adapter(*)\Bytes Sent/sec
\Network Adapter(*)\Packets Sent Unicast/sec
\Network Adapter(*)\Packets Sent Non-Unicast/sec
\Network Adapter(*)\Packets Outbound Discarded
\Network Adapter(*)\Packets Outbound Errors
\Network Adapter(*)\Output Queue Length
\Network Adapter(*)\Offloaded Connections
\Network Adapter(*)\TCP Active RSC Connections
\Network Adapter(*)\TCP RSC Coalesced Packets/sec
\Network Adapter(*)\TCP RSC Exceptions/sec
\Network Adapter(*)\TCP RSC Average Packet Size
```

# All CounterSets

This section lists all the available CounterSets and commands to get more information about a given counterset.

## Commands

### Generate a sorted list of all countersets
```
PS> Get-Counter -ListSet * |
>>   Sort-Object -Property CounterSetName |
>>   Format-Table CounterSetName, CounterSetType -AutoSize
```

### Get more information about a specifc counterset
```
PS> Get-Counter -ListSet "Processor Information"

CounterSetName     : Processor Information
MachineName        : .
CounterSetType     : MultiInstance
Description        : The Processor Information performance counter set consists of counters that measure aspects of
                     processor activity. The processor is the part of the computer that performs arithmetic and
                     logical computations, initiates operations on peripherals, and runs the threads of processes. A
                     computer can have multiple processors. On some computers, processors are organized in NUMA nodes
                     that share hardware resources such as physical memory. The Processor Information counter set
                     represents each processor as a pair of numbers, where the first number is the NUMA node number
                     and the second number is the zero-based index of the processor within that NUMA node. If the
                     computer does not use NUMA nodes, the first number is zero.
Paths              : {\Processor Information(*)\Performance Limit Flags, \Processor Information(*)\% Performance
                     Limit, \Processor Information(*)\% Privileged Utility, \Processor Information(*)\% Processor
                     Utility...}
PathsWithInstances : {\Processor Information(0,0)\Performance Limit Flags, \Processor Information(0,1)\Performance
                     Limit Flags, \Processor Information(0,2)\Performance Limit Flags, \Processor
                     Information(0,3)\Performance Limit Flags...}
Counter            : {\Processor Information(*)\Performance Limit Flags, \Processor Information(*)\% Performance
                     Limit, \Processor Information(*)\% Privileged Utility, \Processor Information(*)\% Processor
                     Utility...}
```

### Get more information about the PATHS within a specifc counterset

```
PS> (Get-Counter -ListSet "Processor Information").paths

\Processor Information(*)\Performance Limit Flags
\Processor Information(*)\% Performance Limit
\Processor Information(*)\% Privileged Utility
\Processor Information(*)\% Processor Utility
\Processor Information(*)\% Processor Performance
\Processor Information(*)\Idle Break Events/sec
\Processor Information(*)\Average Idle Time
\Processor Information(*)\Clock Interrupts/sec
\Processor Information(*)\Processor State Flags
\Processor Information(*)\% of Maximum Frequency
\Processor Information(*)\Processor Frequency
\Processor Information(*)\Parking Status
\Processor Information(*)\% Priority Time
\Processor Information(*)\C3 Transitions/sec
\Processor Information(*)\C2 Transitions/sec
\Processor Information(*)\C1 Transitions/sec
\Processor Information(*)\% C3 Time
\Processor Information(*)\% C2 Time
\Processor Information(*)\% C1 Time
\Processor Information(*)\% Idle Time
\Processor Information(*)\DPC Rate
\Processor Information(*)\DPCs Queued/sec
\Processor Information(*)\% Interrupt Time
\Processor Information(*)\% DPC Time
\Processor Information(*)\Interrupts/sec
\Processor Information(*)\% Privileged Time
\Processor Information(*)\% User Time
\Processor Information(*)\% Processor Time
```


## Output - List of All CounterSets

All of the above CounterSets are a part of this comprehensive list.

```
CounterSetName                                         CounterSetType
--------------                                         --------------
.NET CLR Data                                          SingleInstance
.NET CLR Exceptions                                     MultiInstance
.NET CLR Interop                                        MultiInstance
.NET CLR Jit                                            MultiInstance
.NET CLR Loading                                        MultiInstance
.NET CLR LocksAndThreads                                MultiInstance
.NET CLR Memory                                         MultiInstance
.NET CLR Networking                                    SingleInstance
.NET CLR Networking 4.0.0.0                            SingleInstance
.NET CLR Remoting                                       MultiInstance
.NET CLR Security                                       MultiInstance
.NET Data Provider for Oracle                          SingleInstance
.NET Data Provider for SqlServer                       SingleInstance
.NET Memory Cache 4.0                                  SingleInstance
{115b92b4-7191-491a-a9b5-93c8e9fb641b}                 SingleInstance
{7d937e49-cfd5-438f-af4f-b3047d90a5c3}                 SingleInstance
{f3e82f6e-9df4-425d-a5d5-3a9832005b16}                 SingleInstance
AppV Client Streamed Data Percentage                   SingleInstance
ASP.NET                                                SingleInstance
ASP.NET Applications                                   SingleInstance
ASP.NET Apps v4.0.30319                                SingleInstance
ASP.NET State Service                                  SingleInstance
ASP.NET v4.0.30319                                     SingleInstance
Authorization Manager Applications                     SingleInstance
BITS Net Utilization                                   SingleInstance
Bluetooth Device                                       SingleInstance
Bluetooth Radio                                        SingleInstance
Browser                                                SingleInstance
Cache                                                  SingleInstance
Client Side Caching                                    SingleInstance
Database                                                MultiInstance
Database ==> Databases                                  MultiInstance
Database ==> Instances                                  MultiInstance
Database ==> TableClasses                               MultiInstance
Distributed Transaction Coordinator                    SingleInstance
DNS64 Global                                           SingleInstance
ENA Packets Shaping                                     MultiInstance
Energy Meter                                           SingleInstance
Event Tracing for Windows                              SingleInstance
Event Tracing for Windows Session                       MultiInstance
FileSystem Disk Activity                               SingleInstance
Generic IKEv1, AuthIP, and IKEv2                       SingleInstance
GPU Adapter Memory                                     SingleInstance
GPU Engine                                              MultiInstance
GPU Local Adapter Memory                               SingleInstance
GPU Non Local Adapter Memory                           SingleInstance
GPU Process Memory                                      MultiInstance
HTTP Service                                           SingleInstance
HTTP Service Request Queues                            SingleInstance
HTTP Service Url Groups                                SingleInstance
Hyper-V Dynamic Memory Integration Service             SingleInstance
Hyper-V Hypervisor                                     SingleInstance
Hyper-V Hypervisor Logical Processor                   SingleInstance
Hyper-V Hypervisor Root Partition                      SingleInstance
Hyper-V Hypervisor Root Virtual Processor              SingleInstance
Hyper-V Virtual Machine Bus Pipes                      SingleInstance
ICMP                                                   SingleInstance
ICMPv6                                                 SingleInstance
IPHTTPS Global                                         SingleInstance
IPHTTPS Session                                        SingleInstance
IPsec AuthIP IPv4                                      SingleInstance
IPsec AuthIP IPv6                                      SingleInstance
IPsec Connections                                      SingleInstance
IPsec DoS Protection                                   SingleInstance
IPsec Driver                                           SingleInstance
IPsec IKEv1 IPv4                                       SingleInstance
IPsec IKEv1 IPv6                                       SingleInstance
IPsec IKEv2 IPv4                                       SingleInstance
IPsec IKEv2 IPv6                                       SingleInstance
IPv4                                                   SingleInstance
IPv6                                                   SingleInstance
Job Object                                             SingleInstance
Job Object Details                                     SingleInstance
KPSSVC                                                 SingleInstance
LogicalDisk                                             MultiInstance
Memory                                                 SingleInstance
Microsoft Winsock BSP                                  SingleInstance
MSDTC Bridge 4.0.0.0                                   SingleInstance
NBT Connection                                         SingleInstance
Netlogon                                               SingleInstance
Network Adapter                                         MultiInstance
Network Interface                                      SingleInstance
Network QoS Policy                                     SingleInstance
NUMA Node Memory                                        MultiInstance
Objects                                                SingleInstance
Offline Files                                          SingleInstance
Pacer Flow                                             SingleInstance
Pacer Pipe                                             SingleInstance
PacketDirect EC Utilization                            SingleInstance
PacketDirect Queue Depth                               SingleInstance
PacketDirect Receive Counters                          SingleInstance
PacketDirect Receive Filters                           SingleInstance
PacketDirect Transmit Counters                         SingleInstance
Paging File                                             MultiInstance
Per Processor Network Activity Cycles                   MultiInstance
Per Processor Network Interface Card Activity           MultiInstance
Physical Network Interface Card Activity               SingleInstance
PhysicalDisk                                            MultiInstance
Power Meter                                            SingleInstance
PowerShell Workflow                                    SingleInstance
Print Queue                                             MultiInstance
Process                                                 MultiInstance
Processor                                               MultiInstance
Processor Information                                   MultiInstance
RAS                                                    SingleInstance
RAS Port                                               SingleInstance
RAS Total                                              SingleInstance
RDMA Activity                                          SingleInstance
Redirector                                             SingleInstance
ReFS                                                   SingleInstance
Remote Desktop Connection Broker Redirector Counterset SingleInstance
RemoteFX Graphics                                      SingleInstance
RemoteFX Network                                       SingleInstance
Security Per-Process Statistics                         MultiInstance
Security System-Wide Statistics                        SingleInstance
Server                                                 SingleInstance
Server Work Queues                                      MultiInstance
ServiceModelEndpoint 4.0.0.0                           SingleInstance
ServiceModelOperation 4.0.0.0                          SingleInstance
ServiceModelService 4.0.0.0                            SingleInstance
SMB Client Shares                                      SingleInstance
SMB Direct Connection                                  SingleInstance
SMB Server                                             SingleInstance
SMB Server Sessions                                    SingleInstance
SMB Server Shares                                       MultiInstance
SMSvcHost 4.0.0.0                                      SingleInstance
Storage Management WSP Spaces Runtime                  SingleInstance
Storage Spaces Drt                                     SingleInstance
Storage Spaces Tier                                    SingleInstance
Storage Spaces Virtual Disk                            SingleInstance
Storage Spaces Write Cache                             SingleInstance
Synchronization                                         MultiInstance
SynchronizationNuma                                     MultiInstance
System                                                 SingleInstance
TCPIP Performance Diagnostics                          SingleInstance
TCPIP Performance Diagnostics (Per-CPU)                 MultiInstance
TCPv4                                                  SingleInstance
TCPv6                                                  SingleInstance
Telephony                                              SingleInstance
Teredo Client                                          SingleInstance
Teredo Relay                                           SingleInstance
Teredo Server                                          SingleInstance
Terminal Services                                      SingleInstance
Terminal Services Session                               MultiInstance
Thermal Zone Information                               SingleInstance
Thread                                                  MultiInstance
UDPv4                                                  SingleInstance
UDPv6                                                  SingleInstance
USB                                                    SingleInstance
User Input Delay per Process                            MultiInstance
User Input Delay per Session                            MultiInstance
WF (System.Workflow) 4.0.0.0                           SingleInstance
WFP                                                    SingleInstance
WFP Classify                                           SingleInstance
WFP Reauthorization                                    SingleInstance
WFPv4                                                  SingleInstance
WFPv6                                                  SingleInstance
Windows Media Player Metadata                          SingleInstance
Windows Time Service                                   SingleInstance
WinNAT                                                 SingleInstance
WinNAT ICMP                                            SingleInstance
WinNAT Instance                                        SingleInstance
WinNAT TCP                                             SingleInstance
WinNAT UDP                                             SingleInstance
WMI Objects                                            SingleInstance
WorkflowServiceHost 4.0.0.0                            SingleInstance
WSMan Quota Statistics                                 SingleInstance
XHCI CommonBuffer                                      SingleInstance
XHCI Interrupter                                       SingleInstance
XHCI TransferRing                                      SingleInstance
```

