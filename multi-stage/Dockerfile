# Copyright (c) 2021 Foundries.io
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# For local x86_64 testing
# docker build --tag stage:1.0 .
# docker run -it --rm stage:1.0

FROM debian:bullseye-slim AS build
RUN  echo "-------Build Stage--------------"

LABEL maintainer="Raul Muñoz <raul@foundries.io>"

#Install packages for the build stage
RUN apt-get update && \
    apt-get install -y --no-install-recommends build-essential && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /build/

COPY helloworld.c /build/

WORKDIR /build/

RUN gcc helloworld.c -o helloworld

RUN  echo "-------Final Stage--------------"
FROM debian:bullseye-slim AS final-stage

#Install packages for the final stage
RUN apt-get update && \
    apt-get install -y --no-install-recommends vim && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /build/

COPY --from=build /build/helloworld /build/

WORKDIR /build/

COPY start.sh /build/

ENTRYPOINT ["/build/start.sh"]