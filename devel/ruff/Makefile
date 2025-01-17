# ring-v0.16.20 does not support those archs
NOT_FOR_ARCHS =	powerpc64 riscv64 sparc64

COMMENT =	extremely fast Python linter

GH_ACCOUNT =	charliermarsh
GH_PROJECT =	ruff
GH_TAGNAME =	v0.0.254

CATEGORIES =	devel

MAINTAINER =	Laurent Cheylus <foxy@free.fr>

# MIT
PERMIT_PACKAGE =	Yes

# Need some Rust extra packages
LIBCST_COMMIT =		80e4c13
RUSTPYTHON_COMMIT =	1871a16
UNICODE_NAMES2_COMMIT =	4ce16aa

MASTER_SITES0 =	https://github.com/charliermarsh/LibCST/archive/
MASTER_SITES1 =	https://github.com/RustPython/RustPython/archive/
MASTER_SITES2 =	https://github.com/youknowone/unicode_names2/archive/

DISTFILES +=	${GH_DISTFILE} \
		LibCST-{}${LIBCST_COMMIT}.tar.gz:0 \
		RustPython-{}${RUSTPYTHON_COMMIT}.tar.gz:1 \
		unicode_names2-{}${UNICODE_NAMES2_COMMIT}.tar.gz:2

WANTLIB +=	${MODCARGO_WANTLIB} m

MODULES =	devel/cargo

BUILD_DEPENDS =	security/rust-ring

CONFIGURE_STYLE =	cargo
SEPARATE_BUILD =	Yes

SUBST_VARS +=	MODCARGO_VENDOR_DIR

# Move sources for Rust extra packages
post-extract:
	mkdir -p ${MODCARGO_VENDOR_DIR}
	mv ${WRKDIR}/LibCST-${LIBCST_COMMIT}* ${MODCARGO_VENDOR_DIR}/libcst-0.1.0
	mv ${WRKDIR}/RustPython-${RUSTPYTHON_COMMIT}* \
		${MODCARGO_VENDOR_DIR}/rustpython-0.2.0
	mv ${WRKDIR}/unicode_names2-${UNICODE_NAMES2_COMMIT}* \
		${MODCARGO_VENDOR_DIR}/unicode_names2-0.6.0

post-patch:
	${SUBST_CMD} ${WRKSRC}/Cargo.toml
	${SUBST_CMD} ${MODCARGO_VENDOR_DIR}/rustpython-0.2.0/Cargo.toml

# Generate metadata for Rust extract packages (see devel/cargo/cargo.port.mk)
pre-build:
	${LOCALBASE}/bin/cargo-generate-vendor \
		${FULLDISTDIR}/LibCST-${LIBCST_COMMIT}.tar.gz \
		${MODCARGO_VENDOR_DIR}/libcst-0.1.0/native
	${LOCALBASE}/bin/cargo-generate-vendor \
		${FULLDISTDIR}/RustPython-${RUSTPYTHON_COMMIT}.tar.gz \
		${MODCARGO_VENDOR_DIR}/rustpython-0.2.0
	${LOCALBASE}/bin/cargo-generate-vendor \
		${FULLDISTDIR}/unicode_names2-${UNICODE_NAMES2_COMMIT}.tar.gz \
		${MODCARGO_VENDOR_DIR}/unicode_names2-0.6.0

do-install:
	${INSTALL_PROGRAM} ${MODCARGO_TARGET_DIR}/release/ruff ${PREFIX}/bin/

.include "crates.inc"

.include <bsd.port.mk>
