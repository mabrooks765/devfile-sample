FROM registry.hub.docker.com/library/node:16

ENV ORACLE_CLIENT_BASIC_URL https://download.oracle.com/otn_software/linux/instantclient/1919000/oracle-instantclient19.19-basic-19.19.0.0.0-1.x86_64.rpm
ENV ORACLE_CLIENT_BASIC_RPM oracle-instantclient-basic.x86_64.rpm
ENV ORACLE_CLIENT_LIB /usr/lib/oracle/19.19

# Set user to root to install RPMs and app source
USER root

# Add oracle bins via yum or rpm
RUN curl --silent -o /tmp/${ORACLE_CLIENT_BASIC_RPM} ${ORACLE_CLIENT_BASIC_URL} \
    && ls -al /tmp/ \
    && cd /tmp \
    && apk add --allow-untrusted $ORACLE_CLIENT_BASIC_RPM \
    && /bin/rm /tmp/oracle* \
    && echo "${ORACLE_CLIENT_LIB}/client64/lib/" > /etc/ld.so.conf.d/oracle.conf \
    && ldconfig

# Add app source to tmp for assemble script
ADD . /tmp/src

RUN chown -R 1001:0 /tmp/src

# Run assemble script
RUN /usr/libexec/s2i/assemble

# Set user back to image default
USER 1001

# set CMD location to run
CMD ["/usr/libexec/s2i/run"]