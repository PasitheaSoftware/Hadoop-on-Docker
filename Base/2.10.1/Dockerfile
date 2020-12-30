FROM ubuntu:latest

MAINTAINER Pasithea Consulting <flaforgia@pasithea-consulting.com>

WORKDIR /root

# Install openjdk 8
RUN apt-get update
RUN apt-get install -y openjdk-8-jdk openssh-server wget vim nano

# Install Hadoop 2.10.1
RUN wget https://downloads.apache.org/hadoop/common/hadoop-2.10.1/hadoop-2.10.1.tar.gz
RUN tar -xvzf hadoop-2.10.1.tar.gz
RUN mv hadoop-2.10.1 /opt
RUN rm hadoop-2.10.1.tar.gz

# set environment variables
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
ENV HADOOP_HOME=/opt/hadoop-2.10.1
ENV HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
ENV LD_LIBRARY_PATH=${HADOOP_HOME}/lib/native:$LD_LIBRARY_PATH
ENV PATH=$PATH:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin

# ssh without key
RUN ssh-keygen -t rsa -f ~/.ssh/id_rsa -P ''
RUN cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

RUN mkdir -p ~/hdfs/namenode
RUN mkdir -p ~/hdfs/datanode
RUN mkdir -p $HADOOP_HOME/logs

COPY config/* /tmp/

RUN mv /tmp/ssh_config ~/.ssh/config
RUN mv /tmp/hadoop-env.sh $HADOOP_CONF_DIR/hadoop-env.sh
RUN mv /tmp/hdfs-site.xml $HADOOP_CONF_DIR/hdfs-site.xml
RUN mv /tmp/core-site.xml $HADOOP_CONF_DIR/core-site.xml
RUN mv /tmp/mapred-site.xml $HADOOP_CONF_DIR/mapred-site.xml
RUN mv /tmp/yarn-site.xml $HADOOP_CONF_DIR/yarn-site.xml
RUN mv /tmp/slaves $HADOOP_CONF_DIR/slaves
RUN mv /tmp/start-hadoop.sh ~/start-hadoop.sh

RUN chmod +x ~/start-hadoop.sh
RUN chmod +x $HADOOP_HOME/sbin/start-dfs.sh
RUN chmod +x $HADOOP_HOME/sbin/start-yarn.sh  

# format namenode
RUN $HADOOP_HOME/bin/hdfs namenode -format

CMD [ "sh", "-c", "service ssh start; bash"]
