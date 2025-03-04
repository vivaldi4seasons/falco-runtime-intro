# Rule: SSH tunneling/SSH port forwarding [custom]


    - macro: others_tunnels_tools
    condition: >
        ((proc.name=sshuttle) or (proc.name=chisel) or (proc.name=iodine))

    - rule: SSH tunneling/SSH port forwarding detected inside a container
    desc: >
        Detects SSH tunneling and port forwarding inside a container, which attackers may use to bypass network controls, create hidden communication 
        channels, and access internal resources. It analyzes process names and command-line arguments to identify suspicious activity.
    condition: >
        spawned_process
        and container
        and ((ssh_tunneling) or (others_tunnels_tools))
    enabled: true
    output: >
        Detected SSH connection for SSH tunneling/port forwarding (user=%user.name user_loginuid=%user.loginuid program=%proc.name
        command=%proc.cmdline pid=%proc.pid file=%fd.name parent=%proc.pname gparent=%proc.aname[2] ggparent=%proc.aname[3] gggparent=%proc.aname[4] container_id=%container.id image=%container.image.repository)
    priority: WARNING
    tags: [host, container, network, mitre_command_and_control, T1572]


## Relevance


    Helps detect SSH tunneling and port forwarding inside containers, which attackers can use to bypass network policies, establish covert channels, and gain unauthorized access. In a Kubernetes environment, enforcing this rule mitigates security risks by preventing lateral movement and data exfiltration within the cluster.


## Trigger commands

    ssh -i gcp_remote -L 3306:localhost:3306 diegoposada@34.27.180.215 OK
    ssh -i gcp_remote -NL 3306:localhost:3306 diegoposada@34.27.180.215 OK
    ssh -i gcp_remote -fNL 3306:localhost:3306 diegoposada@34.27.180.215 OK 
    ssh -D 1080 -i gcp_remote diegoposada@34.27.180.21 (Proxy SOCKS5)  OK
    ssh -w 0:0 -i gcp_remote diegoposada@34.27.180.21 (VPN basado en SSH, Interfaz TUN/TAP) OK

    ssh -i gcp_remote -R 3306:localhost:3306 diegoposada@34.27.180.215 OK
    sshuttle -r diegoposada@34.27.180.215 0.0.0.0/0 --ssh-cmd "ssh -i /.ssh/gcp_remote" OK


## Output


Se puede observar que se levanta la regla en Falco.

    {"hostname":"falcox33-falco-obsec-s6nzm","output":"01:30:39.455536915: Warning ssh tunneling (user=<NA> user_loginuid=-1 program=ssh command=ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133 pid=3598 file=<NA> parent=bash gparent=containerd-shim ggparent=systemd gggparent=<NA> container_id=b83b3f40cab2 image=sha256) k8s.ns=falco-custom-lab k8s.pod=sneaky container=b83b3f40cab2","priority":"Warning","rule":"ssh tunneling/ssh port forwarding","source":"syscall","tags":["T1572","container","host","mitre_command_and_control","network"],"time":"2025-02-22T01:30:39.455536915Z", "output_fields": {"container.id":"b83b3f40cab2","container.image.repository":"sha256","evt.time":1740187839455536915,"fd.name":null,"k8s.ns.name":"falco-custom-lab","k8s.pod.name":"sneaky","proc.aname[2]":"containerd-shim","proc.aname[3]":"systemd","proc.aname[4]":null,"proc.cmdline":"ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133","proc.name":"ssh","proc.pid":3598,"proc.pname":"bash","user.loginuid":-1,"user.name":"<NA>"}}


## References

    https://sshuttle.readthedocs.io/en/stable/manpage.html

    https://code.kryo.se/iodine/

    https://github.com/jpillora/chisel

    https://man.openbsd.org/ssh

