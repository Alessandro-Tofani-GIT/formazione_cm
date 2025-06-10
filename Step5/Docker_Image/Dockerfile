FROM docker:dind

# Installa OpenSSH server e altre utilità necessarie
RUN apk update && apk add --no-cache \
    openssh \
    sudo \
    bash \
    shadow

# Configura la password per genericuser (cambia 'password123' con quella che preferisci)

# Crea l'utente genericuser
RUN adduser -D -s /bin/bash genericuser
RUN echo 'genericuser:password123' | chpasswd

# Crea la directory .ssh per genericuser
RUN mkdir -p /home/genericuser/.ssh
RUN chown genericuser:genericuser /home/genericuser/.ssh
RUN chmod 700 /home/genericuser/.ssh

# Aggiungi genericuser al gruppo docker per permettere l'uso di Docker
RUN addgroup genericuser docker

# Configura SSH
RUN ssh-keygen -A
RUN mkdir -p /var/run/sshd

# Modifica la configurazione SSH per usare solo chiavi
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config
RUN sed -i 's/#AuthorizedKeysFile/AuthorizedKeysFile/' /etc/ssh/sshd_config
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config

# Copia la chiave pubblica (deve essere presente nella directory di build)
COPY ssh_keys/id_rsa.pub /home/genericuser/.ssh/authorized_keys
RUN chown genericuser:genericuser /home/genericuser/.ssh/authorized_keys
RUN chmod 600 /home/genericuser/.ssh/authorized_keys

RUN echo 'genericuser ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# Crea lo script di avvio
RUN echo '#!/bin/bash' > /start.sh && \
    echo 'dockerd-entrypoint.sh &' >> /start.sh && \
    echo 'sleep 5' >> /start.sh && \
    echo '/usr/sbin/sshd -D' >> /start.sh && \
    chmod +x /start.sh

# Esponi la porta SSH
EXPOSE 22

# Avvia Docker daemon e SSH server
CMD ["/start.sh"]
