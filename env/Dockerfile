FROM registry.access.redhat.com/ubi8/python-39@sha256:84639af111ab2b3fd7ca325f0912e405c117df31c976446b2a1902948f65a61d

USER root

RUN yum install -y openssh-server sudo && \
  mkdir /var/run/sshd && \
  useradd ansible -s /bin/bash && \
  mkdir -p /home/ansible/.ssh/ && \
  chmod 0700 /home/ansible/.ssh/ && \
  grep -q "ChallengeResponseAuthentication" /etc/ssh/sshd_config && sed -i "/^[^#]*ChallengeResponseAuthentication[[:space:]]yes.*/c\ChallengeResponseAuthentication no" /etc/ssh/sshd_config || echo "ChallengeResponseAuthentication no" >> /etc/ssh/sshd_config && \
  grep -q "^[^#]*PasswordAuthentication" /etc/ssh/sshd_config && sed -i "/^[^#]*PasswordAuthentication[[:space:]]yes/c\PasswordAuthentication no" /etc/ssh/sshd_config || echo "PasswordAuthentication no" >> /etc/ssh/sshd_config

##TODO: Move some of above to a shell script and call that in CMD 

COPY ssh_config /home/ansible/.ssh/config
COPY ansible /home/ansible/.ssh/id_rsa
COPY ansible.pub /home/ansible/.ssh/id_rsa.pub
COPY ansible.pub /home/ansible/.ssh/authorized_keys

RUN chown -R ansible:ansible /home/ansible/ && \
  echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
  touch /etc/sysconfig/network

ENV container docker

EXPOSE 22
RUN /usr/bin/ssh-keygen -A 
# RUN systemctl start sshd
# RUN /usr/sbin/sshd -D

USER ansible:ansible

ENTRYPOINT ["/sbin/init"]
# CMD ["sudo", "/usr/sbin/sshd", "-D"]
