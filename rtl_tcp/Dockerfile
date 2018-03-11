# Dockerfile to create an image for a hass.io add-on that contains enough software to run an accessible rtl_tcp server
# This hass.io addon is based on James Fry's project here: https://github.com/james-fry/hassio-addons/blob/master/rtl4332mqtt
# which is in turn based on Chris Kacerguis' project here: https://github.com/chriskacerguis/honeywell2mqtt,
# which is in turn based on Marco Verleun's rtl2mqtt image here: https://github.com/roflmao/rtl2mqtt

# IMPORTANT: The container needs privileged access to /dev/bus/usb on the host.

ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

MAINTAINER Kyle Kamperschroer

LABEL Description="This image is used to start a script that will run an rtl_tcp server"

#
# First install software packages needed to compile rtl_433 and to publish MQTT events
#
RUN apk add --no-cache --virtual build-deps alpine-sdk cmake git libusb-dev && \
    mkdir /tmp/src && \
    cd /tmp/src && \
    git clone git://git.osmocom.org/rtl-sdr.git && \
    mkdir /tmp/src/rtl-sdr/build && \
    cd /tmp/src/rtl-sdr/build && \
    cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON -DCMAKE_INSTALL_PREFIX:PATH=/usr/local && \
    make && \
    make install && \
    chmod +s /usr/local/bin/rtl_* && \
    apk del build-deps && \
    rm -r /tmp/src && \
    apk add --no-cache libusb mosquitto-clients jq

#
# Define an environment variable
#
# Use this variable when creating a container to specify the MQTT broker host.
# ENV LISTEN_ADDRESS="192.168.86.15"

CMD rtl_tcp -a 192.168.86.15 -p 1234
