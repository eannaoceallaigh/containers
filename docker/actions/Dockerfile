FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive

ENV pip_packages "ansible passlib"

RUN apt update && \
  apt install -y ca-certificates curl

RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash -

# Install dependencies.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       apt-utils \
       build-essential \
       git \
       locales \
       libffi-dev \
       libssl-dev \
       libyaml-dev \
       nodejs \
       python3-dev \
       python3-setuptools \
       python3-pip \
       python3-yaml \
       ssh \
       sshpass \
       software-properties-common \
       rsyslog systemd systemd-cron sudo iproute2 \
       unzip \
    && apt-get clean \
    && rm -Rf /var/lib/apt/lists/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man

# Fix potential UTF-8 errors with ansible-test.
RUN locale-gen en_US.UTF-8

# Remove useless Python environment warning flag.
RUN rm -rf /usr/lib/python3.12/EXTERNALLY-MANAGED

# Install Ansible via Pip.
RUN pip3 install $pip_packages

COPY initctl_faker .
RUN chmod +x initctl_faker && rm -fr /sbin/initctl && ln -s /initctl_faker /sbin/initctl

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo "[local]\nlocalhost ansible_connection=local" > /etc/ansible/hosts

RUN useradd -ms /bin/bash actions
RUN git clone https://github.com/tfutils/tfenv.git /opt/tfenv
RUN ln -s /opt/tfenv/bin/* /bin

RUN chown -R actions:actions /opt/tfenv

USER actions