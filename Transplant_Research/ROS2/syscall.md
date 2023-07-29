# RustDDS 系统调用情况分析



RustDDS 提供了基于[DDSI-RTPS](http://www.omg.org/spec/DDSI-RTPS/) 规范的测试仓库，本分析主要基于 [该仓库](https://github.com/jhelovuo/dds-rtps) 中提供的示例，对于其使用的系统调用进行分析，看当前 arceos 中缺少什么样的元素。



```
dds-rtps/RustDDS $ strace -c cargo run -- -P -t publisher

strace: Process 455720 detached
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ------------------
 95.99    0.649592          66      9830           epoll_wait
  2.04    0.013793           1      9854           write
  0.46    0.003126           2      1060           read
  0.40    0.002688           1      1969       541 statx
  0.36    0.002453           7       346           futex
  0.28    0.001870           1      1139       449 openat
  0.11    0.000775           1       701           close
  0.08    0.000545         181         3           execve
  0.05    0.000319          31        10           sendto
  0.03    0.000234           1       149           brk
  0.03    0.000196           1       136           getdents64
  0.02    0.000138           4        30           recvmsg
  0.02    0.000127           4        29           mprotect
  0.02    0.000107           1        96           mmap
  0.01    0.000099           0       198        93 newfstatat
  0.01    0.000086           2        35        35 mkdir
  0.01    0.000080           6        13           munmap
  0.01    0.000070          14         5           clone3
  0.01    0.000042           2        15         7 ioctl
  0.01    0.000042           4         9           socket
  0.01    0.000037           4         9           bind
  0.00    0.000027           2        13           getsockname
  0.00    0.000024           1        14           rt_sigprocmask
  0.00    0.000023          23         1           epoll_create1
  0.00    0.000022          11         2           socketpair
  0.00    0.000022           1        14           flock
  0.00    0.000021           1        21           rt_sigaction
  0.00    0.000021           1        15           getpid
  0.00    0.000020           2         8           setsockopt
  0.00    0.000018           2         7           statfs
  0.00    0.000015           1        11           fcntl
  0.00    0.000014           0        19           getcwd
  0.00    0.000013           4         3           pipe2
  0.00    0.000012           1         9           getrandom
  0.00    0.000010           3         3           poll
  0.00    0.000010           0        18         9 access
  0.00    0.000009           0        25        22 readlink
  0.00    0.000007           1         6           prlimit64
  0.00    0.000006           1         4           sched_getaffinity
  0.00    0.000005           0        12           stat
  0.00    0.000004           0        10           pread64
  0.00    0.000004           2         2           sysinfo
  0.00    0.000004           1         3           getuid
  0.00    0.000004           1         3           geteuid
  0.00    0.000004           0         6           sigaltstack
  0.00    0.000004           4         1           epoll_ctl
  0.00    0.000003           0         6         3 arch_prctl
  0.00    0.000002           2         1           lseek
  0.00    0.000002           2         1           sched_yield
  0.00    0.000002           0         3           set_robust_list
  0.00    0.000002           0         3           rseq
  0.00    0.000001           1         1           chdir
  0.00    0.000001           0         3           set_tid_address
  0.00    0.000001           1         1           tgkill
  0.00    0.000000           0         1           mremap
------ ----------- ----------- --------- --------- ------------------
100.00    0.676756          26     25886      1159 total
```

```
dds-rtps/RustDDS $ strace -c cargo run -- -S -t subscriber
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ------------------
 99.60    0.724461          73      9813         1 epoll_wait
  0.11    0.000818           0      1061           read
  0.10    0.000731           0      1969       541 statx
  0.07    0.000477           0      1139       449 openat
  0.03    0.000240           0       701           close
  0.03    0.000203          67         3           execve
  0.01    0.000098           0       349         2 futex
  0.01    0.000066           5        13           munmap
  0.01    0.000061           0       144           brk
  0.01    0.000060           1        35        35 mkdir
  0.01    0.000039           2        18         9 access
  0.00    0.000025           1        25        22 readlink
  0.00    0.000019           0        96           mmap
  0.00    0.000017           1        12           stat
  0.00    0.000016           0       198        93 newfstatat
  0.00    0.000013           0       136           getdents64
  0.00    0.000011           0        37           write
  0.00    0.000004           0         5           clone3
  0.00    0.000003           0        29           mprotect
  0.00    0.000003           0        14           flock
  0.00    0.000002           0         6           sigaltstack
  0.00    0.000002           0         9           getrandom
  0.00    0.000001           0         3           poll
  0.00    0.000001           0        21           rt_sigaction
  0.00    0.000001           0        14           rt_sigprocmask
  0.00    0.000001           0        10           pread64
  0.00    0.000001           1         1           sched_yield
  0.00    0.000001           0        15           getpid
  0.00    0.000001           1         1           chdir
  0.00    0.000001           0         7           statfs
  0.00    0.000001           0         3           set_tid_address
  0.00    0.000001           1         1           tgkill
  0.00    0.000001           0         6           prlimit64
  0.00    0.000000           0         1           lseek
  0.00    0.000000           0        15         7 ioctl
  0.00    0.000000           0         1           mremap
  0.00    0.000000           0         9           socket
  0.00    0.000000           0         8           sendto
  0.00    0.000000           0        24           recvmsg
  0.00    0.000000           0         9         1 bind
  0.00    0.000000           0        12           getsockname
  0.00    0.000000           0         2           socketpair
  0.00    0.000000           0         8           setsockopt
  0.00    0.000000           0        11           fcntl
  0.00    0.000000           0        19           getcwd
  0.00    0.000000           0         2           sysinfo
  0.00    0.000000           0         3           getuid
  0.00    0.000000           0         3           geteuid
  0.00    0.000000           0         6         3 arch_prctl
  0.00    0.000000           0         4           sched_getaffinity
  0.00    0.000000           0         1           epoll_ctl
  0.00    0.000000           0         3           set_robust_list
  0.00    0.000000           0         1           epoll_create1
  0.00    0.000000           0         3           pipe2
  0.00    0.000000           0         3           rseq
------ ----------- ----------- --------- --------- ------------------
100.00    0.727380          45     16042      1163 total
```

经过[脚本](https://gist.github.com/jackyliu16/9d5c15d90b930cf743892c06d207242e)分析之后，发现在发送方和接受方同时出现的系统调用有

```
{'set_tid_address', 'rt_sigprocmask', 'mkdir', 'epoll_ctl', 'munmap', 'sched_yield', 'epoll_create1', 'write', 'execve', 'openat', 'sysinfo', 'getuid', 'set_robust_list', 'setsockopt', 'mremap', 'rseq', 'epoll_wait', 'mprotect', 'recvmsg', 'stat', 'getcwd', 'sigaltstack', 'prlimit64', 'readlink', 'socketpair', 'flock', 'close', 'newfstatat', 'tgkill', 'rt_sigaction', 'socket', 'chdir', 'geteuid', 'getdents64', 'clone3', 'getpid', 'statfs', 'sendto', 'getrandom', 'mmap', 'brk', 'getsockname', 'statx', 'fcntl', 'futex', 'bind', 'read', 'pipe2', 'lseek', 'ioctl', 'pread64', 'sched_getaffinity', 'arch_prctl', 'poll', 'access'}
```

```
# haven't yet
'set_tid_address', 'rt_sigprocmask','sched_yield','execve','openat','openat', 'sysinfo', 'getuid', 'set_robust_list','rseq','recvmsg', 'sigaltstack', 'prlimit64','socketpair','newfstatat', 'tgkill', 'rt_sigaction', 'chdir','getdents64','clone3','statfs','getrandom','statx','futex','ioctl', 'pread64', 'sched_getaffinity', 'arch_prctl', 'poll', 'access'
```

s



mprotect,'flock','brk',

# tiny_http

```
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ------------------
  0.00    0.000000           0         7           read
  0.00    0.000000           0         1           write
  0.00    0.000000           0         4           close
  0.00    0.000000           0         1           poll
  0.00    0.000000           0        17           mmap
  0.00    0.000000           0         7           mprotect
  0.00    0.000000           0         3           brk
  0.00    0.000000           0         6           rt_sigaction
  0.00    0.000000           0         3           rt_sigprocmask
  0.00    0.000000           0         2           pread64
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           socket
  0.00    0.000000           0         1           sendto
  0.00    0.000000           0         1           bind
  0.00    0.000000           0         1           listen
  0.00    0.000000           0         1           getsockname
  0.00    0.000000           0         1           setsockopt
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         2           sigaltstack
  0.00    0.000000           0         2         1 arch_prctl
  0.00    0.000000           0         1           futex
  0.00    0.000000           0         1           sched_getaffinity
  0.00    0.000000           0         1           set_tid_address
  0.00    0.000000           0        12         8 openat
  0.00    0.000000           0        12         7 newfstatat
  0.00    0.000000           0         1           set_robust_list
  0.00    0.000000           0         2           prlimit64
  0.00    0.000000           0         1           getrandom
  0.00    0.000000           0         1           rseq
  0.00    0.000000           0         1           clone3
------ ----------- ----------- --------- --------- ------------------
100.00    0.000000           0        96        17 total
```

