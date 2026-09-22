<img width="1536" height="1024" alt="6eadd4df-6a7f-48aa-915f-b3c9461b60a2" src="https://github.com/user-attachments/assets/253e53d8-2571-451f-acb9-2ae6e885389f" />


## 📚 Table of Contents

21. ⚡ [CPU Troubleshooting with eBPF](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#21--cpu-troubleshooting-with-ebpf)
22. 🔥 [execsnoop](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#22--execsnoop)
23. 📁 [opensnoop](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#23--opensnoop)
24. 🌐 [tcpconnect](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#24--tcpconnect)
25. ⏱️ [tcplife](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#25--tcplife)
26. 💾 [Disk I/O Latency](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#26--disk-io-latency)
27. 🧵 [Run Queue Latency](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#27--run-queue-latency)
28. 🔐 [eBPF and Security](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#28--ebpf-and-security)
29. 📦 [eBPF + Containers](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#29--ebpf--containers)
30. ☸️ [eBPF + Kubernetes](https://chatgpt.com/c/6ab22efe-51d4-83e8-96ad-9985a4bce765#30--ebpf--kubernetes)

---
    
  # 21. ⚡ CPU Troubleshooting with eBPF

Suppose an application suddenly consumes significant CPU.

Start with traditional Linux tools:

```bash
top

```

Then investigate deeper using eBPF-based tools.

>BCC (BPF Compiler Collection) is a powerful toolkit used for dynamic Linux kernel tracing, performance monitoring, and network analysis.
>It provides a collection of out-of-the-box tools and a framework to write custom programs that leverage eBPF (extended Berkeley Packet Filter)—
>a technology built into the Linux kernel that allows you to safely run sandboxed code inside the kernel without crashing it or needing a reboot.

BCC provides useful tools such as:

```text
execsnoop
opensnoop
tcpconnect
tcplife
biolatency
runqlat

```

These are useful for DevOps troubleshooting.

---

# 22. 🔥 execsnoop

`execsnoop` displays commands being executed.

Run:

```bash
sudo execsnoop

```

Then:

```bash
ls

```

or:

```bash
curl example.com

```

You may see:

```text
PID     PPID     COMM
4210    4000     ls
4230    4000     curl

```

### 🎯 Useful Question

> **Which processes are being launched on my server?**

---

# 23. 📁 opensnoop

Run:

```bash
sudo opensnoop

```

Then:

```bash
cat /etc/hosts

```

You can investigate:

```text
Which process opened a file?
Which files are being accessed?
When did the access happen?

```

---

# 24. 🌐 tcpconnect

Run:

```bash
sudo tcpconnect

```

Then:

```bash
curl https://example.com

```

You can observe TCP connection attempts.

### 🔎 Useful For

```text
Unexpected outbound connections
Application network behavior
Connection problems
Network-heavy applications

```

---

# 25. ⏱️ tcplife

Run:

```bash
sudo tcplife

```

It can provide information about TCP connections and their lifetime.

Conceptually:

```text
Application
    ↓
TCP Connection
    ↓
Connection Starts
    ↓
Data Transfer
    ↓
Connection Closes

```

This is useful when investigating applications that repeatedly establish short-lived connections.

---

# 26. 💾 Disk I/O Latency

For storage troubleshooting, BCC provides:

```bash
biolatency

```

Run:

```bash
sudo biolatency

```

You may see latency distributions.

Conceptually:

```text
Disk I/O
   │
   ├── 10 μs
   ├── 20 μs
   ├── 50 μs
   ├── 100 μs
   ├── 500 μs
   └── 1 ms+

```

### 🎯 Question It Helps Investigate

> **Is disk I/O latency contributing to application slowness?**

---

# 27. 🧵 Run Queue Latency

Another useful BCC tool:

```bash
sudo runqlat

```

This can help investigate scheduler/run-queue latency.

Think about it like this:

```text
Process Wants CPU
       │
       ▼
CPU Unavailable
       │
       ▼
Process Waits
       │
       ▼
Scheduler
       │
       ▼
CPU Becomes Available

```

eBPF can help measure this waiting behavior.

---

# 28. 🔐 eBPF and Security

eBPF can also be used for security monitoring.

```text
Process
   │
   ▼
System Call
   │
   ▼
eBPF
   │
   ▼
Security Detection / Policy

```

Security-oriented systems can use eBPF to detect or observe:

```text
Unexpected process execution
Suspicious network activity
File access
Container behavior
Privilege-related activity

```

eBPF can also integrate with Linux security mechanisms such as **LSM** for security-related monitoring and enforcement.

---

# 29. 📦 eBPF + Containers

This is especially important for DevOps engineers.

Imagine:

```text
             Kubernetes
                 │
                 ▼
             Container
                 │
                 ▼
             Application
                 │
                 ▼
            Linux Kernel
                 │
                 ▼
                eBPF

```

Traditional monitoring might tell you:

```text
Container CPU = 80%

```

eBPF-based observability can provide deeper visibility into:

```text
Network connections
Processes
System calls
Latency
DNS
TCP connections
Kernel events

```

---

# 30. ☸️ eBPF + Kubernetes

A major real-world ecosystem around eBPF is **Cilium**, which uses eBPF for Kubernetes networking, observability, and security.

Conceptually:

```text
                    Kubernetes
                         │
              ┌──────────┼──────────┐
              │          │          │
             Pod        Pod        Pod
              │          │          │
              └──────────┼──────────┘
                         │
                       Cilium
                         │
                        eBPF
                         │
                   Linux Kernel

```

This makes eBPF particularly relevant to:

```text
DevOps
SRE
Cloud Engineering
Kubernetes
Platform Engineering
Security

```
