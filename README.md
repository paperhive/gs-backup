# gs-backup
Encrypt a file and upload it to Google Cloud storage.

## Required environment variables

 * `BACKUP_FILE`: path to a file that should be uploaded.
 * `BACKUP_BUCKET`: bucket url like gs://awesome-backups
 * `BACKUP_PGP_KEY`: path to public key, defaults to /pubkey.asc

## Volumes

 * `/scratch`: temporary storage
 * `/.config`: gcloud config

## Usage

Authenticate with service account (if the `/.config` volume is permanent this only needs to be done once):
```
docker run -i --rm \
  -v /PATH/TO/gcloud-config/:/.config \
  -v /PATH/TO/:/key.json \
  gs-backup \
  gcloud auth activate-service-account --key-file=/key.json
```

Run:
```
docker run -i --rm \
  -v /PATH/TO/gcloud-config/:/.config \
  -v /PATH/TO/paperhive-backups.pub.asc:/pubkey.asc \
  -v /PATH/TO/data:/data \
  -e BACKUP_FILE=/data/file \
  -e BACKUP_BUCKET=gs://awesome-backups \
  gs-backup
```
