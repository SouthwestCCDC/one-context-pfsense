SHELL:=/bin/bash
.PHONY = help
.DEFAULT_GOAL = pfsense.iso

MIRROR="atxfiles.netgate.com"
VERSION="2.5.0"
PFSENSE_ISO="pfSense-CE-${VERSION}-RELEASE-amd64.iso"
PFSENSE_URL="https://${MIRROR}/mirror/downloads/${PFSENSE_ISO}.gz"

pfsense.iso.gz:
	wget ${PFSENSE_URL}
	wget ${PFSENSE_URL}.sha256 -O pfsense.iso.gz.sha256
	sha256sum --check pfsense.iso.gz.sha256
	rm pfsense.iso.gz.sha256
	mv pfSense-CE-${VERSION}-RELEASE-amd64.iso.gz pfsense.iso.gz

pfsense.iso: pfsense.iso.gz
	gzip -d pfsense.iso.gz

http:
	mkdir http
	cp ../one-context* http/

pfsense.qcow2: pfsense.iso http

clean:
	@-rm -rf packer_cache http pfsense.iso* output-qemu ${PFSENSE_ISO}* 2>/dev/null || true

help:
	@echo "---  Prepare a pfSense image for SWCCDC OpenNebula  ---"
	@echo "--- --- --- --- --- --- --- --- --- --- --- --- --- ---"
