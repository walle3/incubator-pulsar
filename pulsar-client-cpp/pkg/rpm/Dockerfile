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

# Build pulsar client library in Centos with tools to build static RPM

FROM centos:7

RUN yum update && \
    yum install -y gcc-c++ make cmake git rpm-build \
                python-devel openssl-devel openssl-static

# Download and compile boost
RUN curl -O -L https://dl.bintray.com/boostorg/release/1.64.0/source/boost_1_64_0.tar.gz && \
    tar xvfz boost_1_64_0.tar.gz && \
    cd /boost_1_64_0 && \
    ./bootstrap.sh --with-libraries=program_options,filesystem,regex,thread,system,python && \
    ./b2 address-model=64 cxxflags=-fPIC link=static threading=multi variant=release install && \
    rm -rf /boost_1_64_0.tar.gz /boost_1_64_0

RUN curl -O -L https://github.com/Kitware/CMake/archive/v3.8.2.tar.gz && \
    tar xvfz v3.8.2.tar.gz && \
    cd CMake-3.8.2 && \
    ./configure && \
    make && make install && \
    rm -rf /v3.8.2.tar.gz /CMake-3.8.2

# Compile JSON CPP
RUN curl -O -L  https://github.com/open-source-parsers/jsoncpp/archive/1.8.0.tar.gz && \
    tar xvfz 1.8.0.tar.gz && \
    cd jsoncpp-1.8.0 && \
    cmake . -DCMAKE_POSITION_INDEPENDENT_CODE=ON && \
    make && make install && \
    rm -rf /1.8.0.tar.gz /jsoncpp-1.8.0

# Download and copile protoubf
RUN curl -O -L  https://github.com/google/protobuf/releases/download/v3.3.0/protobuf-cpp-3.3.0.tar.gz && \
    tar xvfz protobuf-cpp-3.3.0.tar.gz && \
    cd protobuf-3.3.0/ && \
    CXXFLAGS=-fPIC ./configure && \
    make && make install && ldconfig && \
    rm -rf /protobuf-cpp-3.3.0.tar.gz /protobuf-3.3.0

# ZLib
RUN curl -O -L https://github.com/madler/zlib/archive/v1.2.11.tar.gz && \
    tar xvfz v1.2.11.tar.gz && \
    cd zlib-1.2.11 && \
    CFLAGS="-fPIC -O3" ./configure && \
    make && make install && \
    rm -rf /zlib-1.2.11.tar.gz /zlib-1.2.11

# LibCurl
RUN curl -O -L  https://github.com/curl/curl/releases/download/curl-7_54_0/curl-7.54.0.tar.gz && \
    tar xvfz curl-7.54.0.tar.gz && \
    cd curl-7.54.0 && \
    CFLAGS=-fPIC ./configure && \
    make && make install && \
    rm -rf /curl-7.54.0.tar.gz /curl-7.54.0

ADD build-rpm.sh /

RUN yum install -y createrepo
