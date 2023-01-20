# Creamos la imagen a partir de ubuntu versión 18.04
FROM ubuntu:18.04

# Damos información sobre la imagen que estamos creando
LABEL \
	version="1.0" \
	description="Ubuntu + Apache2 + virtual host" \
	creationDate="23-11-2019" \
	maintainer="Nora San Saturnino <nsansaturnino@birt.eus>"

# Instalamos el editor nano
RUN \
	apt-get update \
	&& apt-get install nano \ 
	&& apt-get -y install proftpd \
	&& apt-get -y install openssh-server \
	&& apt-get install apache2 --yes \
	&& apt-get install git --yes \
	&& mkdir /var/www/html/sitioweb1 /var/www/html/sitioweb2  \
	&& user_password="1234"

	

	


# Copiamos el index al directorio por defecto del servidor Web 	 apt-get install openssh-server \
COPY index1.html index2.html sitioweb1.conf sitioweb2.conf sitioweb1.key sitioweb1.cer jone1.key jone1.crt id_rsa.pub /

RUN \
	mv /index1.html /var/www/html/sitioweb1/index.html \
	&& mv /index2.html /var/www/html/sitioweb2/index.html \
	&& mv /sitioweb1.conf /etc/apache2/sites-available \
	&& a2ensite sitioweb1 \
	&& mv /sitioweb2.conf /etc/apache2/sites-available \
	&& a2ensite sitioweb2 \
	&& mv /sitioweb1.key /etc/ssl/private \
	&& mv /sitioweb1.cer /etc/ssl/certs \
	&& a2enmod ssl \
	
	&& eval `ssh-agent -s` \
	&& mkdir ~/.ssh \
	&& mv /id_rsa.pub ~/.ssh/id_rsa.pub \
	&& echo "StrictHostKeyChecking no" >> /etc/ssh/ssh_config \
	&& chmod go-w /root  \
    && chmod 700 /root/.ssh \
    && chmod 600 /root/.ssh/id_rsa.pub \
	&& ssh-add ~/.ssh/id_rsa.pub \

	
	&& git clone git@github.com:deaw-birt/deaw03-te1-ftp-anonimo.git /srv/ftp/proyecto \
	
	
	&& rm /etc/proftpd/proftpd.conf \
	&& rm /etc/proftpd/tls.conf \
	&& rm /etc/ftpusers \
	&& rm /etc/ssh/sshd_config \
	&& useradd jone1 -m -d /var/www/html/sitioweb1 -g nogroup -s /usr/sbin/nologin \
	&& useradd jone2 -m -d /var/www/html/sitioweb2 -g nogroup -s /bin/bash \
	&& useradd jone -m -d /srv/ftp -g nogroup -s /usr/sbin/nologin \


	
COPY proftpd.conf tls.conf ftpusers sshd_config /

RUN \
	mv /proftpd.conf /etc/proftpd/proftpd.conf \
	&& mv /tls.conf /etc/proftpd/tls.conf \
	&& mv /jone1.key /etc/ssl/jone1.key \
	&& mv /jone1.crt /etc/ssl/certs/jone1.crt \
	&& mv /sshd_config /etc/ssh/sshd_config \
	&& mv /ftpusers /etc/ftpusers 
	
# Indicamos el puerto que utiliza la imagen EXPOSE 22 EXPOSE 33
EXPOSE 80
EXPOSE 443
EXPOSE 21
EXPOSE 50000-50300
EXPOSE 2220
EXPOSE 22

