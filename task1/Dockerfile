FROM alpine:3.9

LABEL maintainer="pavlo.mykytyuk@gmail.com"


RUN apk --update add curl ca-certificates tar zip unzip wget bash openjdk8 so:libnss3.so

ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk
ENV PATH $PATH:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin


ARG TOMCAT_VERSION=8.5.39


ENV CATALINA_HOME=/opt/apache-tomcat-${TOMCAT_VERSION}
ENV PATH=$PATH:$CATALINA_HOME/bin
ARG TOMCATURL=http://apache.cp.if.ua/tomcat/tomcat-8/v8.5.39/bin/apache-tomcat-8.5.39.tar.gz
RUN addgroup tomcat \
    && adduser -D -G tomcat tomcat \
    && mkdir -p /opt \
    && curl ${TOMCATURL} -o /tmp/apache-tomcat-${TOMCAT_VERSION}.tar.gz \
    && tar xpfz /tmp/apache-tomcat-${TOMCAT_VERSION}.tar.gz -C /opt \
    && rm /tmp/apache-tomcat-${TOMCAT_VERSION}.tar.gz \
    && chown -R tomcat:tomcat /opt/apache-tomcat-${TOMCAT_VERSION} \
    && chown -R tomcat:tomcat $CATALINA_HOME \
    && chown -R tomcat:tomcat /home/tomcat

WORKDIR $CATALINA_HOME

RUN wget https://tomcat.apache.org/tomcat-8.0-doc/appdev/sample/sample.war \
    && mv sample.war $CATALINA_HOME/webapps/sample.war \
    && chown -R tomcat:tomcat $CATALINA_HOME
USER tomcat

EXPOSE 8080

CMD  $CATALINA_HOME/bin/catalina.sh run

