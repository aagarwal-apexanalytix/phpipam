# Overlay the OceanBase-compat LockForUpdate patch onto the official phpIPAM image.
# BASE is phpipam/phpipam-www:<tag> or phpipam/phpipam-cron:<tag>. Build context = repo root.
ARG BASE
FROM ${BASE}
# App lives at /phpipam in the official phpipam-www / -cron images (verified live).
COPY functions/classes/class.LockForUpdate.php /phpipam/functions/classes/class.LockForUpdate.php
