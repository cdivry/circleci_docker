FROM debian:stretch
MAINTAINER cdivry

# updates
COPY ./src/debian9_apt_sources.list /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y psmisc wget curl vim emacs-nox openssh-server \
	postgresql-client sudo telnet nmap python-pip
RUN pip install psycopg2

# entrypoint
COPY ./src/entrypoint_ssh.sh /entrypoint_ssh.sh
RUN chmod +x /entrypoint_ssh.sh

# timezone
RUN apt-get install -y tzdata
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# locales
RUN apt-get install -y locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
RUN dpkg-reconfigure -f noninteractive locales
RUN update-locale LANG=en_US.UTF-8
ENV LANG en_US.UTF-8

# adduser
RUN mkdir -pv /home/baarg/
RUN useradd -d /home/baarg/ -N -s /bin/bash baarg
RUN adduser baarg sudo
RUN echo "baarg:pouet" | chpasswd
RUN chown -R baarg /home/baarg

ENTRYPOINT ["/entrypoint_ssh.sh"]
