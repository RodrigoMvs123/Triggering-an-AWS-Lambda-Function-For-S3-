# Triggering-an-AWS-Lambda-Function-For-S3-

https://www.youtube.com/watch?v=X8U-6RuZMW8

https://raw.githubusercontent.com/RodrigoMvs123/Triggering-an-AWS-Lambda-Function-For-S3-/main/README.md


https://github.com/RodrigoMvs123/Triggering-an-AWS-Lambda-Function-For-S3-/blame/main/README.md

https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html
https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html
https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html

Index.js

const s3 = require('@aws-sdk/client-s3');

const client = new s3.S3Client();

async function streamToString(stream) {
  return await new Promise((resolve, reject) => {
    const chunks = [];
    stream.on('data', (chunk) => chunks.push(chunk));
    stream.on('error', reject);
    stream.on('end', () => resolve(Buffer.concat(chunks).toString('utf-8')));
  });
}

exports.handler = async (event) => {
  const bucketName = event.Records[0].s3.bucket.name;
  const objectName = event.Records[0].s3.object.key;
  const cmd = new s3.GetObjectCommand({
    Bucket: bucketName,
    Key: objectName,
  });
  const response = await client.send(cmd);
  const data = await streamToString(response.Body);
  console.log(data); // print content of file
};


package.json 
{
  "name": "lambda-s3-read-data",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@aws-sdk/client-s3": "^3.150.0"
  }
}
