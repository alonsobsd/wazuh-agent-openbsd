COMMENT=		Security tool to monitor and check logs and intrusions

V=			4.9.2
GH_ACCOUNT=		wazuh
GH_PROJECT=		wazuh
GH_TAGNAME=		v${V}
PKGNAME=		wazuh-agent-${V}

CATEGORIES=		security

HOMEPAGE=		http://www.wazuh.com

MAINTAINER=		acardenas@bsd-peru.org

SITES.alt=		https://packages.wazuh.com/deps/30/libraries/sources/

# GPLv2
PERMIT_PACKAGE=		Yes

COMPILER=		base-clang ports-gcc base-gcc
BUILD_DEPENDS=		devel/cmake
LIB_DEPENDS=		devel/libinotify \
			www/nghttp2 \
			databases/sqlite3

WANTLIB = 		${COMPILER_LIBCXX} c m
WANTLIB +=		lib/inotify/inotify>=3 nghttp2 perl readline sqlite3

DISTFILES.alt=		cJSON.tar.gz \
                        curl.tar.gz \
                        libdb.tar.gz \
                        libffi.tar.gz \
                        libyaml.tar.gz \
                        openssl.tar.gz \
                        procps.tar.gz \
                        sqlite.tar.gz \
                        zlib.tar.gz \
                        audit-userspace.tar.gz \
                        msgpack.tar.gz \
                        bzip2.tar.gz \
                        nlohmann.tar.gz \
                        googletest.tar.gz \
                        libpcre2.tar.gz \
                        libplist.tar.gz \
                        libarchive.tar.gz \
                        popt.tar.gz


USE_GMAKE=	Yes

ALL_TARGET=	TARGET=agent

MAKE_ENV+=	USE_INOTIFY=yes WAZUH_USER=_wazuh WAZUH_GROUP=_wazuh

NO_TEST=	Yes
PREFIX=		${VARBASE}
INSTDIR=	${PREFIX}/ossec
TINSTDIR =	${TRUEPREFIX}/ossec
SUBST_VARS =	INSTDIR TINSTDIR

WRKBUILD=	${WRKSRC}/src

WZBIN_FILES=	agent-auth manage_agents wazuh-agentd wazuh-execd wazuh-logcollector \
		wazuh-modulesd

WZARBIN_FILES=	default-firewall-drop pf npf ipfw firewalld-drop disable-account \
		host-deny ip-customblock restart-wazuh route-null kaspersky wazuh-slack

WAZUHMOD750=	/ /logs/wazuh /bin /lib /queue /queue/diff /queue/logcollector \
		/queue/syscollector /queue/syscollector/db  /ruleset /ruleset/sca /wodles \
		/active-response /active-response/bin /agentless /var /backup \
		/wodles/aws /wodles/azure /wodles/docker /wodles/gcloud \
		/wodles/gcloud/buckets /wodles/gcloud/pubsub

WAZUHMOD770=	/etc/shared/default /logs /queue/alerts /queue/fim \
		/queue/fim/db /queue/rids /queue/sockets /etc /etc/shared \
		/var/run /var/upgrade /var/selinux /var/wodles /var/incoming

post-extract:
.for DIRE in ${DISTFILES.alt}
	@mv ${WRKDIR}/${DIRE:S/.tar.gz//} ${WRKBUILD}/external/
.endfor

do-install:
	mkdir -p -m 0750 ${INSTDIR}/bin
	mkdir -p -m 0750 ${INSTDIR}/lib
	mkdir -p -m 0750 ${INSTDIR}/tmp

.for DIRE in ${WAZUHMOD750}
	mkdir -p -m 0750 ${INSTDIR}${DIRE}
.endfor

.for DIRE in ${WAZUHMOD770}
	mkdir -p -m 0770 ${INSTDIR}${DIRE}
.endfor

.for FILE in ${WZBIN_FILES}
	${INSTALL_PROGRAM} ${WRKSRC}/src/${FILE} ${INSTDIR}/bin
.endfor
	${INSTALL_PROGRAM} ${WRKSRC}/src/syscheckd/build/bin/wazuh-syscheckd ${INSTDIR}/bin
	${INSTALL_SCRIPT} ${WRKSRC}/src/init/wazuh-client.sh ${INSTDIR}/bin/wazuh-control

	touch ${INSTDIR}/etc/localtime

	${INSTALL_DATA} ${WRKSRC}/etc/internal_options.conf ${INSTDIR}/etc
	${INSTALL_DATA} ${WRKSRC}/src/wazuh_modules/syscollector/norm_config.json ${INSTDIR}/queue/syscollector
	${INSTALL_DATA} ${WRKSRC}/etc/local_internal_options.conf ${INSTDIR}/etc/local_internal_options.conf.sample

	${INSTALL_DATA} /dev/null ${INSTDIR}/etc/client.keys.sample
	${INSTALL_DATA} ${WRKSRC}/etc/wpk_root.pem ${INSTDIR}/etc/

	${INSTALL_DATA} ${WRKSRC}/etc/ossec-agent.conf ${INSTDIR}/etc/ossec.conf.sample

	${INSTALL_SCRIPT} ${WRKSRC}/src/agentlessd/scripts/* ${INSTDIR}/agentless/

.for FILE in ${WZARBIN_FILES}
	${INSTALL_PROGRAM} ${WRKSRC}/src/${FILE} ${INSTDIR}/active-response/bin
.endfor
	${INSTALL_PROGRAM} ${WRKSRC}/src/wazuh-slack ${INSTDIR}/active-response/bin

	${INSTALL_SCRIPT} ${WRKSRC}/src/active-response/*.sh ${INSTDIR}/active-response/bin
	${INSTALL_SCRIPT} ${WRKSRC}/src/active-response/*.py ${INSTDIR}/active-response/bin

	${INSTALL_PROGRAM} ${WRKSRC}/src/default-firewall-drop ${INSTDIR}/active-response/bin/firewall-drop
	${INSTALL_DATA} ${WRKSRC}/ruleset/rootcheck/db/*.txt ${INSTDIR}/etc/shared

	${INSTALL_DATA} ${WRKSRC}/ruleset/rootcheck/db/*.txt ${INSTDIR}/etc/shared/default

	${INSTALL_SCRIPT} ${WRKSRC}/wodles/__init__.py ${INSTDIR}/wodles/__init__.py
	${INSTALL_SCRIPT} ${WRKSRC}/wodles/utils.py ${INSTDIR}/wodles/utils.py

	${INSTALL_SCRIPT} ${WRKSRC}/wodles/aws/aws_s3.py ${INSTDIR}/wodles/aws/aws-s3.py
	${INSTALL_SCRIPT} ${WRKSRC}/framework/wrappers/generic_wrapper.sh ${INSTDIR}/wodles/aws/aws-s3

	${INSTALL_SCRIPT} ${WRKSRC}/wodles/gcloud/exceptions.py ${INSTDIR}/wodles/gcloud/exceptions.py
	${INSTALL_SCRIPT} ${WRKSRC}/wodles/gcloud/gcloud.py ${INSTDIR}/wodles/gcloud/gcloud.py
	${INSTALL_SCRIPT} ${WRKSRC}/wodles/gcloud/integration.py ${INSTDIR}/wodles/gcloud/integration.py
	${INSTALL_SCRIPT} ${WRKSRC}/wodles/gcloud/tools.py ${INSTDIR}/wodles/gcloud/tools.py
	${INSTALL_SCRIPT} ${WRKSRC}/wodles/gcloud/buckets/bucket.py ${INSTDIR}/wodles/gcloud/buckets/bucket.py
	${INSTALL_SCRIPT} ${WRKSRC}/wodles/gcloud/buckets/access_logs.py ${INSTDIR}/wodles/gcloud/buckets/access_logs.py
	${INSTALL_SCRIPT} ${WRKSRC}/wodles/gcloud/pubsub/subscriber.py ${INSTDIR}/wodles/gcloud/pubsub/subscriber.py
	${INSTALL_SCRIPT} ${WRKSRC}/framework/wrappers/generic_wrapper.sh ${INSTDIR}/wodles/gcloud/gcloud

	${INSTALL_SCRIPT} ${WRKSRC}/wodles/docker-listener/DockerListener.py ${INSTDIR}/wodles/docker/DockerListener.py
	${INSTALL_SCRIPT} ${WRKSRC}/framework/wrappers/generic_wrapper.sh ${INSTDIR}/wodles/docker/DockerListener

	${INSTALL_SCRIPT} ${WRKSRC}/wodles/azure/azure-logs.py ${INSTDIR}/wodles/azure/azure-logs.py
	${INSTALL_SCRIPT} ${WRKSRC}/framework/wrappers/generic_wrapper.sh ${INSTDIR}/wodles/azure/azure-logs

	${INSTALL_DATA} ${WRKSRC}/ruleset/sca/generic/sca_unix_audit.yml ${INSTDIR}/ruleset/sca/

	${INSTALL_DATA} ${WRKSRC}/src/libwazuhext.so ${INSTDIR}/lib
	${INSTALL_DATA} ${WRKSRC}/src/libwazuhshared.so ${INSTDIR}/lib
	${INSTALL_DATA} ${WRKSRC}/src/shared_modules/dbsync/build/lib/libdbsync.so ${INSTDIR}/lib
	${INSTALL_DATA} ${WRKSRC}/src/shared_modules/rsync/build/lib/librsync.so ${INSTDIR}/lib
	${INSTALL_DATA} ${WRKSRC}/src/syscheckd/build/lib/libfimdb.so ${INSTDIR}/lib
	${INSTALL_DATA} ${WRKSRC}/src/wazuh_modules/syscollector/build/lib/libsyscollector.so ${INSTDIR}/lib
	${INSTALL_DATA} ${WRKSRC}/src/data_provider/build/lib/libsysinfo.so ${INSTDIR}/lib

	mkdir -p -m 0750 ${INSTDIR}/packages_files/agent_installation_scripts/etc/templates
	mkdir -p -m 0750 ${INSTDIR}/packages_files/agent_installation_scripts/src

	cd ${WRKSRC}/etc/templates && mv config \
		${INSTDIR}/packages_files/agent_installation_scripts/etc/templates/
	cd ${WRKSRC}/ruleset && mv sca \
		${INSTDIR}/packages_files/agent_installation_scripts/
	cd ${WRKSRC}/src && mv init \
		${INSTDIR}/packages_files/agent_installation_scripts/src/

	${INSTALL_SCRIPT} ${WRKSRC}/gen_ossec.sh ${INSTDIR}/packages_files/agent_installation_scripts/
	${INSTALL_SCRIPT} ${WRKSRC}/add_localfiles.sh ${INSTDIR}/packages_files/agent_installation_scripts/

.include <bsd.port.mk>
