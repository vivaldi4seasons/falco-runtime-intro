# Concepts

> La diferencia entre SSH Port Forwarding y SSH Tunneling es más una cuestión de terminología que de funcionalidad, ya que SSH Port Forwarding es un tipo específico de SSH Tunneling. Sin embargo, en la práctica, estos términos a veces se usan indistintamente.


## SSH Port forwarding (Tunelización de puertos)


El SSH Tunneling pertenece al marco MITRE ATT&CK dentro de la táctica TA0011 - Command and Control y a la técnica T1572 - Protocol Tunneling.

**Detalles**

- Táctica: Command and Control (TA0011)

- Técnica: Protocol Tunneling (T1572)

- Descripción: SSH Tunneling se utiliza para evadir detección y comunicación restringida al encapsular tráfico dentro de un canal cifrado de SSH, permitiendo a los atacantes acceder a recursos internos o exfiltrar datos sin ser detectados fácilmente.

**Subtécnicas relacionadas:**

T1090.002 - Connection Proxy: SSH → Uso de SSH como proxy para ocultar la ubicación real del atacante.
T1572 - Protocol Tunneling → SSH se usa para encapsular otros protocolos y evadir restricciones de red.



| Tipo     | Opción | Flujo de tráfico                 | Uso principal                                              |
|----------|--------|---------------------------------|-----------------------------------------------------------|
| **Local**  | `-L`  | Local → SSH → Destino remoto   | Acceder a servicios remotos desde tu máquina             |
| **Remoto** | `-R`  | Remoto → SSH → Tu máquina      | Hacer que servicios locales sean accesibles desde una máquina remota |
| **Dinámico** | `-D`  | Proxy SOCKS5 (Cualquier destino) | Proxy seguro para navegación o túneles VPN-like          |


##


sshuttle es una herramienta que permite tunelizar tráfico de red a través de una conexión SSH, similar a un VPN pero sin necesidad de configurar un servidor VPN. Es útil para acceder a redes privadas de forma segura sin exponer servicios al exterior.


### Summary


- SSH Port Forwarding 🔒 → Redirige puertos específicos.

- SSH Tunneling 🔄 → Encapsula tráfico TCP de manera más amplia (tipo VPN).  (usar tu servidor como proxy para todo el tráfico, usa SSH Tunneling con -D)