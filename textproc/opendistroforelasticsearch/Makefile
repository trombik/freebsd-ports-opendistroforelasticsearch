# Created by: Tom Judge <tj@FreeBSD.org>
# $FreeBSD$

PORTNAME=	opendistroforelasticsearch
PORTVERSION=	1.13.2
CATEGORIES=	textproc java devel
MASTER_SITES=	https://d3g5vo6xdbdb9a.cloudfront.net/tarball/opendistro-elasticsearch/

MAINTAINER=	y@trombik.org
COMMENT=	Apache 2.0-licensed full-text search engine for Java

LICENSE=	APACHE20

BUILD_DEPENDS=	jna>0:devel/jna
RUN_DEPENDS=	bash>0:shells/bash \
		jna>0:devel/jna

USES=		cpe shebangfix
CONFLICTS=	elasticsearch-[0-9]* elasticsearch2-[0-9]* elasticsearch5-[0-9]* elasticsearch6-[0-9]*
DISTNAME=	${PORTNAME}-${DISTVERSIONPREFIX}${DISTVERSION}-linux-x64${DISTVERSIONSUFFIX}
WRKSRC=	${WRKDIR}/${PORTNAME}-${DISTVERSIONPREFIX}${DISTVERSION}

NO_ARCH=	yes
USE_JAVA=	yes
NO_BUILD=	yes
JAVA_VERSION=	11+
USE_RC_SUBR=	elasticsearch
SHEBANG_FILES=	bin/elasticsearch \
		bin/elasticsearch-cli \
		bin/elasticsearch-env \
		bin/elasticsearch-keystore \
		bin/elasticsearch-node \
		bin/elasticsearch-plugin \
		bin/elasticsearch-shard \
		opendistro-tar-install.sh

OPTIONS_DEFINE=	DOCS

.include <bsd.port.options.mk>

CONFIG_FILES=	elasticsearch.yml log4j2.properties jvm.options
BINS=	\
		elasticsearch \
		elasticsearch-cli \
		elasticsearch-env \
		elasticsearch-env-from-file \
		elasticsearch-keystore \
		elasticsearch-node \
		elasticsearch-plugin \
		elasticsearch-shard \
		performance-analyzer-agent-cli


PORTDOCS=	LICENSE.txt \
		NOTICE.txt

SIGAR_ARCH=	${ARCH:S|i386|x86|}
SEARCHUSER?=	elasticsearch
SEARCHGROUP?=	${SEARCHUSER}
USERS=		${SEARCHUSER}
GROUPS=		${SEARCHGROUP}
ETCDIR=		${PREFIX}/etc/elasticsearch

SUB_LIST=	ETCDIR=${ETCDIR} JAVA=${JAVA} JAVA_HOME=${JAVA_HOME} INSTDIR=${PREFIX}/lib/elasticsearch
SUB_FILES=	pkg-message

post-patch:
	${REINPLACE_CMD} -e "s|%%PREFIX%%|${PREFIX}|g" ${WRKSRC}/bin/elasticsearch
	${REINPLACE_CMD} -e "s|/bin/bash|${PREFIX}/bin/bash|" ${WRKSRC}/bin/elasticsearch-*
	${RM} ${WRKSRC}/lib/jna-*.jar

do-install:
	${MKDIR} ${STAGEDIR}${ETCDIR}
.for f in ${CONFIG_FILES}
	${INSTALL} ${WRKSRC}/config/${f} ${STAGEDIR}${ETCDIR}/${f}.sample
.endfor
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/elasticsearch/bin
.for f in ${BINS}
	${INSTALL_SCRIPT} ${WRKSRC}/bin/${f} ${STAGEDIR}${PREFIX}/lib/elasticsearch/bin
.endfor
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/elasticsearch/lib
	(cd ${WRKSRC}/lib && ${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/lib/elasticsearch/lib/ "-name *\.jar")

	${MKDIR} ${STAGEDIR}${PREFIX}/lib/elasticsearch/modules
	(cd ${WRKSRC}/modules && ${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/lib/elasticsearch/modules/)
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/elasticsearch/plugins
	(cd ${WRKSRC}/plugins && ${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/lib/elasticsearch/plugins/)
	${MKDIR} ${STAGEDIR}${PREFIX}/libexec/elasticsearch
	${INSTALL} -lrs ${STAGEDIR}${ETCDIR} ${STAGEDIR}${PREFIX}/lib/elasticsearch/config
	${LN} -s ${JAVASHAREDIR}/classes/jna.jar ${STAGEDIR}${PREFIX}/lib/elasticsearch/lib/jna.jar

	${INSTALL_SCRIPT} ${WRKSRC}/opendistro-tar-install.sh ${STAGEDIR}${PREFIX}/lib/elasticsearch

	# remove linux binaries
	${RM} ${STAGEDIR}${PREFIX}/lib/elasticsearch/plugins/opendistro-knn/knn-lib/libKNNIndexV2_0_11.so

do-install-DOCS-on:
	${MKDIR} ${STAGEDIR}${DOCSDIR}
.for f in ${PORTDOCS}
	${INSTALL_DATA} ${WRKSRC}/${f} ${STAGEDIR}${DOCSDIR}
.endfor

.include <bsd.port.mk>
