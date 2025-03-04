# Scenario A

> SSH Port forwarding (Tunelización de puertos)



<!-- <img src="../../assets/memoryDumping.webp" align="center" width="50%" height="50%"/> -->

<div style="text-align: center;">
  <img src="../../assets/ssh-port-forwarding.webp" width="50%" height="50%" />
</div>


Local Port Forwarding

    ssh -i gcp_remote -L 3306:localhost:3306 diegoposada@34.27.180.215
    ssh -i gcp_remote -N -L 3306:localhost:3306 diegoposada@34.27.180.215

Remote Port Forwarding

    ssh -i gcp_remote -R 3306:localhost:3306 diegoposada@34.27.180.215
    ssh -i gcp_remote -f -N -R 3306:localhost:3306 diegoposada@34.27.180.215  (background execution)

    ssh -i gcp_remote -R 3306:localhost:3306 diegoposada@34.27.180.215

Dynamic Port Forwarding

    ssh -D 8080 user@server  (SSH Tunneling encapsula todo el tráfico TCP a través de una conexión segura SSH, actuando como una especie de VPN básica.)



    ssh -w 0:0 usuario@servidor




| Tipo                   | Descripción                                                                                  | Comando                                                                 |
|------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| SSH Port Forwarding    | Establece un túnel en el puerto local 3306 hacia el puerto 3306 del servidor remoto (localhost).| `ssh -i gcp_remote -L 3306:localhost:3306 diegoposada@34.27.180.215`    |
| SSH Port Forwarding    | Similar al anterior, pero sin ejecutar comandos remotos (`-N`), solo establece el túnel.       | `ssh -i gcp_remote -NL 3306:localhost:3306 diegoposada@34.27.180.215`   |
| SSH Port Forwarding    | Similar al anterior, pero ejecuta el túnel en segundo plano (`-f`).                           | `ssh -i gcp_remote -fNL 3306:localhost:3306 diegoposada@34.27.180.215`  |
| SSH Tunneling          | Establece un proxy SOCKS en el puerto 1080, lo que permite enrutar tráfico a través del servidor remoto. | `ssh -D 1080 -i gcp_remote diegoposada@34.27.180.21`                   |
| SSH Tunneling          | Crea un túnel VPN basado en SSH utilizando la interfaz TUN/TAP.                              | `ssh -w 0:0 -i gcp_remote diegoposada@34.27.180.21`                    |



Redirigir el puerto 3306 (MySQL) del servidor remoto (34.27.180.215) al pod a través de una conexión SSH. En otras palabras, cualquier conexión a localhost:3306 en el pod se enviará al puerto 3306 del servidor remoto.


### Why can this be a security risk?

🔸Exposición del puerto local: El port forwarding (-L o -R) puede exponer puertos locales o remotos a accesos no autorizados. Si un puerto se reenvía de forma inapropiada, personas no autorizadas podrían acceder a servicios internos a través de un puerto que, de otra manera, estaría cerrado.

🔸 Túneles sin cifrar o inseguridad en el canal SSH: Aunque el canal SSH está cifrado, si las credenciales de acceso (como las claves SSH) no están bien protegidas o si la máquina local es comprometida, un atacante podría utilizar ese túnel para realizar actividades maliciosas o interceptar comunicaciones.

🔸 Uso de un proxy SOCKS o VPN: El comando ssh -D establece un proxy SOCKS y el comando ssh -w establece un túnel VPN, lo cual podría permitir que el tráfico se enrute a través de servidores no confiables. Si no se asegura que el servidor SSH sea confiable y seguro, un atacante podría tener acceso a toda la comunicación a través de esos túneles y potencialmente robar información sensible.

🔸 Acceso a redes privadas o recursos internos: Al hacer tunneling de puertos o utilizar VPN, puedes acceder a redes internas o recursos privados. Si no se configuran correctamente las reglas de firewall o se hacen cambios inadvertidos, estos túneles podrían exponer redes sensibles a posibles atacantes.

 

### How can these risks be mitigated?


## Scenario



Se puede observar que se levanta la regla en Falco.

    {"hostname":"falcox33-falco-obsec-s6nzm","output":"01:30:39.455536915: Warning ssh tunneling (user=<NA> user_loginuid=-1 program=ssh command=ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133 pid=3598 file=<NA> parent=bash gparent=containerd-shim ggparent=systemd gggparent=<NA> container_id=b83b3f40cab2 image=sha256) k8s.ns=falco-custom-lab k8s.pod=sneaky container=b83b3f40cab2","priority":"Warning","rule":"ssh tunneling/ssh port forwarding","source":"syscall","tags":["T1572","container","host","mitre_command_and_control","network"],"time":"2025-02-22T01:30:39.455536915Z", "output_fields": {"container.id":"b83b3f40cab2","container.image.repository":"sha256","evt.time":1740187839455536915,"fd.name":null,"k8s.ns.name":"falco-custom-lab","k8s.pod.name":"sneaky","proc.aname[2]":"containerd-shim","proc.aname[3]":"systemd","proc.aname[4]":null,"proc.cmdline":"ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133","proc.name":"ssh","proc.pid":3598,"proc.pname":"bash","user.loginuid":-1,"user.name":"<NA>"}}

 con sshuttle no aparece tun0 es decir con ssh -D


 dns tunneling