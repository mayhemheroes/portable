# Copyright 2018 Google Inc.
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
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder
RUN apt-get -o Acquire::Retries=5 update && apt-get -o Acquire::Retries=5 install -y make cmake wget
# Download LibreSSL release tarball (has pre-generated arch headers, no update.sh needed)
RUN for i in 1 2 3; do \
      wget -q https://ftp.openbsd.org/pub/OpenBSD/LibreSSL/libressl-4.3.1.tar.gz && break || sleep 5; \
    done && \
    tar -xzf libressl-4.3.1.tar.gz && \
    mv libressl-4.3.1 libressl && \
    rm libressl-4.3.1.tar.gz
RUN for i in 1 2 3; do git clone --depth 1 https://github.com/libressl-portable/fuzz.git libressl.fuzzers && break || sleep 5; done
RUN for i in 1 2 3; do git clone --depth 1 https://github.com/guidovranken/cryptofuzz && break || sleep 5; done
RUN for i in 1 2 3; do git clone --depth 1 https://github.com/guidovranken/cryptofuzz-corpora && break || sleep 5; done
RUN for i in 1 2 3; do wget -q https://archives.boost.io/release/1.74.0/source/boost_1_74_0.tar.bz2 && break || sleep 5; done
COPY mayhem/build.sh mayhem/*.options $SRC/

ENV FUZZING_LANGUAGE=c++ SANITIZER=address
RUN compile
