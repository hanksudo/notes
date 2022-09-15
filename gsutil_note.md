# gsutil tool

- [gsutil tool  |  Cloud Storage  |  Google Cloud](https://cloud.google.com/storage/docs/gsutil)

## Usage

```bash
# create bucket
gsutil mb gs://$BUCKET_NAME

# list buckets
gsutil ls
gsutil list

# list folder and files in bucket
gsutil ls gs://$BUCKET_NAME
gsutil ls -h gs://$BUCKET_NAME/20220901/

# copy file from remote
gsutil cp gs://$BUCKET_NAME/20220901/myfile.gz .

# copy files from remote (and skip existing)
gsutil -m cp -n "gs://$BUCKET_NAME/*" .

# modify metadata
gsutil -m setmeta -h "content-disposition:" "gs://$BUCKET_NAME/*"
```
