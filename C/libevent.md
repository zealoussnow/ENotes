## 源代码组织结构

libevent库主要分为：头文件、辅助功能函数、日志、libevent框架、对系统
IO多路复用机制的封装、信号管理、定时事件管理、缓冲区管理、基本数据结构
和基于libevent的两个实用库等几部分。

### 1. 头文件

主要就是event.h：事件宏定义、接口函数声明，主要结构体event的声明

### 2. 内部头文件

xxx-internal.h：内部数据结构和函数，对外不可见，以达到信息隐藏的目的

### 3. libevent框架

event.c：event整体框架的代码实现

### 4. 对系统IO多路复用机制的封装(Linux默认使用epoll)

epoll.c：对epoll的封装

    1). epoll_init(event_base_new, event_init)

    2). epoll_changelist_add/epoll_nochangelist_add(event_add)

    3). epoll_dispatch

    4). epoll_changelist_del/epoll_nochangelist_del(在evmap_io_del中被调用)

    5). epoll_dealloc(在event_base_free中被调用)

poll.c：对poll的封装

select.c：对select的封装

devpoll.c：对/dev/poll的封装

kqueue.c：对kqueue的封装


### 5. 定时事件管理

min-heap.h：其实就是一个以时间作为key的小根堆结构

### 6. 信号管理

signal.c：对信号事件的处理

### 7. 辅助功能函数

evutil.h和evutil.c：一些辅助功能函数，包括创建socket pair和一些时间
操作函数：加、减和比较等

### 8. 日志

log.h和log.c：log日志函数

### 9. 缓冲区管理

evbuffer.c和buffer.c：libevent对缓冲区的封装

### 10. 基本数据结构

compact/sys下的两个源文件：queue.h是libevent基本数据结构的实现，包括
链表，双向链表，队列等。

### 11. 网络库

http和evdns：是基于libevent实现的http服务器和异步dns查询库

## 源码分析

### Reactor --- 反应器

典型的Reactor声明方式，反应器时事件管理的接口

    class Reactor {
    public:
        int register_handler(EventHandler *handler, int event);
        int remove_handler(EventHandler *handler, int event);
        void handle_events(timeval *tv);
        // ...
    }

在libevent中就是event\_base结构体, event\_base是一组event的集合

    /* event-internal.h */
    struct event_base {
    	/** Function pointers and other data to describe this event_base's
    	 * backend. */
    	const struct eventop *evsel;
    	/** Pointer to backend-specific data. */
    	void *evbase;

    	/** List of changes to tell backend about at next dispatch.  Only used
    	 * by the O(1) backends. */
    	struct event_changelist changelist;

    	/** Function pointers used to describe the backend that this event_base
    	 * uses for signals */
    	const struct eventop *evsigsel;
    	/** Data to implement the common signal handelr code. */
    	struct evsig_info sig;

    	/** Number of virtual events */
    	int virtual_event_count;
    	/** Number of total events added to this event_base */
    	int event_count;
    	/** Number of total events active in this event_base */
    	int event_count_active;

    	/** Set if we should terminate the loop once we're done processing
    	 * events. */
    	int event_gotterm;
    	/** Set if we should terminate the loop immediately */
    	int event_break;
    	/** Set if we should start a new instance of the loop immediately. */
    	int event_continue;

    	/** The currently running priority of events */
    	int event_running_priority;

    	/** Set if we're running the event_base_loop function, to prevent
    	 * reentrant invocation. */
    	int running_loop;

    	/* Active event management. */
    	/** An array of nactivequeues queues for active events (ones that
    	 * have triggered, and whose callbacks need to be called).  Low
    	 * priority numbers are more important, and stall higher ones.
    	 */
    	struct event_list *activequeues;
    	/** The length of the activequeues array */
    	int nactivequeues;

    	/* common timeout logic */

    	/** An array of common_timeout_list* for all of the common timeout
    	 * values we know. */
    	struct common_timeout_list **common_timeout_queues;
    	/** The number of entries used in common_timeout_queues */
    	int n_common_timeouts;
    	/** The total size of common_timeout_queues. */
    	int n_common_timeouts_allocated;

    	/** List of defered_cb that are active.  We run these after the active
    	 * events. */
    	struct deferred_cb_queue defer_queue;

    	/** Mapping from file descriptors to enabled (added) events */
    	struct event_io_map io;

    	/** Mapping from signal numbers to enabled (added) events. */
    	struct event_signal_map sigmap;

    	/** All events that have been enabled (added) in this event_base */
    	struct event_list eventqueue;

    	/** Stored timeval; used to detect when time is running backwards. */
    	struct timeval event_tv;

    	/** Priority queue of events with timeouts. */
    	struct min_heap timeheap;

    	/** Stored timeval: used to avoid calling gettimeofday/clock_gettime
    	 * too often. */
    	struct timeval tv_cache;

    #if defined(_EVENT_HAVE_CLOCK_GETTIME) && defined(CLOCK_MONOTONIC)
    	/** Difference between internal time (maybe from clock_gettime) and
    	 * gettimeofday. */
    	struct timeval tv_clock_diff;
    	/** Second in which we last updated tv_clock_diff, in monotonic time. */
    	time_t last_updated_clock_diff;
    #endif

    #ifndef _EVENT_DISABLE_THREAD_SUPPORT
    	/* threading support */
    	/** The thread currently running the event_loop for this base */
    	unsigned long th_owner_id;
    	/** A lock to prevent conflicting accesses to this event_base */
    	void *th_base_lock;
    	/** The event whose callback is executing right now */
    	struct event *current_event;
    	/** A condition that gets signalled when we're done processing an
    	 * event with waiters on it. */
    	void *current_event_cond;
    	/** Number of threads blocking on current_event_cond. */
    	int current_event_waiters;
    #endif

    #ifdef WIN32
    	/** IOCP support structure, if IOCP is enabled. */
    	struct event_iocp_port *iocp;
    #endif

    	/** Flags that this base was configured with */
    	enum event_base_config_flag flags;

    	/* Notify main thread to wake up break, etc. */
    	/** True if the base already has a pending notify, and we don't need
    	 * to add any more. */
    	int is_notify_pending;
    	/** A socketpair used by some th_notify functions to wake up the main
    	 * thread. */
    	evutil_socket_t th_notify_fd[2];
    	/** An event used by some th_notify functions to wake up the main
    	 * thread. */
    	struct event th_notify;
    	/** A function used to wake up the main thread from another thread. */
    	int (*th_notify_fn)(struct event_base *base);
    };

### EventHandler --- 事件处理程序

event是整个libevent库的核心，是Reactor框架中的事件处理程序组件，它提供了函数接口，供Reactor在事件发生时
调用，以执行相应的事件处理，通常它会绑定一个有效的句柄。

    /* include/event2/event_struct.h */
    struct event {
        TAILQ_ENTRY(event) ev_active_next;
        TAILQ_ENTRY(event) ev_next;
        /* for managing timeouts */
        union {
            TAILQ_ENTRY(event) ev_next_with_common_timeout;
            int min_heap_idx;
        } ev_timeout_pos;
        evutil_socket_t ev_fd;

        struct event_base *ev_base;

        union {
            /* used for io events */
            struct {
                TAILQ_ENTRY(event) ev_io_next;
                struct timeval ev_timeout;
            } ev_io;

            /* used by signal events */
            struct {
                TAILQ_ENTRY(event) ev_signal_next;
                short ev_ncalls;
                /* Allows deletes in callback */
                short *ev_pncalls;
            } ev_signal;
        } _ev;

        short ev_events;
        short ev_res;		/* result passed to event callback */
        short ev_flags;
        ev_uint8_t ev_pri;	/* smaller numbers are higher priority */
        ev_uint8_t ev_closure;
        struct timeval ev_timeout;

        /* allows us to adopt for different types of events */
        void (*ev_callback)(evutil_socket_t, short, void *arg);
        void *ev_arg;
    };

宏展开后为：

    struct event {
        struct { struct event *tqe_next; struct event **tqe_prev; } ev_active_next;
        struct { struct event *tqe_next; struct event **tqe_prev; } ev_next;

        union {
            struct { struct event *tqe_next; struct event **tqe_prev; } ev_next_with_common_timeout;
            int min_heap_idx;
        } ev_timeout_pos;
        int ev_fd;

        struct event_base *ev_base;

        union {

            struct {
            struct { struct event *tqe_next; struct event **tqe_prev; } ev_io_next;
            struct timeval ev_timeout;
        } ev_io;


        struct {
            struct { struct event *tqe_next; struct event **tqe_prev; } ev_signal_next;
                short ev_ncalls;

                short *ev_pncalls;
            } ev_signal;
        } _ev;

        short ev_events;
        short ev_res;
        short ev_flags;
        uint8_t ev_pri;
        uint8_t ev_closure;
        struct timeval ev_timeout;


        void (*ev_callback)(int, short, void *arg);
        void *ev_arg;
    };

### Signal

    struct evsig_info {
        /* Event watching ev_signal_pair[1] */
        struct event ev_signal;
        /* Socketpair used to send notifications from the signal handler */
        evutil_socket_t ev_signal_pair[2];
        /* True iff we've added the ev_signal event yet. */
        int ev_signal_added;
        /* Count of the number of signals we're currently watching. */
        int ev_n_signals_added;

        /* Array of previous signal handler objects before Libevent started
         * messing with them.  Used to restore old signal handlers. */
    #ifdef _EVENT_HAVE_SIGACTION
        struct sigaction **sh_old;
    #else
        ev_sighandler_t **sh_old;
    #endif
        /* Size of sh_old. */
        int sh_old_max;
    };

### event_list

ev_map.c中的evmap_io中用到了event_list结构，event_list在event_struct.h
的结构是:

    TAILQ_HEAD (event_list, event);
    #ifndef TAILQ_HEAD
    #define _EVENT_DEFINED_TQHEAD
    #define TAILQ_HEAD(name, type)          \
    struct name {                   \
        struct type *tqh_first;         \
        struct type **tqh_last;         \
    }
    #endif

使用cpp或者gcc -E展开后得到的结果：

    struct event_list
    {
        struct event *tqh_first;
        struct event **tqh_last;
    };