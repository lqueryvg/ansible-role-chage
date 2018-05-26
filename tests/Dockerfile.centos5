# - run sshd with builder user's public key
#   so that builder can ssh directly in as root
#
FROM centos:5

RUN                                                                                              \
  mkdir -p /var/cache/yum/{base,extras,updates,epel,libselinux}                                  \
  && echo "http://vault.centos.org/5.11/os/x86_64/" > /var/cache/yum/base/mirrorlist.txt         \
  && echo "http://vault.centos.org/5.11/extras/x86_64/" > /var/cache/yum/extras/mirrorlist.txt   \
  && echo "http://vault.centos.org/5.11/updates/x86_64/" > /var/cache/yum/updates/mirrorlist.txt \
  && echo "http://ftp-stud.hs-esslingen.de/pub/Mirrors/archive.fedoraproject.org/epel/5/x86_64/" \
    > /var/cache/yum/epel/mirrorlist.txt \
  && cp /var/cache/yum/extras/mirrorlist.txt /var/cache/yum/libselinux/

RUN yum -y install openssh-server
RUN yum -y install python-simplejson wget

RUN wget --no-check-certificate                                                                 \
      "http://download.fedoraproject.org/pub/archive/epel/5/x86_64/epel-release-5-4.noarch.rpm" \
    && rpm -i epel-release-5-4.noarch.rpm                                                       \
    && yum install -y python26                                                                  \
    && mv /usr/bin/python /usr/bin/python-                                                      \
    && ln -sf /usr/bin/python26 /usr/bin/python

RUN sed -i \
  's/PermitRootLogin without-password/PermitRootLogin yes/' \
  /etc/ssh/sshd_config
RUN sed -i \
  's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' \
  /etc/pam.d/sshd
RUN service sshd start  # to generate host keys
RUN mkdir -m 700 /root/.ssh
COPY ./tmp/id_rsa.pub /root/.ssh/authorized_keys
RUN chmod 400 /root/.ssh/authorized_keys
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
