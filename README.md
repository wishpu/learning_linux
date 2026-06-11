# learning_linux

This file will record my learning process of linux and the following items are sorted by glm-5.1.
I will learn according to the following sequence and update my learning material.

## Linux 内核模块学习列表

### 1. 进程调度 (Process Scheduling)

- [ ] **调度器演进**
  - [ ] O(1) 调度器设计原理与局限
  - [ ] CFS (Completely Fair Scheduler) 核心算法：虚拟时间 (vruntime) 与红黑树
  - [ ] CFS 的权重体系：prio_to_weight 表与 nice 值映射
  - [ ] 实时调度类：SCHED_FIFO / SCHED_RR / SCHED_DEADLINE
  - [ ] Deadline 调度器的 EDF 算法实现
- [ ] **调度核心流程**
  - [ ] schedule() 主入口与 context_switch 机制
  - [ ] pick_next_task 与进程选择策略
  - [ ] 时钟 tick 与调度时机 (sched_tick / preempt)
  - [ ] 主动调度与被动调度 (voluntary / involuntary preempt)
  - [ ] 调度延迟 (sched_latency) 与最小粒度 (min_granularity)
- [ ] **负载均衡与多核调度**
  - [ ] 调度域 (sched_domain) 与调度组 (sched_group) 层级结构
  - [ ] load_balance 与主动均衡 (active balance)
  - [ ] NUMA 感知调度与 numa_balancing
  - [ ] CPU 亲和性与 cpuset 机制
  - [ ] 运行队列 (cfs_rq / rt_rq / dl_rq) 数据结构
- [ ] **组调度与公平性**
  - [ ] CFS 组调度 (task_group / cfs_rq 嵌套)
  - [ ] CPU bandwidth 控制 (cfs_quota / cfs_period)
  - [ ] autogroup 机制 (session 级公平)
- [ ] **抢占与并发**
  - [ ] 内核抢占模型 (PREEMPT_NONE / PREEMPT_VOLUNTARY / PREEMPT / PREEMPT_RT)
  - [ ] PREEMPT_RT 实时内核补丁原理
  - [ ] 抢占点与 preempt_count 计数器
  - [ ] voluntary preempt (might_sleep / cond_resched)

### 2. 内存管理 (Memory Management)

- [ ] **物理内存管理**
  - [ ] 内存节点 (pglist_data / node) 与 UMA / NUMA 架构
  - [ ] 内存区域 (zone: DMA / DMA32 / NORMAL / HIGHMEM / MOVABLE)
  - [ ] 伙伴系统 (buddy allocator)：空闲页组织、分配与合并算法
  - [ ] 伙伴系统的迁移类型 (migrate types: UNMOVABLE / MOVABLE / RECLAIMABLE)
  - [ ] per-cpu 页帧缓存 (pcp: per-cpu pageset) 快速分配路径
  - [ ] slab / slob / slub 分配器原理与对比
  - [ ] slub 数据结构：kmem_cache / kmalloc_info / partial 链表
  - [ ] kmalloc 分配路径与 size 索引表
  - [ ] 页帧结构 (struct page) 与 page flags 体系
  - [ ] 连续内存分配器 (CMA: Contiguous Memory Allocator)
- [ ] **虚拟内存与地址空间**
  - [ ] 进程地址空间布局 (mm_struct / 代码段 / 数据段 / 堆 / 栈 / mmap 区)
  - [ ] VMA (vm_area_struct) 的组织、查找与合并 (红黑树 + 链表)
  - [ ] mmap 系统调用流程与 VMA 创建
  - [ ] 私有映射与共享映射 (MAP_PRIVATE / MAP_SHARED)
  - [ ] brk / sbrk 堆扩展与栈自动增长
  - [ ] 64 位地址空间布局 (canonical address / hole)
  - [ ] 内核地址空间布局 (直接映射区 / vmalloc 区 / 固定映射 / kmap)
- [ ] **页表与地址翻译**
  - [ ] x86_64 四级 / 五级页表 (PGD / P4D / PUD / PMD / PTE)
  - [ ] 页表项标志位 (Present / Write / User / Accessed / Dirty / Global)
  - [ ] TLB 结构、刷新策略与延迟刷新 (lazy TLB / shootdown)
  - [ ] 页表内存分配与 pgtable 页面缓存
  - [ ] 大页支持 (HugeTLB: 2MB / 1GB 页)
  - [ ] 透明大页 (THP: Transparent Huge Pages) 合并与拆分
  - [ ] __get_user_pages 与 page walk 机制
- [ ] **页面置换与回收**
  - [ ] kswapd 后台回收线程与水位线 (min / low / high)
  - [ ] LRU 链表体系 (active / inactive / unevictable / anon / file)
  - [ ] LRU 年龄计算与 PG_refered / PG_active 双缓冲策略
  - [ ] 页面回收路径：shrink_node / shrink_folio_list
  - [ ] 匿名页与文件页的回收策略差异
  - [ ] swap 系统与 swap cache
  - [ ] zswap / zram 压缩存储机制
  - [ ] OOM killer 机制与 oom_score_adj
- [ ] **缺页异常处理**
  - [ ] x86 do_page_fault 入口与错误码解析
  - [ ] 匿名页缺页 (do_anonymous_page / cow_zero_page)
  - [ ] 文件映射缺页 (do_fault / readpage 回调)
  - [ ] 写时复制 (COW) 流程与 folio_dup
  - [ ] 用户态缺页与内核态缺页的不同处理路径
  - [ ] 页错误统计与 /proc/vmstat
- [ ] **内存映射与 DMA**
  - [ ] vmalloc 与不连续内存映射
  - [ ] ioremap 与设备内存映射
  - [ ] DMA 映射：一致性映射 (dma_alloc_coherent) 与流式映射 (dma_map_single/page)
  - [ ] IOMMU 与设备地址翻译
  - [ ] swiotlb (软件 IO TLB) 溢出缓冲

### 3. 存储 (Storage: File System & Block Layer)

- [ ] **VFS 虚拟文件系统**
  - [ ] VFS 四大核心对象：super_block / inode / dentry / file
  - [ ] super_block 操作集 (super_operations) 与挂载流程
  - [ ] inode 操作集 (inode_operations) 与文件元数据
  - [ ] dentry 缓存 (dcache) 与路径解析 (path_lookup)
  - [ ] dentry 状态与回收 (dentry LRU / shrink_dcache)
  - [ ] file 结构与文件操作集 (file_operations)
  - [ ] mount 结构与挂载树 (vfsmount / mount_hashtable)
  - [ ] bind mount / pivot_root / namespace 挂载
- [ ] **文件 I/O 全路径**
  - [ ] read() 系统调用 → vfs_read → generic_file_read_iter
  - [ ] write() 系统调用 → vfs_write → generic_file_write_iter
  - [ ] Direct I/O 路径 (O_DIRECT)：绕过页缓存
  - [ ] Buffered I/O 与页缓存交互
  - [ ] AIO / io_uring 异步 I/O 机制
  - [ ] io_uring 设计原理：共享环形缓冲区、SQ / CQ / SQE / CQE
  - [ ] io_uring 与 epoll 集成
  - [ ] fadvise 与 posix_fadvise 预读策略控制
  - [ ] 文件预读算法 (readahead 窗口 / async sync readahead)
- [ ] **页缓存与写回**
  - [ ] address_space 与页缓存树 (xarray / radix tree)
  - [ ] 页缓存查找 (find_get_pages) 与缓存命中
  - [ ] 脏页跟踪与 writeback 结构 (wb)
  - [ ] writeback 线程 (flusher / kworker) 与唤醒策略
  - [ ] 脏页回写触发条件：时间 / 内存压力 / sync
  - [ ] PDflush / bdi_writeback 工作机制
  - [ ] 页缓存回收与脏页跳过策略
- [ ] **ext4 文件系统**
  - [ ] ext4 物理结构：超级块 / GDT / 块位图 / inode 位图 / inode 表 / 数据块
  - [ ] ext4 extent 树结构 (extent tree / ext4_extent_idx / ext4_extent)
  - [ ] ext4 多块分配器 (mballoc) 与延迟分配 (delalloc)
  - [ ] ext4 日志系统 (jbd2)：事务 / 日志模式 (journal / ordered / writeback)
  - [ ] jbd2 事务生命周期：开事务 / 提交 / 刷新 / 完成
  - [ ] ext4 大文件支持 (huge_file / 64-bit block nr)
  - [ ] ext4 内联数据 (inline_data) 与小文件优化
  - [ ] ext4 快速 commit (fast commit) 机制
- [ ] **其他文件系统**
  - [ ] xfs：B+tree 空间管理、延迟分配与日志
  - [ ] btrfs：COW B-tree、子卷、快照与校验
  - [ ] tmpfs / ramfs：内存文件系统
  - [ ] procfs：进程信息导出与 /proc 结构
  - [ ] sysfs：kobject 导出与设备拓扑
  - [ ] debugfs：内核调试信息导出
  - [ ] fuse：用户态文件系统框架
  - [ ] overlayfs：容器镜像层叠文件系统
  - [ ] NFS / CIFS 分布式文件系统概述
- [ ] **块设备层**
  - [ ] bio 结构与请求构造 (bio_vec / bvec)
  - [ ] 请求队列 (request_queue) 与 blk-mq 框架
  - [ ] blk-mq：硬件队列 / 软件队列 / tag 标签管理
  - [ ] I/O 调度器：mq-deadline / bfq / kyber / none
  - [ ] bfq 预算公平调度原理与带宽分配
  - [ ] blk-mq 轮询 (io_poll) 与 IRQ 模式切换
  - [ ] 请求合并 (plug / unplug / merge) 与排序
  - [ ] I/O 优先级与 cgroup I/O 控制 (blk-iocost / blk-throttle)
  - [ ] NVMe 驱动与 blk-mq 多队列映射
  - [ ] 块设备分区与通用块层操作

### 4. eBPF (Extended Berkeley Packet Filter)

- [ ] **eBPF 基础架构**
  - [ ] eBPF 指令集架构 (ISA)：64-bit 寄存器 / 指令格式 / ALU / 内存 / 跳转
  - [ ] BPF 程序类型 (prog type)：socket filter / kprobe / tracepoint / xdp / cgroup / lsm 等
  - [ ] BPF map 类型：hash / array / perf_event / ringbuf / stack_trace / lru_hash 等
  - [ ] BPF verifier 验证流程：CFG 构建 / 死代码消除 / 路径探索 / 深度限制
  - [ ] verifier 安全检查：指针范围 / 内存访问 / 循环上限 / helper 函数白名单
  - [ ] BPF 程序加载流程：bpf() 系统调用 / ELF 解析 / 重定位 / JIT 编译
- [ ] **eBPF 运行时与 JIT**
  - [ ] x86_64 JIT 编译原理与指令映射
  - [ ] BPF interpreter 执行流程
  - [ ] BPF trampoline 与 fentry / fexit 尾调用优化
  - [ ] BPF subprog 与函数调用 (bpf_call)
  - [ ] tail call 机制与程序链式调用
  - [ ] BPF 程序 attach / detach / link 生命周期
  - [ ] BPF cookie 与 attach 点标识
- [ ] **eBPF 网络应用**
  - [ ] XDP (eXpress Data Path)：早期数据包处理与驱动层 hook
  - [ ] XDP 动作：PASS / DROP / TX / REDIRECT / ABORTED
  - [ ] tc BPF：流量控制与分类器 (cls_bpf)
  - [ ] cgroup BPF：socket 创建 / connect / sendmsg / recvmsg hook
  - [ ] BPF sk_msg / sk_skb：socket 数据流过滤与重定向
  - [ ] BPF 网络策略与 L4LB (Cilium / Katran)
- [ ] **eBPF 可观测性应用**
  - [ ] kprobe / kretprobe BPF 程序
  - [ ] tracepoint BPF 程序与内核 tracepoint 数据导出
  - [ ] perf_event BPF 程序与性能采样
  - [ ] BPF stack trace 与调用栈采集
  - [ ] BTF (BPF Type Format)：内核类型信息导出与 CO-RE
  - [ ] CO-RE (Compile Once - Run Everywhere) 原理与 vmlinux.h
  - [ ] BPF ringbuf 与 perf buffer 数据传输
  - [ ] bpftrace / BCC 工具链
- [ ] **eBPF 安全应用**
  - [ ] BPF LSM 程序类型：文件 / 网络 / 进程 / mount 安全 hook
  - [ ] BPF 安全策略动态加载与审计
  - [ ] seccomp BPF：系统调用过滤与沙箱

### 5. 虚拟化与容器 (Virtualization & Containerization)

- [ ] **硬件虚拟化基础**
  - [ ] x86 VT-x / VT-d 硬件虚拟化扩展
  - [ ] VMX 操作模式：VM Entry / VM Exit / VMCS 结构
  - [ ] EPT (Extended Page Tables)：二级地址翻译
  - [ ] VPID (Virtual Processor ID)：虚拟 TLB 标识
  - [ ] VT-d / IOMMU：设备直通与 DMA 保护
  - [ ] AMD SVM (Secure Virtual Machine) 对应机制
- [ ] **KVM 内核虚拟化**
  - [ ] KVM 架构：kvm / kvm_vcpu / kvm_mmio 抽象
  - [ ] KVM 用户态接口：KVM_CREATE_VM / KVM_CREATE_VCPU / KVM_RUN ioctl
  - [ ] KVM 虚拟机生命周期与管理
  - [ ] KVM 内存管理：memslot / gfn_to_pfn / EPT 映射
  - [ ] KVM 中断虚拟化：虚拟 LAPIC / IRQ chip / posted interrupt
  - [ ] KVM 时钟虚拟化：kvmclock / TSC offset / pvclock
  - [ ] KVM 设备虚拟化：Virtio 框架 / vhost / ioeventfd / irqfd
  - [ ] KVM dirty page log 与 Live Migration
- [ ] **Virtio 虚拟设备**
  - [ ] Virtio 规范：设备 / 驱动 / transport (legacy / modern)
  - [ ] Virtqueue 结构：avail ring / used ring / descriptor table
  - [ ] Virtio 数据流：buffer 添加 / 通知 / 处理 / 回收
  - [ ] Vhost 内核加速与 vhost_net / vhost_vsock
  - [ ] Virtio-blk / virtio-net / virtio-scsi / virtio-gpu
  - [ ] Virtio 1.0 modern transport 与 packed ring
- [ ] **容器基础机制**
  - [ ] namespace 体系：PID / NET / MNT / IPC / USER / UTS / CGROUP / TIME
  - [ ] namespace 创建：unshare / setns 系统调用
  - [ ] cgroups v1 与 v2 架构对比
  - [ ] cgroups 控制器：cpu / cpuacct / cpuset / memory / blkio / pids / devices / freezer / net_cls
  - [ ] cgroup 进程迁移与层级管理
  - [ ] cgroup eBPF 程序 attach 与网络策略
- [ ] **容器运行时与安全**
  - [ ] runc / containerd 架构与 OCI 规范
  - [ ] seccomp 与容器系统调用过滤
  - [ ] LSM / AppArmor / SELinux 与容器安全策略
  - [ ] rootless container 与 user namespace 映射
  - [ ] rlimit 与容器资源限制
  - [ ] 容器逃逸攻防与安全加固

### 6. 进程管理 (Process Management)

- [ ] **进程与线程抽象**
  - [ ] task_struct 结构与关键字段解析 (state / pid / tgid / mm / signals / sched)
  - [ ] 内核线程 (kthread) 与用户线程的差异
  - [ ] 线程组与进程组 (tgid / pgid / session)
  - [ ] 进程树与父子关系 (parent / children / sibling 链表)
  - [ ] 进程资源限制 (rlimit) 与统计 (rusage)
  - [ ] PID 命名空间与进程 ID 映射
- [ ] **进程创建与销毁**
  - [ ] fork 系统调用全路径：copy_process / dup_mm / dup_fd / copy_signal
  - [ ] clone 系统调用与共享选项 (CLONE_VM / CLONE_FS / CLONE_FILES / CLONE_SIGHAND)
  - [ ] vfork 机制与 child_wait 等待优化
  - [ ] exec 系统调用：do_execve / binprm / search_binary_handler
  - [ ] exit 与 do_exit 流程：资源释放 / 信号通知 / zombie 状态
  - [ ] wait / waitpid 与子进程回收
  - [ ] OOM 与进程强制终止
- [ ] **进程间通信 (IPC)**
  - [ ] pipe / fifo 票据管道实现与环形缓冲区
  - [ ] signal 信号体系：发送 / 接收 / 处理 / sigaction
  - [ ] 信号递送路径：kill / tkill / rt_sigqueueinfo / dequeue_signal
  - [ ] 实时信号与标准信号的区别
  - [ ] POSIX 消息队列 (mqueue)：优先级队列与通知机制
  - [ ] 共享内存 (shm / mmap)：映射 / 同步 / POSIX shm
  - [ ] System V IPC：semaphore / msg / shm 持久化语义
  - [ ] eventfd / signalfd / timerfd 通知机制
- [ ] **进程状态与生命周期**
  - [ ] 进程状态转换：TASK_RUNNING / TASK_INTERRUPTIBLE / TASK_UNINTERRUPTIBLE / TASK_STOPPED / TASK_TRACED / EXIT_ZOMBIE / EXIT_DEAD
  - [ ] 等待队列 (wait_queue) 与唤醒 (wake_up) 机制
  - [ ] ptrace 调试跟踪与信号拦截
  - [ ] core dump 生成与 ELF 格式
  - [ ] coredump 过滤与大小限制

### 7. 网络子系统 (Networking)

- [ ] **网络协议栈架构**
  - [ ] 协议栈分层：socket 层 / TCP/UDP 层 / IP 层 / 链路层 / 驔动层
  - [ ] sk_buff 结构与数据包生命周期 (alloc / clone / copy / free)
  - [ ] skb 数据组织：head / data / tail / end / frag_list / frag_page
  - [ ] 网络命名空间 (net_namespace) 与协议栈隔离
  - [ ] /proc/net 与 /sys/class/net 信息导出
- [ ] **socket 层**
  - [ ] socket 系统调用族：socket / bind / listen / accept / connect / send / recv
  - [ ] sock 结构与 socket 的关系与层次
  - [ ] socket 状态机与连接生命周期
  - [ ] socket 缓冲区管理：sndbuf / rcvbuf / sk_wmem_queued / sk_rmem_alloc
  - [ ] epoll 机制：epoll_create / epoll_ctl / epoll_wait 与红黑树 + 链表
  - [ ] epoll LT (水平触发) 与 ET (边缘触发) 模式
  - [ ] select / poll 与 epoll 的实现对比
  - [ ] multi-shot accept 与 epoll batch 优化
- [ ] **TCP 实现**
  - [ ] TCP 连接建立：三次握手 / SYN 队列 / accept 队列 / syncookie
  - [ ] TCP 连接关闭：四次挥手 / FIN_WAIT / TIME_WAIT / LINGER 选项
  - [ ] TCP 拥塞控制算法：cubic / reno / bbr / dctcp / hybla
  - [ ] BBR 算法原理：bandwidth probing / RTT tracking / pacing rate
  - [ ] TCP 流量控制：滑动窗口 / 缓窗 / cwnd / 拥塞避免
  - [ ] TCP 快速重传 / 快速恢复 / SACK / FACK / RACK
  - [ ] TCP keepalive / TCP_DEFER_ACCEPT / SO_REUSEPORT
  - [ ] TCP 超时与 RTO 计算 (RTT / SRTT / RTTVAR)
  - [ ] TCP MD5 / TLS offload / kTLS
- [ ] **UDP 与 IP 层**
  - [ ] UDP 数据包收发路径与 checksum
  - [ ] UDP GSO / GRO 批量收发优化
  - [ ] IP 路由查找：fib_table / trie / route cache
  - [ ] IP 分片与重组 (fragment / defrag)
  - [ ] ICMP 协议处理与错误报文
  - [ ] IP 多播与 IGMP
  - [ ] IPsec / XFRM 框架与加密变换
- [ ] **netfilter 与包过滤**
  - [ ] netfilter hook 点：PRE_ROUTING / LOCAL_IN / FORWARD / LOCAL_OUT / POST_ROUTING
  - [ ] iptables 表与链：filter / nat / mangle / raw / security
  - [ ] nftables 架构：表达式 / 集合 / map / chain / 规则
  - [ ] conntrack 连接跟踪：状态机 / nat / tuple / expect
  - [ ] conntrack 扩展与超时管理
  - [ ] NFQUEUE 与用户态包处理
- [ ] **路由与邻居子系统**
  - [ ] FIB (Forwarding Information Base) 结构与路由表组织
  - [ ] 路由策略数据库 (RPDB) 与 rule / table / default route
  - [ ] 多路径路由 (multipath) 与 ECMP
  - [ ] ARP / NDP 邻居发现与邻居表 (neigh_table)
  - [ ] 邻居状态机：INCOMPLETE / REACHABLE / STALE / DELAY / PROBE / FAILED
  - [ ] 路由缓存与 nexthop 对象

### 8. 内核同步与锁 (Synchronization & Locking)

- [ ] **自旋锁**
  - [ ] spinlock 实现原理：原子变量 + 循环等待
  - [ ] spin_lock / spin_unlock / spin_lock_irq / spin_lock_irqsave 语义
  - [ ] ticket spinlock 与 MCS spinlock / queued spinlock (qspinlock)
  - [ ] 自旋锁与中断安全的组合使用
  - [ ] 自旋锁开销与持有时间约束
- [ ] **互斥锁与信号量**
  - [ ] mutex 实现：owner field / optimistic spinning / wait_list
  - [ ] mutex 与 spinlock 选择策略
  - [ ] rt_mutex 与优先级继承 (PI: Priority Inheritance)
  - [ ] semaphore 计数信号量与现代内核中的使用场景
  - [ ] rw_semaphore (读写信号量) 与 down_read / down_write
  - [ ] per-cpu rw_semaphore 优化
- [ ] **RCU (Read-Copy-Update)**
  - [ ] RCU 核心原理：读者无锁 + 写者延迟释放
  - [ ] RCU grace period 与回调机制 (call_rcu / rcu_barrier)
  - [ ] RCU 宽限期检测：GP kthread / funnel locking / expedited
  - [ ] SRCU (Sleepable RCU) 与可睡眠场景
  - [ ] RCU-sched / RCU-bh / Tasks RCU 分类
  - [ ] RCU 指针更新：rcu_assign_pointer / rcu_dereference
  - [ ] RCU 在 slab / dentry / 调度器中的典型应用
- [ ] **原子操作与内存屏障**
  - [ ] atomic_t / atomic64_t 操作与内核 API
  - [ ] refcount_t 与引用计数溢出保护
  - [ ] 内存屏障语义：smp_mb / smp_rmb / smp_wmb / smp_store_release / smp_load_acquire
  - [ ] READ_ONCE / WRITE_ONCE 与 volatile 语义
  - [ ] lockless 数据结构设计原则
- [ ] **死锁与锁调试**
  - [ ] lockdep 锁依赖图与死锁检测原理
  - [ ] lockdep class / subclass 与 irq context 标记
  - [ ] 常见死锁模式：ABBA / self-deadlock / irq deadlock
  - [ ] mutex debug / spinlock debug 选项
  - [ ] lock_stat 与锁竞争统计

### 9. 设备驱动 (Device Drivers)

- [ ] **字符设备驱动**
  - [ ] 字符设备注册：register_chrdev / cdev_add / alloc_chrdev_region
  - [ ] file_operations 与用户态接口实现
  - [ ] misc_register 与简易字符设备
  - [ ] 设备节点与 mknod / udev / devtmpfs 自动创建
  - [ ] ioctl 系统调用与命令编码 (\_IO / \_IOR / \_IOW / \_IOWR)
- [ ] **设备模型**
  - [ ] kobject / kset / ktype 层次结构
  - [ ] device / driver / bus_type 关系与匹配流程
  - [ ] platform_driver 与 platform_device
  - [ ] device tree 解析与 of_* API
  - [ ] sysfs 文件导出与 attribute / bin_attribute
  - [ ] device_register / driver_register 与 probe 流程
- [ ] **中断处理**
  - [ ] 硬中断 (hard IRQ) 与中断描述符 (irq_desc / irq_data)
  - [ ] 中断流控：handle_level_irq / handle_edge_irq / handle_fasteoi_irq
  - [ ] IRQ affinity 与多核中断分发
  - [ ] softirq 机制与 9 大软中断类型
  - [ ] tasklet 与 softirq 的关系与调度
  - [ ] workqueue 机制：system wq / WQ_UNBOUND / WQ_HIGHPRI / cmwq
  - [ ] threaded IRQ 与中断线程化
  - [ ] MSI / MSI-X 与多向量中断
- [ ] **DMA 与内存映射 I/O**
  - [ ] MMIO：ioremap / readl / writel / memcpy_toio
  - [ ] PIO：inb / outb 与 x86 端口 I/O
  - [ ] DMA coherent mapping 与 streaming mapping
  - [ ] DMA mask / bounce buffer 与地址限制
  - [ ] IOMMU 集成与设备地址映射

### 10. 系统调用与内核入口 (System Calls & Kernel Entry)

- [ ] **系统调用机制**
  - [ ] x86_64 syscall 指令与 MSR_LSTAR / SYSENTER
  - [ ] entry_SYSCALL_64_fastpath / entry_SYSCALL_64_slowpath
  - [ ] 系统调用表 (sys_call_table) 与编号映射
  - [ ] 参数传递：寄存器约定 (rdi / rsi / rdx / r10 / r8 / r9)
  - [ ] 系统调用返回与 pt_regs 保存恢复
  - [ ] sysret vs iret 返回路径与安全考量
- [ ] **中断与异常入口**
  - [ ] IDT (Interrupt Descriptor Table) 与门描述符
  - [ ] 中断入口路径：entry_64.S / interrupt_entry / common_interrupt
  - [ ] 异常入口：#PF / #GP / #DE / #DB 与 do_error_entry
  - [ ] NMI 处理与不可屏蔽中断路径
  - [ ] MCE (Machine Check Exception) 与硬件故障处理
  - [ ] 双重故障 (Double Fault) 与栈切换
- [ ] **用户态 / 内核态切换**
  - [ ] 特权级切换：CPL / RPL / DPL 与段选择子
  - [ ] 栈切换：内核栈 / per-cpu IST 栈 / entry trampoline stack
  - [ ] 用户态寄存器保存与恢复 (pt_regs 结构)
  - [ ] 切换开销分析与优化 (syscall fast path)

### 11. 时间与定时器 (Time & Timers)

- [ ] **时钟源与时间基础**
  - [ ] clocksource 抽象与注册 (TSC / HPET / ACPI PM Timer)
  - [ ] TSC (Time Stamp Counter)：invariant TSC / frequency / reliability
  - [ ] clocksource rating 与自动选择
  - [ ] timekeeping 模块与单调时钟维护
  - [ ] jiffies 与 CLOCK_TICK_RATE
  - [ ] POSIX clock 与 clock_gettime 实现
- [ ] **定时器框架**
  - [ ] hrtimer (High-Resolution Timer)：红黑树组织 / base / clock base
  - [ ] hrtimer_range / hrtimer_start / hrtimer_cancel
  - [ ] hrtimer 与 tick device 的关系
  - [ ] timer_list 低精度定时器与 hrtimer emulation
  - [ ] Tickerless / NO_HZ: idle / full / full dynticks 模式
  - [ ] tick_sched_timer 与定时广播 (broadcast)
- [ ] **延迟与调度工作**
  - [ ] schedule_timeout / msleep / udelay / ndelay
  - [ ] delayed_work 与工作队列延迟调度
  - [ ] wait_event_timeout 与条件等待超时
  - [ ] 时间与调度器 tick 的耦合

### 12. 内核启动与初始化 (Boot & Init)

- [ ] **启动流程**
  - [ ] BIOS / UEFI 固件阶段与 GRUB 加载
  - [ ] 内核映像格式与解压 (bzImage / startup_64)
  - [ ] start_kernel 流程：各子系统初始化顺序
  - [ ] early boot 参数解析与 cmdline
  - [ ] early param 与 __setup 宏
  - [ ] initcall 层级：pure / core / postcore / arch / subsys / fs / device / late
- [ ] **init 进程与用户空间**
  - [ ] init 进程选择：/sbin/init / systemd / upstart
  - [ ] initcall 与内核内置模块初始化
  - [ ] rootfs 与 initramfs 挂载
  - [ ] pivot_root 与根文件系统切换
  - [ ] init 进程的 PID 1 特殊职责
- [ ] **设备发现与初始化**
  - [ ] Device Tree (FDT / DTB) 解析与 of_* API
  - [ ] ACPI 表解析与设备枚举
  - [ ] deferred probe 与设备依赖排序
  - [ ] module_init / module_exit 与动态加载

### 13. 电源管理 (Power Management)

- [ ] **CPU 电源状态**
  - [ ] C-states (C0 / C1 / C2 / C3...) 与 idle 驱动 (intel_idle / acpi_idle)
  - [ ] cpuidle governor：ladder / menu
  - [ ] P-states 与 CPU 频率调节 (cpufreq)
  - [ ] cpufreq governor：performance / powersave / ondemand / conservative / schedutil
  - [ ] schedutil 与调度器联动
- [ ] **系统级电源管理**
  - [ ] suspend-to-RAM (S3) / suspend-to-disk (S4 / hibernation) / S0ix (Modern Standby)
  - [ ] freeze / standby / mem / disk 挂起级别
  - [ ] suspend 流程：prepare / suspend / suspend_late / suspend_noirq / resume 对称路径
  - [ ] hibernate 与 swsusp / uswsusp
  - [ ] PM notifier 与电源事件通知
- [ ] **设备电源管理**
  - [ ] runtime PM：pm_runtime_* API 与引用计数
  - [ ] autosleep / autosuspend 与延迟挂起
  - [ ] 设备 wakeup 与 enable_irq_wake
  - [ ] PM QoS 与延迟约束

### 14. 内核调试与性能分析 (Debugging & Profiling)

- [ ] **内核日志**
  - [ ] printk 日志级别 (KERN_EMERG ~ KERN_DEBUG) 与控制台输出
  - [ ] dmesg 与 /proc/kmsg / dev/kmsg
  - [ ] printk rate limiting 与 printk_ratelimited
  - [ ] pr_* 宏家族：pr_info / pr_err / pr_warn / pr_debug
  - [ ] dynamic debug 与 pr_debug 动态开关
  - [ ] early console 与 boot early_printk
- [ ] **ftrace 与 tracepoint**
  - [ ] ftrace 功能概览：function tracer / function graph / irq / preempt / sched
  - [ ] ftrace 实现：gcc -pg / mcount / ftrace_caller / ftrace_ops
  - [ ] tracepoint 声明与注册 (TRACE_EVENT / DECLARE_TRACE)
  - [ ] tracepoint 与 ftrace 的协作
  - [ ] trace events 与 /sys/kernel/tracing/events 目录
  - [ ] trace buffer 与 per-cpu ring buffer
- [ ] **kprobe**
  - [ ] kprobe 注册与执行：pre_handler / post_handler / fault_handler
  - [ ] kprobe 实现原理：INT3 替换 / 单步执行 / 执行回调
  - [ ] jprobe / kretprobe 与函数返回探测
  - [ ] kprobe blacklist 与不可探测函数
  - [ ] ftrace-based kprobe 与优化路径
- [ ] **perf**
  - [ ] perf 采样原理：硬件事件 (PMU) / 软件事件 / tracepoint 事件
  - [ ] perf stat / perf record / perf report / perf top
  - [ ] perf hardware counters：cycles / instructions / cache-ref / branch-misses
  - [ ] perf 调用图采集：fp / dwarf / lbr
  - [ ] perf cgroup / perf pid 过滤
  - [ ] perf script 与自定义分析
- [ ] **内核崩溃分析**
  - [ ] kdump 与 kexec 快速重载内核
  - [ ] crash 工具与 vmcore 分析
  - [ ] kexec_load / kexec_file_load 系统调用
  - [ ] pstore 与 ramoops 崩溃日志持久化
  - [ ] kernel panic / oops / BUG / WARN 输出解析
  - [ ] objdump / gdb + vmlinux 调试

### 15. 安全与权限 (Security & Access Control)

- [ ] **Linux 安全模块 (LSM)**
  - [ ] LSM hook 注册与调用点 (inode_permission / file_open / mmap / task_create 等)
  - [ ] LSM 堆叠 (stacking) 与多个 LSM 并存
  - [ ] LSM 顺序与 major / minor LSM 分类
  - [ ] BPF LSM 与动态安全策略
- [ ] **SELinux**
  - [ ] SELinux 架构：policy / enforcement / labeling
  - [ ] security context 与类型强制 (TE) 规则
  - [ ] MLS / MCS 多级安全与类别
  - [ ] SELinux 状态切换与 audit 日志
- [ ] **AppArmor**
  - [ ] AppArmor profile 语法与路径匹配
  - [ ] AppArmor 与 SELinux 对比
  - [ ] profile 加载 / 编译 / 缓存
- [ ] **capabilities**
  - [ ] POSIX capabilities 体系：CAP_CHOWN / CAP_NET_RAW / CAP_SYS_ADMIN 等
  - [ ] capability set：permitted / effective / inheritable / ambient
  - [ ] capset / capget 系统调用与能力升降
  - [ ] 容器中的 capabilities 裁剪策略
- [ ] **密钥与加密**
  - [ ] keyrings 与 key retention service
  - [ ] 内核加密 API (crypto API)：算法注册 / 模板 / 异步变换
  - [ ] dm-crypt / LUKS 块设备加密
  - [ ] trusted key / encrypted key 与 TPM 集成

---

## 学习路线建议

按以下顺序逐步深入：

1. **调度先行**：进程调度 (1) — 理解内核最核心的资源分配机制
2. **内存根基**：内存管理 (2) — 所有子系统都依赖内存，必须深入掌握
3. **存储打通**：文件系统与块层 (3) — 理解数据持久化与 I/O 全路径
4. **eBPF 实战**：eBPF (4) — 现代内核可观测性与网络编程基石
5. **虚拟化扩展**：虚拟化与容器 (5) — 理解云基础设施的内核支撑
6. **进程全景**：进程管理 (6) — 配合调度理解完整的进程生命周期
7. **网络数据流**：网络子系统 (7) — 从 socket 到驱动的完整路径
8. **并发基石**：同步与锁 (8) — 穿插在各模块学习中逐步掌握
9. **驱动接口**：设备驱动 (9) — 理解内核与硬件的交互层
10. **机制入口**：系统调用与内核入口 (10) — 理解用户态到内核态的桥梁
11. **专题深入**：时间 (11) → 启动 (12) → 电源 (13) → 调试 (14) → 安全 (15) — 按需深入

## 参考资源

- [Linux 内核源码](https://github.com/torvalds/linux)
- [Bootlin Elixir - 内核源码交叉引用浏览](https://elixir.bootlin.com/linux/latest/source)
- 《Understanding the Linux Kernel》 — Daniel P. Bovet & Marco Cesati
- 《Linux Kernel Development》 — Robert Love
- 《深入理解 Linux 内核架构》 — Wolfgang Mauerer
- 《Linux 内核设计与实现》 — Robert Love (中文译本)
- 《深入理解 Linux 网络技术栈》 — Arthur Chun Wang Liu
- 《BPF 性能之火》 — Brendan Gregg
- 《容器云架构与内核》 — 倪朋飞 / 赵亚