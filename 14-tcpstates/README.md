## eBPF 入门实践教程：

## 备注

对于使用 `libbpf-bootstrap` 的开发，具体见 [tcpstates-libbpf-bootstrap.md](tcpstates-libbpf-bootstrap.md)

## origin

origin from:

https://github.com/iovisor/bcc/blob/master/libbpf-tools/tcpconnlat.bpf.c

## Compile and Run

Compile:

```shell
docker run -it -v `pwd`/:/src/ yunwei37/ebpm:latest
```
Run:

```shell
sudo ./ecli run package.json
```

## details in bcc

Demonstrations of tcpstates, the Linux BPF/bcc version.


tcpstates prints TCP state change information, including the duration in each
state as milliseconds. For example, a single TCP session:
```console
# tcpstates
SKADDR           C-PID C-COMM     LADDR           LPORT RADDR           RPORT OLDSTATE    -> NEWSTATE    MS
ffff9fd7e8192000 22384 curl       100.66.100.185  0     52.33.159.26    80    CLOSE       -> SYN_SENT    0.000
ffff9fd7e8192000 0     swapper/5  100.66.100.185  63446 52.33.159.26    80    SYN_SENT    -> ESTABLISHED 1.373
ffff9fd7e8192000 22384 curl       100.66.100.185  63446 52.33.159.26    80    ESTABLISHED -> FIN_WAIT1   176.042
ffff9fd7e8192000 0     swapper/5  100.66.100.185  63446 52.33.159.26    80    FIN_WAIT1   -> FIN_WAIT2   0.536
ffff9fd7e8192000 0     swapper/5  100.66.100.185  63446 52.33.159.26    80    FIN_WAIT2   -> CLOSE       0.006
^C
```
This showed that the most time was spent in the ESTABLISHED state (which then
transitioned to FIN_WAIT1), which was 176.042 milliseconds.

The first column is the socked address, as the output may include lines from
different sessions interleaved. The next two columns show the current on-CPU
process ID and command name: these may show the process that owns the TCP
session, depending on whether the state change executes synchronously in
process context. If that's not the case, they may show kernel details.

