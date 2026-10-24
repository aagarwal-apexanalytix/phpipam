# Overlay the OceanBase-compat LockForUpdate patch onto the official phpIPAM image.
# BASE is phpipam/phpipam-www:<tag> or phpipam/phpipam-cron:<tag>. Build context = repo root.
ARG BASE
FROM ${BASE}
# Upstream vX.Y.Z images are built once and never rebuilt, so their Alpine packages (PHP included)
# freeze at release time. Upgrade every apk package from the base's own Alpine branch so a rebuild
# picks up current PHP security releases, and refuse the build if PHP is below MIN_PHP_VERSION
# (8.3.36 fixes CVE-2026-109618 / CVE-2026-109619, unauthenticated RCE).
# php83-zip: the v1.8.3 base dropped the zip extension that the v1.8.1-ob images carried; it is
# re-added so `php -m` matches the image this one replaces.
ARG MIN_PHP_VERSION=8.3.36
RUN apk upgrade --no-cache \
 && apk add --no-cache php83-zip \
 && php -r 'exit(version_compare(PHP_VERSION, getenv("MIN_PHP_VERSION"), ">=") ? 0 : 1);' \
 || { echo "PHP $(php -r 'echo PHP_VERSION;') is below ${MIN_PHP_VERSION}" >&2; exit 1; }
# App lives at /phpipam in the official phpipam-www / -cron images (verified live).
COPY functions/classes/class.LockForUpdate.php /phpipam/functions/classes/class.LockForUpdate.php
# Upstream master 7b38d4c3 (#4663, cherry-picked onto v1.8.3 on this branch): the v1.8.2 cache-key
# rework makes API multi-row lookups (e.g. addresses/search_hostname) return decimal IPs for every
# row after the first. Drop this COPY once the base tag carries the fix.
COPY functions/classes/class.Common.php /phpipam/functions/classes/class.Common.php
