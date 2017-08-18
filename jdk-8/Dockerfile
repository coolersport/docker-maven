FROM anapsix/alpine-java:8_jdk_unlimited

ARG MAVEN_VERSION=3.2.5
ARG USER_HOME_DIR="/root"
ARG SHA=8c190264bdf591ff9f1268dc0ad940a2726f9e958e367716a09b8aaa7e74a755
ARG BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries

# upgrade tar, install curl and git
RUN apk add --update tar curl git \
  && rm -rf /var/cache/apk/* \
# install docker to run docker commands inside this container
  && curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-17.03.1-ce.tgz \
  && tar --strip-components=1 -xvzf docker-17.03.1-ce.tgz -C /usr/local/bin \
# install maven
  && mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && echo "${SHA}  /tmp/apache-maven.tar.gz" | sha256sum -c - \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV LC_ALL C
ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

COPY mvn-entrypoint.sh /usr/local/bin/mvn-entrypoint.sh
COPY settings-docker.xml /usr/share/maven/ref/

VOLUME "$USER_HOME_DIR/.m2"

ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]
CMD ["mvn"]
