# Copyright 2023 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder-jvm

RUN curl -L https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip -o maven.zip && \
    unzip maven.zip -d $SRC/maven && \
    rm -rf maven.zip

ENV MVN $SRC/maven/apache-maven-3.6.3/bin/mvn

RUN git clone --depth 1 https://github.com/google/fuzzing && \
    cp fuzzing/dictionaries/yaml.dict $SRC/YamlReaderFuzzer.dict && \
    cp fuzzing/dictionaries/yaml.dict $SRC/YamlWriterFuzzer.dict && \
    rm -rf fuzzing

# if not set python infra helper cannot be used for local testing

COPY project-parent $SRC/project-parent/

RUN rm -rf $SRC/project-parent/yamlbeans
RUN apt update && apt install -y openjdk-8-jdk
RUN git clone --depth 1 https://github.com/EsotericSoftware/yamlbeans/ $SRC/project-parent/yamlbeans

COPY build.sh $SRC/
WORKDIR $SRC/