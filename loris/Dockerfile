FROM phusion/passenger-full:0.9.10

MAINTAINER eliotj@princeton.edu

ENV HOME /root

# Update packages and install tools 
RUN apt-get update -y
RUN apt-get install -y wget git 

# Install pip and python libs
RUN apt-get install -y python-dev python-setuptools python-pip
RUN pip install --upgrade pip		
RUN pip2.7 install Werkzeug
RUN pip2.7 install configobj

# Install kakadu
WORKDIR /usr/local/lib
RUN wget --no-check-certificate https://github.com/sul-dlss/Djatoka/raw/master/lib/Linux-x86-32/libkdu_v60R.so
RUN chmod 755 libkdu_v60R.so

WORKDIR /usr/local/bin
RUN wget --no-check-certificate https://github.com/sul-dlss/Djatoka/raw/master/bin/Linux-x86-32/kdu_expand
RUN chmod 755 kdu_expand

RUN ln -s /usr/lib/`uname -i`-linux-gnu/libfreetype.so /usr/lib/
RUN ln -s /usr/lib/`uname -i`-linux-gnu/libjpeg.so /usr/lib/
RUN ln -s /usr/lib/`uname -i`-linux-gnu/libz.so /usr/lib/
RUN ln -s /usr/lib/`uname -i`-linux-gnu/liblcms.so /usr/lib/
RUN ln -s /usr/lib/`uname -i`-linux-gnu/libtiff.so /usr/lib/

RUN echo "/usr/local/lib" >> /etc/ld.so.conf && ldconfig

# Install Pillow
RUN apt-get install -y libjpeg8 libjpeg8-dev libfreetype6 libfreetype6-dev zlib1g-dev liblcms2-2 liblcms2-dev liblcms2-utils libtiff4-dev
RUN pip2.7 install Pillow

# Install loris
WORKDIR /opt

RUN git clone https://github.com/pulibrary/loris.git
RUN useradd -d /var/www/loris -s /sbin/false loris

WORKDIR /opt/loris

# Create image directory
RUN mkdir /usr/local/share/images

# Load example images
RUN cp -R tests/img/* /usr/local/share/images/

RUN ./setup.py install

WORKDIR /opt/loris/www

# Setup directory structure for passenger
RUN mkdir public && cp -r icons/ public/. && cp index.txt public/. && mv loris2.wsgi passenger_wsgi.py

# Startup scripts
ENTRYPOINT ["/sbin/my_init", "--skip-startup-files", "--skip-runit"]

ADD config/passenger.conf.erb /opt/loris/www/nginx.conf.erb
ADD config/loris_start.sh /bin/loris_start
RUN chmod +x /bin/loris_start
CMD ["/bin/loris_start"]