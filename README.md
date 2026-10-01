# lsvm

An `ls`-style listing of VMware Workstation VMs on Linux, with live resource usage for the running ones.

```
NAME          GUEST OS        vCPU  RAM   CPU    HOST MEM USED         UPTIME  DISK  PID
kali-linux    Debian 10 x64   4     2.0G  12.4%  1.8G / 2.0G (90%)     1h12m   42G   48211
Ubuntu        Ubuntu x64      2     2.0G  -      -                     -       15G   -
AlmaLinux     AlmaLinux x64   2     2.0G  -      -                     -       18G   -
Windows-10    Windows 10 x64  2     2.0G  -      -                     -       19G   -
```

Running VMs are listed first (in green), stopped ones after (dimmed).

## Usage

```bash
lsvm        # list all VMs, running ones first
lsvm -r     # list running VMs only
lsvm -h     # help
```

## How it works

- Reads the VM list from `~/.vmware/inventory.vmls` and each VM's settings (name, guest OS, vCPUs, RAM) from its `.vmx` file.
- Matches running `vmware-vmx` processes to their `.vmx` path through `/proc/<pid>/cmdline`.
- Measures CPU usage by sampling the process CPU time over 0.5 seconds, and reads host memory used from `VmRSS`.
- Translates VMware guest OS ids into readable names (VMware's `windows9-64` is actually Windows 10).

## Install

```bash
curl -o ~/.local/bin/lsvm https://raw.githubusercontent.com/LumosDhia/lsvm/main/lsvm
chmod +x ~/.local/bin/lsvm
```

Requires bash, awk, `column` and `/proc`, so Linux only.
