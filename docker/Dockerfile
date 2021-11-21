FROM node:latest
# RUN apt-get update
RUN apt-get install -y apt-transport-https ca-certificates
RUN sed -i '1i\# vvvvvvvvvvvvvvvvvvvvvvv默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free\n\
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free\n\
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free\n\
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free\n' /etc/apt/sources.list
RUN cat /etc/apt/sources.list
RUN apt-get update
#RUN apt-get upgrade -y
#RUN apt-get dist-upgrade -y
RUN apt-get install -y sudo git nano wget curl ntp build-essential 
RUN apt-get install -y libtool autotools-dev autoconf pkg-config libssl-dev 
RUN apt-get install -y libboost-all-dev git npm nodejs libminiupnpc-dev 
RUN apt-get install -y redis-server software-properties-common 
#RUN apt-get install -y fail2ban
# 配置fail2ban配置文件


#RUN sudo add-apt-repository ppa:bitcoin/bitcoin
#RUN sudo add-apt-repository ppa:pi-rho/dev
#RUN apt-get update
#RUN apt-get install libdb4.8-dev libdb4.8++-dev
RUN sudo systemctl enable redis-server
RUN ls -a /etc/init.d/
RUN sudo /etc/init.d/redis-server start
#RUN sudo systemctl enable fail2ban
#RUN sudo /etc/init.d/fail2ban start
RUN sudo systemctl enable ntp
RUN sudo /etc/init.d/ntp start
RUN wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
RUN /bin/bash -c "source ~/.bashrc"
#RUN /bin/bash -c "nvm install v8.1.4"
#RUN /bin/bash -c "nvm use v8.1.4"
RUN /bin/bash -c "npm update -g"
RUN /bin/bash -c "npm install -g pm2@latest"
RUN /bin/bash -c "npm install -g npm@latest"
RUN /bin/bash -c "pm2 init"



WORKDIR /opt/
RUN git clone https://github.com/EasyX-Community/easyNOMP.git

WORKDIR /opt/EasyNOMP

RUN /bin/bash -c npm install -g multi-hashing@latest
RUN /bin/bash -c npm install
RUN /bin/bash -c npm update
RUN /bin/bash -c npm audit fix

RUN rm config_example.json
RUN rm -rf coins
RUN rm -rf pool_configs
RUN ln -s /opt/config/config.json /opt/BootNOMP/config.json
RUN ln -s /opt/config/coins /opt/BootNOMP/coins
RUN ln -s /opt/config/pool_configs /opt/BootNOMP/pool_configs

CMD sudo /etc/init.d/redis-server restart; sudo /etc/init.d/ntp restart; /bin/bash -c "pm2 start init.js -i max --watch --name pool"
# CMD sudo /etc/init.d/fail2ban
