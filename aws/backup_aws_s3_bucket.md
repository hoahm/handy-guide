Backup AWS S3 bucket
___

This guide show you how to backup bucket to a new one.

# Install AWSCLI

    brew install awscli

# Configuration

    aws configure

# Create new bucket

    aws s3 mb s3://my-backup-bucket

# Backup bucket to new bucket

    aws s3 sync s3://my-current-bucket s3://my-backup-bucket

# References:

 * http://docs.aws.amazon.com/cli/latest/reference/s3/sync.html
 * http://sourabhbajaj.com/python/2014/03/31/fix-valueerror-unknown-locale-utf-8/
 * http://blog.tristanmedia.com/2009/09/using-amazons-cloudfront-with-rails-and-paperclip/
