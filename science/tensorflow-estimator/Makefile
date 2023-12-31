# $FreeBSD$
PORTNAME=	tensorflow_estimator
PORTVERSION=	1.14.0
DISTVERSIONPREFIX=	v
CATEGORIES=	science python

MAINTAINER=	amzo1337@gmail.com
COMMENT=	"TensorFlow Estimator is a high-level TensorFlow API that greatly simplifies machine learning programming."

LICENSE=	APACHE20

BUILD_DEPENDS=	bash:shells/bash \
		bazel:devel/bazel \
		${PYTHON_PKGNAMEPREFIX}tensorflow>=2.1.0:science/py-tensorflow@${PY_FLAVOR} \

USES=		python shebangfix
SHEBANG_GLOB=	*.py

USE_GITHUB=	yes
GH_PROJECT=	estimator
GH_ACCOUNT=	tensorflow

BAZEL_BOOT=     --output_user_root=${WRKDIR}/bazel_out

post-patch:
	@${REINPLACE_CMD} "s#%%PYTHON%%#${PYTHON_CMD}#" \
		${WRKSRC}/tensorflow_estimator/tools/pip_package/build_pip_package.sh

do-build:
	(cd ${WRKSRC} && \
		bazel ${BAZEL_BOOT} build -s \
			//tensorflow_estimator/tools/pip_package:build_pip_package)

	(cd ${WRKSRC} && ${SETENV} TMPDIR=${WRKDIR} && \
		bazel-bin/tensorflow_estimator/tools/pip_package/build_pip_package \
			${WRKDIR}/whl)

do-install:
	@${MKDIR} ${STAGEDIR}/${PYTHON_SITELIBDIR}
	@${MKDIR} ${WRKDIR}/tmp
	@${UNZIP_NATIVE_CMD} -d ${WRKDIR}/tmp ${WRKDIR}/whl/${PORTNAME}-${PORTVERSION}-*.whl

	cd ${WRKDIR}/tmp && ${COPYTREE_SHARE} ${PORTNAME} \
		${STAGEDIR}${PYTHON_SITELIBDIR}

	cd ${WRKDIR}/tmp && ${COPYTREE_SHARE} ${PORTNAME}-${PORTVERSION}.dist-info \
		${STAGEDIR}${PYTHON_SITELIBDIR}

.include <bsd.port.mk>
