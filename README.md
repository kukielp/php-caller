Createa a Static website to call the PHP lambda function to add 2 numbers

```bash

mkdir static
cd static/

curl https://raw.githubusercontent.com/kukielp/php-caller/master/index.html --output index.html

export bucket='paul-test-123'
aws s3 mb s3://$bucket --region ap-southeast-2                     
aws s3 cp index.html s3://$bucket/index.html
aws s3 website s3://$bucket/ --index-document index.html

cat > policy.json <<POLICY
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadForGetBucketObjects",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::paul-test-123/*"
        }
    ]
}
POLICY

aws s3api put-bucket-policy --bucket $bucket --policy file://policy.json

printf '\nBucket URL is: http://'$bucket'.s3-website-ap-southeast-2.amazonaws.com\n\n'
```