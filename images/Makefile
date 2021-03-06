# Copyright 2016 The Rook Authors. All rights reserved.
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

include image.mk

PLATFORMS ?= linux_amd64 linux_arm64

CEPH_BASE_IMAGE ?= ceph/daemon-base:v3.0.5-stable-3.0-luminous-centos-7-$(PLATFORM_ARCH)
export CEPH_BASE_IMAGE

# tini's version
export TINI_VERSION = v0.16.1

# ====================================================================================
# Image Targets

cross: cross.linux_amd64
cross.%:
	@$(MAKE) -C cross PLATFORM=$*

ceph.%:
	@$(MAKE) -C ceph PLATFORM=$*

ceph-toolbox.%: ceph.%
	@$(MAKE) -C ceph-toolbox PLATFORM=$*

cockroachdb.%:
	@$(MAKE) -C cockroachdb PLATFORM=$*

do.build.images.%: ceph.% ceph-toolbox.% cockroachdb.% ;
do.build: do.build.images.$(PLATFORM) ;
build.all: $(foreach p,$(PLATFORMS), do.build.images.$(p)) ;

# ====================================================================================
# Help

.PHONY: help
help:
	@echo 'Usage: make <OPTIONS> ... <TARGETS>'
	@echo ''
	@echo 'Targets:'
	@echo '    build        Build images for the host platform.'
	@echo '    build.all    Build images for all platforms.'
	@echo '    clean        Remove all images created from the'
	@echo '                 current build.'
	@echo '    prune        Prunes orphaned and cached images at the.'
	@echo '                 host level.'
	@echo ''
	@echo 'Advanced Targets:'
	@echo '    cross        Build images used for cross building.'
	@echo '    clean.all    Remove all images including cached.'
	@echo '                 images at the host level.'
	@echo '    prune.all    Prune everything ignoring policy.'
	@echo ''
	@echo 'Options:'
	@echo '    PLATFORM     The platform to build.'
	@echo '    PULL         Whether to pull base images. Default is 1.'
	@echo '    V            Set to 1 enable verbose build. Default is 0.'
	@echo ''
	@echo 'Advanced Options:'
	@echo '    PRUNE_HOURS  The number of hours from when an image is'
	@echo '                 last used for it to be considered a target for'
	@echo '                 pruning. Default is 48 hrs.'
	@echo '    PRUNE_KEEP   The minimum number of cached images to keep.'
	@echo '                 Default is 24 images.'
