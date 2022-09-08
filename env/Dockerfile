FROM registry.access.redhat.com/ubi8/ubi-init@sha256:c7c5c582d4b916936042b97f056e44c7acbccafd2a43c05de67b65525e3bdc61

USER root

RUN yum install -y openssh-server sudo && \
  yum clean all && \
  mkdir /var/run/sshd && \
  useradd ansible -s /bin/bash && \
  mkdir -p /home/ansible/.ssh/ && \
  chmod 0700 /home/ansible/.ssh/ 

RUN /usr/bin/ssh-keygen -A

COPY ssh_config /home/ansible/.ssh/config
COPY ansible /home/ansible/.ssh/id_rsa
COPY ansible.pub /home/ansible/.ssh/id_rsa.pub
COPY ansible.pub /home/ansible/.ssh/authorized_keys

RUN chown -R ansible:ansible /home/ansible/  && \
  echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers 
  # && \
  # touch /etc/sysconfig/network

ENV container docker

EXPOSE 22

USER ansible:ansible

CMD ["sudo", "/usr/sbin/sshd", "-D"]

# CMD ["tail", "-f", "/dev/null"]