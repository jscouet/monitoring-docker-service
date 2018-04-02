# create image of influxdb for receiving collectd info
FROM influxdb:1.4

RUN echo "new image influxdb for collectd listen"

RUN mkdir /usr/share/collectd/

#COPY influxdb/config/collectd.conf /etc/collectd/collectd.conf
COPY influxdb/config/influxdb.conf /etc/influxdb/influxdb.conf
COPY influxdb/config/types.db /usr/share/collectd/types.db

EXPOSE 8086 8086/tcp
EXPOSE 25826 25826/udp
