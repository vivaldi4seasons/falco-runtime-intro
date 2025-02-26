# Scenario A

> SSH Port forwarding (Tunelización de puertos)


Local Port Forwarding

    ssh -i gcp_remote -L 3306:localhost:3306 diegoposada@34.27.180.215
    ssh -i gcp_remote -N -L 3306:localhost:3306 diegoposada@34.27.180.215

Remote Port Forwarding

    ssh -i gcp_remote -R 3306:localhost:3306 diegoposada@34.27.180.215
    ssh -i gcp_remote -f -N -R 3306:localhost:3306 diegoposada@34.27.180.215  (background execution)

    ssh -i gcp_remote -R 3306:localhost:3306 diegoposada@34.27.180.215

    ssh -D 8080 user@server  (SSH Tunneling encapsula todo el tráfico TCP a través de una conexión segura SSH, actuando como una especie de VPN básica.)


| Comando                                                                 | Tipo                   | Descripción                                                                                  |
|-------------------------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------------|
| `ssh -i gcp_remote -L 3306:localhost:3306 diegoposada@34.27.180.215`    | SSH Port Forwarding    | Establece un túnel en el puerto local 3306 hacia el puerto 3306 del servidor remoto (localhost).|
| `ssh -i gcp_remote -N -L 3306:localhost:3306 diegoposada@34.27.180.215`  | SSH Port Forwarding    | Similar al anterior, pero sin ejecutar comandos remotos (`-N`), solo establece el túnel.       |
| `ssh -i gcp_remote -R 3306:localhost:3306 diegoposada@34.27.180.215`    | SSH Port Forwarding    | Establece un túnel desde el puerto remoto 3306 hacia el puerto 3306 de la máquina local.       |
| `ssh -D 8080 user@server`                                               | SSH Tunneling          | Establece un proxy SOCKS en el puerto 8080, lo que permite enrutar tráfico a través del servidor remoto. |


Redirigir el puerto 3306 (MySQL) del servidor remoto (34.27.180.215) al pod a través de una conexión SSH. En otras palabras, cualquier conexión a localhost:3306 en el pod se enviará al puerto 3306 del servidor remoto.


### Why can this be a security risk?



### How can these risks be mitigated?


## Scenario



Se puede observar que se levanta la relga en Falco.

    {"hostname":"falcox33-falco-obsec-s6nzm","output":"01:30:39.455536915: Warning ssh tunneling (user=<NA> user_loginuid=-1 program=ssh command=ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133 pid=3598 file=<NA> parent=bash gparent=containerd-shim ggparent=systemd gggparent=<NA> container_id=b83b3f40cab2 image=sha256) k8s.ns=falco-custom-lab k8s.pod=sneaky container=b83b3f40cab2","priority":"Warning","rule":"ssh tunneling/ssh port forwarding","source":"syscall","tags":["T1572","container","host","mitre_command_and_control","network"],"time":"2025-02-22T01:30:39.455536915Z", "output_fields": {"container.id":"b83b3f40cab2","container.image.repository":"sha256","evt.time":1740187839455536915,"fd.name":null,"k8s.ns.name":"falco-custom-lab","k8s.pod.name":"sneaky","proc.aname[2]":"containerd-shim","proc.aname[3]":"systemd","proc.aname[4]":null,"proc.cmdline":"ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133","proc.name":"ssh","proc.pid":3598,"proc.pname":"bash","user.loginuid":-1,"user.name":"<NA>"}}

