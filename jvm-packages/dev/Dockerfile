#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
#
FROM centos:7

ENV JAVA_VERSION=8 \
    JAVA_UPDATE=131 \
    JAVA_BUILD=11 \
    JAVA_PATH=d54c1d3a095b4ff2b6607d096fa80163 \
    JAVA_HOME="/usr/lib/jvm/default-jvm"

# Install all basic requirements
RUN yum -y update && \
    yum install -y tar unzip wget xz git centos-release-scl yum-utils bunzip2 bzip2 && \
    yum-config-manager --enable centos-sclo-rh-testing && \
    yum -y update && \
    yum install -y devtoolset-7-gcc devtoolset-7-binutils devtoolset-7-gcc-c++ make && \
    # Java
    wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
      -P /tmp \
      "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/${JAVA_PATH}/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    tar -xzf "/tmp/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" -C /tmp && \
    mkdir -p "/usr/lib/jvm" && \
    mv "/tmp/jdk1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle" && \
    ln -s "java-${JAVA_VERSION}-oracle" "$JAVA_HOME" && \
    ln -s "$JAVA_HOME/bin/"* "/usr/bin/" && \
    rm -rf "$JAVA_HOME/"*src.zip && \
    wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" "http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION}/jce_policy-${JAVA_VERSION}.zip" && \
    unzip -jo -d "${JAVA_HOME}/jre/lib/security" "jce_policy-${JAVA_VERSION}.zip" && \
    rm "${JAVA_HOME}/jre/lib/security/README.txt" && \
    # Python
    wget https://repo.continuum.io/miniconda/Miniconda3-4.5.12-Linux-x86_64.sh && \
    bash Miniconda3-4.5.12-Linux-x86_64.sh -b -p /opt/python && \
    # CMake
    wget -nv -nc https://cmake.org/files/v3.12/cmake-3.12.0-Linux-x86_64.sh --no-check-certificate && \
    bash cmake-3.12.0-Linux-x86_64.sh --skip-license --prefix=/usr && \
    # Maven
    wget https://archive.apache.org/dist/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz && \
    tar xvf apache-maven-3.6.1-bin.tar.gz -C /opt && \
    ln -s /opt/apache-maven-3.6.1/ /opt/maven

# Set the required environment variables
ENV PATH=/opt/python/bin:/opt/maven/bin:$PATH \
    CC=/opt/rh/devtoolset-7/root/usr/bin/gcc \
    CXX=/opt/rh/devtoolset-7/root/usr/bin/c++ \
    CPP=/opt/rh/devtoolset-7/root/usr/bin/cpp


# Install Python packages
RUN \
    pip install numpy pytest scipy scikit-learn wheel kubernetes urllib3==1.22 awscli

WORKDIR /xgboost
