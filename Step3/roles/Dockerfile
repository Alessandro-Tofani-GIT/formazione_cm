FROM rockylinux:9
RUN dnf -y update && dnf -y install openssh-server 
CMD ["/usr/sbin/sshd", "-D"]
