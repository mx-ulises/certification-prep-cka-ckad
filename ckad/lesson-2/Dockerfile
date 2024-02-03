FROM fedora:latest

RUN dnf install -y openssh-server procps-ng iproute && \
ssh-keygen -A

CMD ["/usr/sbin/sshd"]
