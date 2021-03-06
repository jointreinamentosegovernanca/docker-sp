# ------------------------------------------------------------------------
#
# Copyright 2017 WSO2, Inc. (http://wso2.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License
#
# ------------------------------------------------------------------------

# set to latest Centos
FROM centos:7
MAINTAINER Join Treinamentos e Governanca Team "training@jointreinamentos.com.br"

# set user configurations
ARG USER=wso2carbon
ARG USER_ID=802
ARG USER_GROUP=wso2
ARG USER_GROUP_ID=802
ARG USER_HOME=/home/${USER}
# set dependant files directory
ARG FILES=./files
# set jdk configurations
ARG JDK=jdk8u*
ARG JAVA_HOME=${USER_HOME}/java
# set wso2 product configurations
ARG WSO2_SERVER=wso2sp
ARG WSO2_SERVER_VERSION=4.3.0
ARG WSO2_SERVER_PACK=${WSO2_SERVER}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER_PACK}

# install required packages
RUN yum update -y && \
    yum install -y nc && \
    rm -rf /var/cache/yum/* && \
    echo 'printf "\n\
    Welcome to WSO2 Docker Resources \n\
    --------------------------------- \n\
    This Docker container comprises of a WSO2 product, running with its latest updates \n\
    which are under the End User License Agreement (EULA) 2.0. \n\
    Read more about EULA 2.0 here @ https://wso2.com/licenses/wso2-update/2.0 \n"' > /etc/profile.d/motd.sh

# create a user group and a user
RUN groupadd --system -g ${USER_GROUP_ID} ${USER_GROUP} && \
    useradd --system --create-home --home-dir ${USER_HOME} --no-log-init -g ${USER_GROUP_ID} -u ${USER_ID} ${USER}

# copy the jdk and wso2 product distributions to user's home directory
COPY --chown=wso2carbon:wso2 ${FILES}/${JDK} ${USER_HOME}/java/
COPY --chown=wso2carbon:wso2 ${FILES}/${WSO2_SERVER_PACK}/ ${WSO2_SERVER_HOME}/
# copy the Kafka client jars to the product home
COPY --chown=wso2carbon:wso2 ${FILES}/kafka_2.11_0.10.0.0_1.0.0.jar ${WSO2_SERVER_HOME}/lib/
COPY --chown=wso2carbon:wso2 ${FILES}/kafka_clients_0.10.0.0_1.0.0.jar ${WSO2_SERVER_HOME}/lib/
COPY --chown=wso2carbon:wso2 ${FILES}/metrics_core_2.2.0_1.0.0.jar ${WSO2_SERVER_HOME}/lib/
COPY --chown=wso2carbon:wso2 ${FILES}/scala_library_2.11.8_1.0.0.jar ${WSO2_SERVER_HOME}/lib/
COPY --chown=wso2carbon:wso2 ${FILES}/scala_parser_combinators_2.11_1.0.4_1.0.0.jar ${WSO2_SERVER_HOME}/lib/
COPY --chown=wso2carbon:wso2 ${FILES}/zkclient_0.8_1.0.0.jar ${WSO2_SERVER_HOME}/lib/
COPY --chown=wso2carbon:wso2 ${FILES}/zookeeper_3.4.6_1.0.0.jar ${WSO2_SERVER_HOME}/lib/
# copy mysql connector jar to the server as a third party library
COPY --chown=wso2carbon:wso2 ${FILES}/mysql-connector-java-*.jar ${WSO2_SERVER_HOME}/lib/

# set the user and work directory
USER ${USER_ID}
WORKDIR ${USER_HOME}

# set environment variables
ENV JAVA_HOME=${JAVA_HOME} \
    PATH=$JAVA_HOME/bin:$PATH \
    WSO2_SERVER_HOME=${WSO2_SERVER_HOME} \
    WORKING_DIRECTORY=${USER_HOME}
