# Usar la imagen base de Ubuntu
FROM ubuntu
LABEL maintainer="Vivaldi <antonioluciovivaldi4seasons@gmail.com>"

# Configurar entorno no interactivo
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=America/Bogota DEBIAN_FRONTEND=noninteractive

# Instalar paquetes necesarios
RUN apt-get update && apt-get install -y \
    tzdata \
    net-tools \
    nano \
    file \
    wget \
    tar \
    sudo \
    adduser \
    curl \
    mysql-client \
    lsof \
    openssh-server \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Eliminar línea que usa `unminimize`, ya que no está disponible
# RUN yes | unminimize  

# Crear usuario "vivaldi"
RUN useradd -m -s /bin/bash vivaldi

# Cambiar contraseña del usuario root (considera eliminar esta línea por seguridad)
RUN echo "root:toor" | sudo chpasswd

# Agregar usuario "vivaldi" al grupo sudo sin requerir contraseña
RUN usermod -aG sudo vivaldi && echo "vivaldi ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/vivaldi
RUN chmod 0440 /etc/sudoers.d/vivaldi  # Corrección de permisos

# Cambiar al usuario "vivaldi" y definir directorio de trabajo
USER vivaldi
WORKDIR /home/vivaldi

# Ejecutar bash por defecto
CMD ["/bin/bash"]
