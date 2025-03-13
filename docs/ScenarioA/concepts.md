# Conceptos

> La diferencia entre SSH Port Forwarding y SSH Tunneling es más una cuestión de terminología que de funcionalidad, ya que SSH Port Forwarding es un tipo específico de SSH Tunneling. Sin embargo, en la práctica, estos términos a veces se usan indistintamente.


## SSH Port forwarding (Tunelización de puertos)


MITRE ATT&CK

- Tactic: Command and Control (TA0011)

- Technique: Protocol Tunneling (T1572)


**Descripción:**

SSH Tunneling permite encapsular tráfico dentro de un canal cifrado SSH para evadir la detección y comunicación restringida, permitiendo a los atacantes acceder a recursos internos o exfiltrar datos sin ser detectados fácilmente.

**Subtécnicas relacionadas:**

T1090.002 - Connection Proxy: SSH → Uso de SSH como proxy para ocultar la ubicación real del atacante.

T1572 - Protocol Tunneling → SSH se usa para encapsular otros protocolos y evadir restricciones de red.



| Tipo     | Opción | Flujo de tráfico                 | Uso principal                                              |
|----------|--------|---------------------------------|-----------------------------------------------------------|
| **Local**  | `-L`  | Local → SSH → Destino remoto   | Acceder a servicios remotos desde tu máquina.             |
| **Remoto** | `-R`  | Remoto → SSH → Tu máquina      | Hacer que servicios locales sean accesibles desde una máquina remota. |
| **Dinámico** | `-D`  | Proxy SOCKS5 (Cualquier destino) | Proxy seguro para navegación o túneles VPN-like.          |


## Tunneling Tools


Además de SSH, existen otras herramientas utilizadas para tunelizar tráfico de red y evadir controles de seguridad:

sshuttle: Permite redirigir tráfico de red a través de una conexión SSH, similar a una VPN, sin requerir configuración compleja.
chisel: Herramienta de tunelización TCP/UDP para crear túneles reversos y acceder a redes internas.
iodine: Permite tunelizar tráfico IP sobre DNS, útil para evadir restricciones de red.
Estas herramientas son comúnmente usadas para exfiltración de datos y establecer canales de comunicación no autorizados.


### Summary


- SSH Port Forwarding 🔒 → Redirige puertos específicos.

- SSH Tunneling 🔄 → Encapsula tráfico TCP de manera más amplia (tipo VPN).  (usar tu servidor como proxy para todo el tráfico, usa SSH Tunneling con -D)

- No se requiere kubectl para esta demostración. nginx ya se está ejecutando en el pod y se puede acceder a el directamente desde lá maquina local. Los tuneles SSH operan a nivel de red TCP, por lo que no requieren conocimientos en Kubernetes.

ssh -i gcp_remote -R 8080:127.0.0.1:80 diegoposada@34.27.180.215