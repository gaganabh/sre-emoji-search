Dockerfile


FROM centos:7

RUN yum update -y
RUN yum install epel-release -y
RUN yum install net-tools -y
RUN yum install git -y
RUN mkdir /data
ADD apache-tomcat-8.5.24.tar.gz /data
#RUN tar -xvf /data/apache-tomcat-8.5.24.tar.gz
ADD jdk-8u161-linux-x64.tar.gz /data
COPY jenkins.war /data
RUN mkdir /data/jenkinsHome
RUN echo "export JAVA_HOME=/data/jdk1.8.0_161" >> /etc/profile
RUN echo "export JRE_HOME=/data/jdk1.8.0_161/jre" >> /etc/profile
RUN echo "export PATH=$PATH:/data/jdk1.8.0_161/bin:/data/jdk1.8.0_161/jre/bin" >> /etc/profile
RUN echo "export CATALINA_HOME=/data/apache-tomcat-8.5.24" >> /etc/profile
RUN echo "export JENKINS_HOME=/data/jenkinsHome/" >> /etc/profile

RUN update-alternatives --install /usr/bin/java java /data/jdk1.8.0_161/bin/java 100
RUN update-alternatives --install /usr/bin/javac javac /data/jdk1.8.0_161/bin/javac 100

RUN cp /data/jenkins.war /data/apache-tomcat-8.5.24/webapps

EXPOSE 8080 3000
RUN git clone https://github.com/ahfarmer/emoji-search.git

RUN curl -sL https://dl.yarnpkg.com/rpm/yarn.repo -o /etc/yum.repos.d/yarn.repo

RUN yum install yarn -y

#EXPOSE 3000

RUN cd /emoji-search

#RUN yarn install

#RUN yarn start

CMD ["/data/apache-tomcat-8.5.24/bin/catalina.sh","run"]
