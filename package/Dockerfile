ARG BCI_VERSION=15.7
FROM registry.suse.com/bci/bci-micro:${BCI_VERSION} AS micro
FROM registry.suse.com/bci/bci-base:${BCI_VERSION} AS builder

COPY --from=micro / /chroot/
RUN zypper refresh && zypper --installroot /chroot -n in --no-recommends \
    ca-certificates curl grep && \
    zypper --installroot /chroot clean -a && \
    rm -rf /chroot/var/log/

RUN cd /chroot/usr/bin/ && rm -rf basename chcon chgrp chmod chown chroot cksum dd df dircolors dirname du install install-info join locale localedef mkdir mkfifo mknod mktemp paste pathchk readlink realpath sync smidiff smidump smilink smiquery smistrip smixlate tee tiemout tload top truncate unlink watch

FROM micro

ARG COMMIT
ARG VERSION

COPY --from=builder /chroot/ /
WORKDIR /

LABEL "name"="updater" \
      "vendor"="SUSE Security" \
      "neuvector.image"="neuvector/updater" \
      "neuvector.role"="updater" \
      "neuvector.rev"="${COMMIT}" \
      "io.artifacthub.package.logo-url"=https://avatars2.githubusercontent.com/u/19367275 \
      "io.artifacthub.package.readme-url"="https://raw.githubusercontent.com/neuvector/updater/${VERSION}/README.md" \
      "org.opencontainers.image.description"="SUSE Security Updater" \
      "org.opencontainers.image.title"="SUSE Security Updater" \
      "org.opencontainers.image.source"="https://github.com/neuvector/updater/" \
      "org.opencontainers.image.version"="${VERSION}" \
      "org.opensuse.reference"="neuvector/updater:${VERSION}"
      

ENTRYPOINT ["sleep", "30"]
