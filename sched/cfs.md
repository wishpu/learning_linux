# CFS 学习记录
## 核心结构体
```c
struct sched_entity {
	/* For load-balancing: */
	struct load_weight		load;
	struct rb_node			run_node; // 插入rbt，根据vruntime 排序，决定运行先后顺序
	u64				deadline;
	u64				min_vruntime;
	u64				min_slice;
	u64				max_slice;

	struct list_head		group_node; // 插入rq 的cfs_tasks 链表，负载均衡的时候用于detach
	unsigned char			on_rq;
	unsigned char			sched_delayed;
	unsigned char			rel_deadline;
	unsigned char			custom_slice;
					/* hole */

	u64				exec_start;
	u64				sum_exec_runtime;
	u64				prev_sum_exec_runtime;
	u64				vruntime;
	/* Approximated virtual lag: */
	s64				vlag;
	/* 'Protected' deadline, to give out minimum quantums: */
	u64				vprot;
	u64				slice;

	u64				nr_migrations;

#ifdef CONFIG_FAIR_GROUP_SCHED
	int				depth;
	struct sched_entity		*parent;
	/* rq on which this entity is (to be) queued: */
	struct cfs_rq			*cfs_rq;
	/* rq "owned" by this entity/group: */
	struct cfs_rq			*my_q;
	/* cached value of my_q->h_nr_running */
	unsigned long			runnable_weight;
#endif

	/*
	 * Per entity load average tracking.
	 *
	 * Put into separate cache line so it does not
	 * collide with read-mostly values above.
	 */
	struct sched_avg		avg;
};

```
