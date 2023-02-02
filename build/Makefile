SHELL:=/bin/bash
.PHONY = help
.DEFAULT_GOAL = pfsense.iso

ifdef ROLE
ANSIBLE_DIR=${ANSIBLE_BASE}/roles/${ROLE}
ROLE_DEFINED=1
else
ANSIBLE_DIR=${ANSIBLE_BASE}
ROLE_DEFINED=0
endif

ifdef PLAYBOOK
PLAYBOOK_DEFINED=1
else
PLAYBOOK_DEFINED=0
endif

MIRROR="atxfiles.netgate.com"
VERSION="2.6.0"
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

clean:
	@-rm pfsense.iso* ${PFSENSE_ISO}* 2>/dev/null || true

help:
	@echo "--- Prepare a pyenv + ansible swccdc-ops environment ---"
	@echo " This is intended to run on a system that has been"
	@echo " prepared using the blackteam_swccdc_dev ansible role,"
	@echo " by a user that was passed to that role as the"
	@echo " deployment user. Available targets:"
	@echo "  - pydep:"
	@echo "      Updates pip dependencies"
	@echo "  - ansibledep:"
	@echo "      Updates ansible dependencies"
	@echo "  - lint [ROLE=rolepath]:"
	@echo "      Runs molecule lint on the role in the environment"
	@echo "      variable ROLE, which is expected to live in"
	@echo "      ansible/roles/. Without ROLE specified, lints the"
	@echo "      entire ansible tree."
	@echo "  - test-ansible [ROLE=rolepath]:"
	@echo "      Runs molecule test on the role in the environment"
	@echo "      variable ROLE, which is expected to live in"
	@echo "      ansible/roles/. Without ROLE specified, tests the"
	@echo "      entire ansible tree."
	@echo "  - check-ansible {PLAYBOOK=playbookname} [HOSTS=types]:"
	@echo "      Runs ansible-playbook --check against a playbook"
	@echo "      and, optionally, limits that check by host type."
	@echo "      The PLAYBOOK should OMIT the .yml extension,"
	@echo "      and the HOSTS is a string that will be passed to '-l'"
	@echo "  - apply-ansible {PLAYBOOK=playbookname} [HOSTS=types]:"
	@echo "      Actually executes ansible-playbook against a playbook"
	@echo "      and, optionally, limits that check by host type."
	@echo "      The PLAYBOOK should OMIT the .yml extension,"
	@echo "      and the HOSTS is a string that will be passed to '-l'"
	@echo "  - newrole [ROLE=rolepath]:"
	@echo "      Runs molecule init role on the destination environment"
	@echo "      variable ROLE, which will be placed into the path of"
	@echo "      ansible/roles/."
	@echo "  - nwtest-ips:"
	@echo "      Create the list of IP addresses to attempt to ping to"
	@echo "      perform a network flow regression test of the game."
	@echo "  - nwtest:"
	@echo "      Perform a network flow regression test of the game,"
	@echo "      raising an error and printing any failed IP addresses."
	@echo "  - clean:"
	@echo "      Cleans out state created by make runs, and also"
	@echo "      performs a 'molecule destroy'."
	@echo "--- --- --- --- --- --- --- --- --- --- --- --- --- ---"
