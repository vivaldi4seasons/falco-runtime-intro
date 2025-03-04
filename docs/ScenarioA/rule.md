# Rule: SSH tunneling/SSH port forwarding [custom]

    - macro: ssh_tunneling
    condition: >
        ((proc.name=ssh or proc.name=sshd) and proc.args contains "-L" and
        proc.args contains "-N")

    - rule: ssh tunneling/ssh port forwarding
    desc: >
        ssh tunneling
    condition: >
        spawned_process
        and container
        and ((ssh_tunneling))
    enabled: false
    output: >
        ssh tunneling (user=%user.name user_loginuid=%user.loginuid program=%proc.name
        command=%proc.cmdline pid=%proc.pid file=%fd.name parent=%proc.pname gparent=%proc.aname[2] ggparent=%proc.aname[3] gggparent=%proc.aname[4] container_id=%container.id image=%container.image.repository)
    priority: WARNING
    tags: [host, container, network, mitre_command_and_control, T1572]


## Relevance


    Helps detect SSH tunneling and port forwarding inside containers, which attackers can use to bypass network policies, establish covert channels, and gain unauthorized access. In a Kubernetes environment, enforcing this rule mitigates security risks by preventing lateral movement and data exfiltration within the cluster.


## Trigger commands




## Output


Se puede observar que se levanta la regla en Falco.

    {"hostname":"falcox33-falco-obsec-s6nzm","output":"01:30:39.455536915: Warning ssh tunneling (user=<NA> user_loginuid=-1 program=ssh command=ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133 pid=3598 file=<NA> parent=bash gparent=containerd-shim ggparent=systemd gggparent=<NA> container_id=b83b3f40cab2 image=sha256) k8s.ns=falco-custom-lab k8s.pod=sneaky container=b83b3f40cab2","priority":"Warning","rule":"ssh tunneling/ssh port forwarding","source":"syscall","tags":["T1572","container","host","mitre_command_and_control","network"],"time":"2025-02-22T01:30:39.455536915Z", "output_fields": {"container.id":"b83b3f40cab2","container.image.repository":"sha256","evt.time":1740187839455536915,"fd.name":null,"k8s.ns.name":"falco-custom-lab","k8s.pod.name":"sneaky","proc.aname[2]":"containerd-shim","proc.aname[3]":"systemd","proc.aname[4]":null,"proc.cmdline":"ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133","proc.name":"ssh","proc.pid":3598,"proc.pname":"bash","user.loginuid":-1,"user.name":"<NA>"}}


## References

    https://sshuttle.readthedocs.io/en/stable/manpage.html