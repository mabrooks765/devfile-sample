FROM image-registry.openshift-image-registry.svc:5000/openshift/nodejs:16-ubi8

ENV ORACLE_CLIENT_BASIC_URL https://download.oracle.com/otn_software/linux/instantclient/195000/oracle-instantclient19.5-basic-19.5.0.0.0-1.x86_64.rpm
ENV ORACLE_CLIENT_BASIC_RPM oracle-instantclient-basic.x86_64.rpm
ENV ORACLE_CLIENT_LIB /usr/lib/oracle/19.5

ENV APP_SRC app
ENV APP_DEST /tmp/src/

# Set user to root to install RPMs and app source
USER root

# Add oracle bins via yum or rpm
RUN    curl --silent -o /tmp/${ORACLE_CLIENT_BASIC_RPM} ${ORACLE_CLIENT_BASIC_URL} \
    && ls -al /tmp/ \
    && cd /tmp \
    && yum --nogpgcheck localinstall -y /tmp/$ORACLE_CLIENT_BASIC_RPM \
    && yum --enablerepo=\* clean all \
    && /bin/rm /tmp/oracle* \
    && echo "${ORACLE_CLIENT_LIB}/client64/lib/" > /etc/ld.so.conf.d/oracle.conf \
    && ldconfig

# Add app source to tmp for assemble script
ADD $APP_SRC $APP_DEST

RUN chown -R 1001:0 /tmp/src

# Run assemble script
RUN /usr/libexec/s2i/assemble

# Set user back to image default
USER 1001

# set CMD location to run
CMD ["/usr/libexec/s2i/run"]