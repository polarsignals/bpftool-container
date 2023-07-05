# bpftool container

It occasionally happens that we have to debug low level eBPF details. It's useful to have a container image ready with the tools we need.

For example this one-liner can be used to dump BPF logs from a node (replace the `nodeName`):

```
kubectl run --rm -it --privileged --image=fedora --overrides='{ "apiVersion": "v1", "spec": { "nodeName": "<nodeName>", "hostPID": true, "securityContext": { "privileged": true}, "containers": [{"name":"debug", "command": ["/bin/sh", "-c", "sudo bpftool prog tracelog"], "stdin": true, "stdinOnce": true, "tty": true, "securityContext": {"privileged": true}, "image": "ghcr.io/polarsignals/bpftool:latest","volumeMounts":[{"mountPath": "/sys/fs/bpf", "name":"bpffs"}]}], "volumes": [{"name": "bpffs","hostPath":{"path":"/sys/fs/bpf"}}] } }' debug
```
