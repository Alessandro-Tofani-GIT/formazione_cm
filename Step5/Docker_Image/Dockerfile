FROM docker:dind
RUN apk update && apk add --no-cache openssh-server shadow 
# Aggiunge l'utente ai sudoers per poter eseguire comandi come root senza password
RUN  id -u genericuser 2>/dev/null || useradd -m -s /bin/bash genericuser
RUN echo 'genericuser ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN ssh-keygen -A
#Configura il server SSH disabilitando:
 # - accesso root
# - autenticazione via password
# Abilita:
# - autenticazione tramite chiave pubblica
# - consente solo l'utente 'genericuser' per connessione SSH
RUN mkdir -p /var/run/sshd && \
    sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication no/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config && \
    echo 'AllowUsers genericuser' >> /etc/ssh/sshd_config
# Crea la directory .ssh nella home dell'utente
# Imposta i permessi corretti per sicurezza SSH
RUN mkdir -p /home/genericuser/.ssh && \
    chown -R genericuser:genericuser /home/genericuser/.ssh && \
    chmod 700 /home/genericuser/.ssh 
COPY id_key_genericuser.pub /home/genericuser/.ssh/authorized_keys
RUN chown genericuser:genericuser /home/genericuser/.ssh/authorized_keys && chmod 600 /home/genericuser/.ssh/authorized_keys 
EXPOSE 22
# avvia il demone SSH in modalità foreground (non daemonizzato)
CMD ["/usr/sbin/sshd", "-D"]