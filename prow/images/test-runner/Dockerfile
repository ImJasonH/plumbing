# Copyright 2018 The Knative Authors
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

FROM gcr.io/k8s-testimages/kubekins-e2e:latest-master
LABEL maintainer "Adriano Cunha <adrcunha@google.com>"

# Install extras on top of base image

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update
RUN gcloud components update

# Docker
RUN gcloud components install docker-credential-gcr
RUN docker-credential-gcr configure-docker

# Extra tools through apt-get
RUN apt-get install -y uuid-runtime  # for uuidgen
RUN apt-get install -y rubygems  # for mdl

# Extra tools through go get
RUN go get -u github.com/google/ko/cmd/ko
RUN go get -u github.com/golang/dep/cmd/dep
RUN go get -u github.com/google/licenseclassifier
RUN go get -u github.com/jstemmer/go-junit-report
RUN go get -u github.com/raviqqe/liche
RUN go get -u github.com/golangci/golangci-lint/cmd/golangci-lint

# Extra tools through gem
RUN gem install mixlib-config -v 2.2.4  # required because ruby is 2.1
RUN gem install mdl

# Temporarily add test-infra to the image to build custom tools
RUN mkdir -p /go/src/github.com/knative/ && git clone https://github.com/knative/test-infra.git /go/src/github.com/knative/test-infra && \
    make -C /go/src/github.com/knative/test-infra/tools/githubhelper && \
    cp /go/src/github.com/knative/test-infra/tools/githubhelper/githubhelper . && \
    go install github.com/knative/test-infra/tools/dep-collector
