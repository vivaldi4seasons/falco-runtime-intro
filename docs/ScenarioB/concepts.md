# Concepts


## OS Credential Dumping


OS Credential Dumping
La técnica OS Credential Dumping pertenece al marco MITRE ATT&CK dentro de la táctica TA0006 - Credential Access y a la técnica T1003 - OS Credential Dumping.

Detalles

Táctica: Credential Access (TA0006)

Técnica: OS Credential Dumping (T1003)

Descripción: OS Credential Dumping se refiere al proceso en el cual un atacante obtiene credenciales de un sistema operativo. Esto se puede lograr a través de la extracción de hashes de contraseñas, contraseñas en texto claro o credenciales almacenadas de forma insegura en archivos del sistema, como los de la base de datos de SAM (Security Accounts Manager) en Windows. En sistemas Linux y contenedores, las credenciales pueden ser extraídas desde archivos de configuración o la memoria del sistema, utilizando herramientas como dd, dd3, o memdump para volcar los contenidos de la memoria o de procesos específicos que almacenan credenciales.

En sistemas Linux, el proceso de dumping de credenciales a menudo involucra la obtención de contraseñas en texto claro desde archivos como /etc/shadow o la memoria de procesos como sshd, que almacenan claves de autenticación y credenciales de acceso. En contenedores, los atacantes pueden emplear técnicas similares para acceder a las credenciales del contenedor al volcar la memoria o aprovechar configuraciones inseguras dentro del contenedor.

