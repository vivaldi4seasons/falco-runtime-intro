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

    La regla detecta la creación de túneles SSH y redirección de puertos (port forwarding) dentro de contenedores, los cuales pueden ser utilizados por atacantes para evadir políticas de red, establecer canales encubiertos y obtener acceso no autorizado. En un entorno de Kubernetes, aplicar esta regla mitiga los riesgos de seguridad al prevenir movimientos laterales y la exfiltración de datos dentro del nodo.


## Trigger commands


| Tipo                          | Descripción                                                                                                                     | Comando                                                                 |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| **SSH Port Forwarding**        | Establece un túnel entre un puerto local y un puerto remoto en el servidor. Redirige tráfico entre ambos. <br> **Parámetros**: <br> `-f`: ejecuta el túnel en segundo plano. <br> `-N`: no ejecuta comandos remotos. <br> `-L`: redirige el puerto local. | `ssh -i gcp_remote -fNL 3306:localhost:3306 usuario@34.27.180.215`   |
| **SSH Reverse Port Forwarding**| Establece un túnel en reverso, redirigiendo un puerto remoto hacia el puerto local de un servidor. <br> **Parámetros**: <br> `-f`: ejecuta el túnel en segundo plano. <br> `-N`: no ejecuta comandos remotos. <br> `-R`: redirige el puerto remoto. | `ssh -i gcp_remote -fRN 3306:localhost:3306 usuario@34.27.180.215`   |
| **SSH Tunneling (Proxy SOCKS)**| Crea un proxy SOCKS en el servidor remoto para enrutar tráfico, proporcionando anonimato. <br> **Parámetros**: <br> `-D`: establece un proxy SOCKS en el puerto indicado. | `ssh -D 1080 -i gcp_remote usuario@34.27.180.21`                    |
| **SSH VPN Tunnel**             | Establece una conexión VPN utilizando la interfaz TUN/TAP sobre SSH para enrutar tráfico de red completo. <br> **Parámetros**: <br> `-w`: configura un túnel VPN utilizando interfaces TUN/TAP. | `ssh -w 0:0 -i gcp_remote usuario@34.27.180.21`                     |
| **sshuttle (VPN sobre SSH)**  | Proporciona una VPN completa sobre SSH, redirigiendo todo el tráfico de la red a través del servidor remoto. <br> **Parámetros**: <br> `-r`: establece la conexión remota. <br> `0.0.0.0/0`: redirige todo el tráfico de la red. <br> `--ssh-cmd`: comando SSH personalizado. | `sshuttle -r usuario@34.27.180.215 0.0.0.0/0 --ssh-cmd "ssh -i /.ssh/gcp_remote"` |



## Output


Se puede observar que se levanta la regla en Falco.

    {"hostname":"falcox33-falco-obsec-s6nzm","output":"01:30:39.455536915: Warning ssh tunneling (user=<NA> user_loginuid=-1 program=ssh command=ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133 pid=3598 file=<NA> parent=bash gparent=containerd-shim ggparent=systemd gggparent=<NA> container_id=b83b3f40cab2 image=sha256) k8s.ns=falco-custom-lab k8s.pod=sneaky container=b83b3f40cab2","priority":"Warning","rule":"ssh tunneling/ssh port forwarding","source":"syscall","tags":["T1572","container","host","mitre_command_and_control","network"],"time":"2025-02-22T01:30:39.455536915Z", "output_fields": {"container.id":"b83b3f40cab2","container.image.repository":"sha256","evt.time":1740187839455536915,"fd.name":null,"k8s.ns.name":"falco-custom-lab","k8s.pod.name":"sneaky","proc.aname[2]":"containerd-shim","proc.aname[3]":"systemd","proc.aname[4]":null,"proc.cmdline":"ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133","proc.name":"ssh","proc.pid":3598,"proc.pname":"bash","user.loginuid":-1,"user.name":"<NA>"}}


## References

    https://sshuttle.readthedocs.io/en/stable/manpage.html

    https://code.kryo.se/iodine/

    https://github.com/jpillora/chisel

    https://man.openbsd.org/ssh

