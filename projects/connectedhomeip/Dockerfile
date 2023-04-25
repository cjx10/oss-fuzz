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

FROM gcr.io/oss-fuzz-base/base-builder

# Install prerequisite packages
# See connectedhomeip/docs/guides/BUILDING.md#prerequisites
RUN apt-get update && \
    apt-get install -y pkg-config libssl-dev libdbus-1-dev libglib2.0-dev \
		libavahi-client-dev ninja-build python3-venv python3-dev python3-pip \
		unzip libgirepository1.0-dev libcairo2-dev libreadline-dev

# Install Rust for building `cryptography` python package when bootstraping pigweed
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN rustup install nightly
RUN rustup default nightly

RUN git clone --recurse-submodules https://github.com/project-chip/connectedhomeip.git connectedhomeip

# Bootstrap pigweed environment
SHELL ["/bin/bash", "-c"]
RUN cd $SRC/connectedhomeip && . scripts/bootstrap.sh

COPY build.sh $SRC/