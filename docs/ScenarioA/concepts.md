# Concepts


## SSH Port forwarding (Tunelización de puertos)


El SSH Tunneling pertenece al marco MITRE ATT&CK dentro de la táctica TA0011 - Command and Control y a la técnica T1572 - Protocol Tunneling.

**Detalles**

- Táctica: Command and Control (TA0011)

- Técnica: Protocol Tunneling (T1572)

- Descripción: SSH Tunneling se utiliza para evadir detección y comunicación restringida al encapsular tráfico dentro de un canal cifrado de SSH, permitiendo a los atacantes acceder a recursos internos o exfiltrar datos sin ser detectados fácilmente.

**Subtécnicas relacionadas:**

T1090.002 - Connection Proxy: SSH → Uso de SSH como proxy para ocultar la ubicación real del atacante.
T1572 - Protocol Tunneling → SSH se usa para encapsular otros protocolos y evadir restricciones de red.

Hay 3 tipos.


- Local port forwarding -L
- Remote port forwarding -R
- Dynamic port forwarding -D
- Pivoting




### Summary


- SSH Port Forwarding 🔒 → Redirige puertos específicos.

- SSH Tunneling 🔄 → Encapsula tráfico TCP de manera más amplia (tipo VPN).  (usar tu servidor como proxy para todo el tráfico, usa SSH Tunneling con -D)