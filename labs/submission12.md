#### Task 1 - Install and Configure Kata
Kata was initially installed, configured and tested as required. Just so it works:

- Show the shim containerd-shim-kata-v2 --version
```
> containerd-shim-kata-v2 --version
Kata Containers containerd shim (Rust): id: io.containerd.kata.v2, version: 3.23.0, commit: bfc9e446e1f4e7db2741dc953c034447d55ec9b2
```
- Show a successful test run with sudo nerdctl run --runtime io.containerd.kata.v2 ...
```
> sudo nerdctl run --rm --runtime io.containerd.kata.v2 alpine:3.19 uname -a

WARN[0000] cannot set cgroup manager to "systemd" for runtime "io.containerd.kata.v2" 
Linux 3ced72b81fee 6.12.47 #1 SMP Fri Nov 14 15:34:06 UTC 2025 x86_64 Linux
```

#### Task 2 - Run and Compare Containers (runc vs kata)

- Show juice-runc health check (HTTP 200 from port 3012)
Although I can load site from browser and it works, curl always returns 503

```
juice-runc: HTTP 503
```

- Show Kata containers running successfully with --runtime io.containerd.kata.v2
Executed commands:
```
sudo nerdctl run --rm --runtime io.containerd.kata.v2 alpine:3.19 uname -a      > labs/lab12/kata/test1.txt
sudo nerdctl run --rm --runtime io.containerd.kata.v2 alpine:3.19 uname -r      > labs/lab12/kata/kernel.txt
sudo nerdctl run --rm --runtime io.containerd.kata.v2 alpine:3.19 sh -c \
  "grep 'model name' /proc/cpuinfo | head -1"                                   > labs/lab12/kata/cpu.txt
```
Created required files.
```
ls labs/lab12/kata/
cpu.txt  kernel.txt  test1.txt
```
- Compare kernel versions:
Runc shares host kernel 6.16.5-arch1-1), while Kata uses separate kernel (6.12.47)
```
=== Kernel Version Comparison ===
Host kernel (runc uses this): 6.16.5-arch1-1
Kata guest kernel: Linux version 6.12.47 (@4bcec8f4443d) (gcc (Ubuntu 11.4.0-1ubuntu1~22.04.2) 11.4.0, GNU ld (GNU Binutils for Ubuntu) 2.38) #1 SMP Fri Nov 14 15:34:06 UTC 2025
```
- Compare CPU models (real vs virtualized)
Kata VM just shows some general Xeon processor, which isn't even true as it's actually i5, probably providing just the fact that some intel cpu is used, and nothing more.
```
=== CPU Model Comparison ===
Host CPU:
model name  : Intel(R) Core(TM) i5-8265U CPU @ 1.60GHz
Kata VM CPU:
model name  : Intel(R) Xeon(R) Processor @ 1.60GHz
```
- Explain isolation implications:
  - runc:
    Containers shares host kernel, address space, isolated with namespaces and cgroups. Very solid close to baremetal performance.
  - Kata:
    Containers works in separate Virtual Machines, with unique kernel. Extreme level of isolation, but higher startup time, and worse performance overall.

## Task 3 - Isolation Tests

- Show dmesg output differences
```
=== dmesg Access Test ===
Kata VM (separate kernel boot logs):
time="2025-11-27T21:57:34+03:00" level=warning msg="cannot set cgroup manager to \"systemd\" for runtime \"io.containerd.kata.v2\""
[    0.000000] Linux version 6.12.47 (@4bcec8f4443d) (gcc (Ubuntu 11.4.0-1ubuntu1~22.04.2) 11.4.0, GNU ld (GNU Binutils for Ubuntu) 2.38) #1 SMP Fri Nov 14 15:34:06 UTC 2025
[    0.000000] Command line: reboot=k panic=1 systemd.unit=kata-containers.target systemd.mask=systemd-networkd.service root=/dev/vda1 rootflags=data=ordered,errors=remount-ro ro rootfstype=ext4 agent.container_pipe_size=1 console=ttyS1 agent.log_vport=1025 agent.passfd_listener_port=1027 virtio_mmio.device=8K@0xe0000000:5 virtio_mmio.device=8K@0xe0002000:5
[    0.000000] BIOS-provided physical RAM map:
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000009fbff] usable
```

- Compare /proc filesystem visibility
While host has 332 processes, kata sees only 52 entries, as it does see only processes inside the VM

- Show network interface configuration in Kata VM

```
=== Network Interfaces ===
Kata VM network:
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether d2:e2:eb:78:ed:94 brd ff:ff:ff:ff:ff:ff
    inet 10.4.0.15/24 brd 10.4.0.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::d0e2:ebff:fe78:ed94/64 scope link tentative 
       valid_lft forever preferred_lft forever
```

Kata network (ip addr) shows only loopback and eth0, while the host has dozens of variout vpn's/bridges/and long broken references, showing isolation of network interfaces.

- Compare kernel module counts (host vs guest VM)

```
=== Kernel Modules Count ===
Host kernel modules: 332
Kata guest kernel modules: 72
```

The Kata guest kernel loaded only essential for it's work kernel modules, again showing us isolation from host kernel, but also minimization of attack vector.


- Explain isolation boundary differences and escape scenarios:
runc is very lightweight isolation, container uses the same resources and kernel as host, escaping namespaces, cgroups (main isolation mechanizm) would result in free roam in host system.
kata in the other side is proper vm setup with (almost) complete isolation. It uses complete different kernel, isolated resources and io, so escape would require escaping an actual OS through very few connections with host system.

## Task 4 - Performance Snapshot
- Show startup time comparison
```
runc:
________________________________________________________
Executed in    4.03 secs      fish           external
   usr time   59.13 millis    0.99 millis   58.14 millis
   sys time   53.84 millis    4.64 millis   49.19 millis

Kata:

________________________________________________________
Executed in    6.59 secs      fish           external
   usr time    2.86 millis    0.80 millis    2.06 millis
   sys time   10.24 millis    1.25 millis    8.99 millis
```
Runc started a almost twice as fast as kata, as it doesn't need to create extra kernel.
- Show HTTP latency for juice-runc baseline
```
=== HTTP Latency Test (juice-runc) ===
Results for port 3012 (juice-runc):
avg=0.0001s min=0.0001s max=0.0005s n=50
```
Average(and min) latency of 0.1ms with peak 0.5ms shows how small the impact of containerization compared to typical network delay of 10-250ms

P.s. there's the output after the first time I used this command, can you guess why it happened?
```
=== HTTP Latency Test (juice-runc) ===
Results for port 3012 (juice-runc):
avg=1.5656s min=1.5264s max=1.6805s n=50
```

- Analyze performance tradeoffs:
  - Startup overhead: large overhead because extra kernel needed to be loaded
  - Runtime overhead: negligible
  - CPU overhead: Constant. Worth of additional kernel, relatively large for small applications, negligible for large application.
- Interpret when to use each:
  - Use runc when: trusted workloads and tenants. Minimal resource usage matters, speed of deploy matters.
  - Use Kata when: untrusted workloads/tenants, probablt security issues cost more than computing resources and time.
