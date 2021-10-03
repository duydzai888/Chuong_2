# Quản lí quy trình
### Kiểm tra danh sách quy trình
#### Xem các quá trình với ps
```
[root@laiduy ~]# ps
   PID TTY          TIME CMD
 58694 pts/0    00:00:00 bash
 59210 pts/0    00:00:00 ps
```
Xem mọi tiến trình đang chạy trên hệ thống sử dụng option `-ef`
```
[root@laiduy ~]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 Sep18 ?        00:00:37 /usr/lib/systemd/systemd --system --deserialize 18
root          2      0  0 Sep18 ?        00:00:00 [kthreadd]
root          4      2  0 Sep18 ?        00:00:00 [kworker/0:0H]
root          6      2  0 Sep18 ?        00:00:06 [ksoftirqd/0]
root          7      2  0 Sep18 ?        00:00:00 [migration/0]
root          8      2  0 Sep18 ?        00:00:00 [rcu_bh]
root          9      2  0 Sep18 ?        00:00:55 [rcu_sched]
root         10      2  0 Sep18 ?        00:00:00 [lru-add-drain]
root         11      2  0 Sep18 ?        00:00:10 [watchdog/0]
root         12      2  0 Sep18 ?        00:00:05 [watchdog/1]
root         13      2  0 Sep18 ?        00:00:01 [migration/1]
root         14      2  0 Sep18 ?        00:00:09 [ksoftirqd/1]
root         16      2  0 Sep18 ?        00:00:00 [kworker/1:0H]
root         18      2  0 Sep18 ?        00:00:00 [kdevtmpfs]
root         19      2  0 Sep18 ?        00:00:00 [netns]
...
```
1. UID: Người dùng chạy tiến trình.
1. PID: ID của tiến trình.
1. PPID: ID của tiến trình mẹ nếu nó được sinh ra từ tiến trình khác.
1. C: The processor utilization over the lifetime of the process.
1. STIME: Thời gian hệ thống bắt đầu tiến trình.
1. TTY: Thiết bị đầu cuối mà từ đó tiến trình được bắt đầu.
1. TIME: Thời gian chuẩn bị cpu cần thiết để chạy tiến trình
CMD: Tên chương trình bắt đầu tiến trình này.
### Hiểu các trạng thái của quy trình
Các quá trình được hoán đổi vào bộ nhớ ảo được gọi là đang ngủ. Thường thì hạt nhân Linux đặt một tiến trình vào chế độ ngủ trong khi tiến trình này đang đợi một sự kiện. Khi sự kiện kích hoạt, hạt nhân sẽ gửi tín hiệu cho quá trình.
Nếu quá trình ở chế độ ngủ gián đoạn, nó sẽ nhận được tín hiệu ngay lập tức và thức dậy.
Nếu quá trình ở chế độ ngủ liên tục, nó chỉ thức dậy dựa trên một sự kiện bên ngoài, chẳng hạn như phần cứng trở nên khả dụng. Nó sẽ lưu bất kỳ tín hiệu nào khác được gửi khi nó đang ngủ và hoạt động trên chúng khi nó thức dậy.
Nếu một quy trình đã kết thúc nhưng quy trình mẹ của nó không thừa nhậntín hiệu kết thúc vì nó đang ngủ, thì quy trình đó được coi là một thây ma. Nó bị mắc kẹt trong trạng thái lấp lửng giữa chạy và kết thúc cho đến khi quy trình chính xác nhận tín hiệu kết thúc.
### Chọn các quy trình với ps
Xem process của user
```
[root@laiduy ~]# ps -u postfix
   PID TTY          TIME CMD
  1094 ?        00:00:00 qmgr
 59234 ?        00:00:00 pickup
```
### Xem các quy trình với đầu
Sử dụng lệnh top để có thêm thông tin về các tiến trình đang chạy 
```
[root@laiduy ~]# top
top - 14:10:38 up 1 day, 19:58,  2 users,  load average: 0.00, 0.01, 0.05
Tasks: 110 total,   1 running, 109 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.0 sy,  0.0 ni, 99.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1863032 total,   460992 free,   281376 used,  1120664 buff/cache
KiB Swap:  2097148 total,  2096628 free,      520 used.  1377608 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
   300 root      20   0       0      0      0 S   0.3  0.0   2:12.87 xfsaild/sda3
 59316 root      20   0  162092   2216   1552 R   0.3  0.1   0:00.05 top
     1 root      20   0   46212   6552   4016 S   0.0  0.4   0:37.76 systemd
     2 root      20   0       0      0      0 S   0.0  0.0   0:00.53 kthreadd
     4 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H
     6 root      20   0       0      0      0 S   0.0  0.0   0:06.77 ksoftirqd/0
     7 root      rt   0       0      0      0 S   0.0  0.0   0:00.86 migration/0
     8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh
     9 root      20   0       0      0      0 S   0.0  0.0   0:55.51 rcu_sched
    10 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 lru-add-drain
    11 root      rt   0       0      0      0 S   0.0  0.0   0:10.60 watchdog/0
    12 root      rt   0       0      0      0 S   0.0  0.0   0:05.88 watchdog/1
    13 root      rt   0       0      0      0 S   0.0  0.0   0:01.54 migration/1
    14 root      20   0       0      0      0 S   0.0  0.0   0:09.46 ksoftirqd/1
    16 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/1:0H
    18 root      20   0       0      0      0 S   0.0  0.0   0:00.07 kdevtmpfs
    19 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 netns
    20 root      20   0       0      0      0 S   0.0  0.0   0:00.24 khungtaskd
    21 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 writeback
    22 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kintegrityd
    23 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 bioset
    24 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 bioset
    25 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 bioset
    26 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kblockd
    27 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 md
    28 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 edac-poller
```
Sử dụng lệnh `free` để xem trạng thái RAM của hệ thống
```
[root@laiduy ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           1.8G        274M        450M         32M        1.1G        1.3G
Swap:          2.0G        520K        2.0G
```
### Sử dụng nhiều màn hình
`screen` command cho phép tạo cửa sổ làm việc. Khi đăng nhập ssh vào linux, hãy gõ `screen`, 1 cửa sổ làm việc sẽ được tạo.
### Hiểu các quy trình tiền cảnh và hậu cảnh
#### Dừng 1 công việc
Sử dụng `kill -l`để liệt kê các tín hiệu 
```
[root@laiduy ~]# kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```
