
To download files with s3 (e.g. for igenomes), use the docker container with `aws` installed. Download the latest image like so:

```sh
singularity pull docker://amazon/aws-cli
```

Now specify the bucket and the target dir in some variables, and run the container: 

```sh

S3_DIR="s3://ngi-igenomes/igenomes/Homo_sapiens/GATK/GRCh38/Sequence/WholeGenomeFasta/"
TARGET_DIR="./reference/Homo_sapiens/GATK/GRCh38/Sequence/WholeGenomeFasta/"

singularity run \
--bind "/data" \
aws-cli_latest.sif \
s3 \
--no-sign-request \
--region eu-west-1 \
sync \
"$S3_DIR" \
"$TARGET_DIR"
```