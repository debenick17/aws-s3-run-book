# S3 Static Site Hosting Runbook

## Step 1: Create a bucket

The following instructions provide an overview of how to create your buckets for website hosting

1. Sign in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3/
2. Choose `Create bucket`
3. Enter the `Bucket name` (for example, site-bucket)
4. Choose the Region where you want to create the bucket
5. To accept the default settings and create the bucket, choose `Create `

## Step 2: Enable static website hosting

After you create a bucket, you can enable static website hosting for your bucket. You can create a new bucket or use an existing bucket.

1. Navigate to your AWS S3 Console
2. In the `Buckets` list, choose the name of the bucket that you want to enable static website hosting for
3. Choose `Properties`
4. Under "Static website hosting", choose `Edit`
5. Choose `Use this bucket to host a website`
6. Under Static website hosting, choose Enable.
7. In "Index document", enter the file name of the index document, typically _index.html_
8. (Optional) To provide your own custom error document for 4XX class errors, in "Error document", enter the custom error document file name
9. (Optional) If you want to specify advanced redirection rules, in "Redirection rules", enter JSON to describe the rules.
10. Choose `Save changes` (Amazon S3 enables static website hosting for your bucket. At the bottom of the page, under "Static website hosting", you see the website endpoint for your bucket.)
11. Under "Static website hosting", note the "Endpoint" (The Endpoint is the Amazon S3 website endpoint for your bucket. After you finish configuring your bucket as a static website, you can use this endpoint to test your website.)

## Step 3: Edit Block Public Access settings

By default, Amazon S3 blocks public access to your account and buckets. If you want to use a bucket to host a static website, you can use these steps to edit your block public access settings.

1. Make sure you are logged in to your AWS S3 Console
2. Choose the name of the bucket that you have configured as a static website
3. Choose `Permissions`
4. Under "Block public access (bucket settings)", choose `Edit`
5. Clear "Block all public access", and choose `Save changes`

## Step 4: Add a bucket policy that makes your bucket content publicly available

After you edit S3 Block Public Access settings, you can add a bucket policy to grant public read access to your bucket. When you grant public read access, anyone on the internet can access your bucket.

1. Under "Buckets", choose the name of your bucket
2. Choose `Permissions`
3. Under "Bucket Policy", choose `Edit`
4. To grant public read access for your website, copy the following bucket policy, and paste it in the "Bucket policy editor"

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}
```

5. Update the "Resource" to your bucket name ("_site-bucket_", for example if that's your bucket name)
6. Choose `Save changes`

## Step 5: Configure an index document

When you enable static website hosting for your bucket, you enter the name of the index document (for example, _index.html_). After you enable static website hosting for the bucket, you upload an HTML file with this index document name to your bucket.

**To configure the index document**

1. Create an _index.html_ file
   If you don't have an index.html file, you can create a file using "vim", copy and paste the following HTML:

```
<html xmlns="http://www.w3.org/1999/xhtml" >
<head>
    <title>My Website Home Page</title>
</head>
<body>
  <h1>Welcome to my website</h1>
  <p>Now hosted on Amazon S3!</p>
</body>
</html>
```

2. Save the index file locally
3. Go to your AWS S3 Console
4. In the "Buckets" list, choose the name of the bucket that you want to use to host a static website
5. Enable static website hosting for your bucket, and enter the exact name of your index document (for example, _index.html_)
6. To upload the index document to your bucket, do one of the following
   > - Drag and drop the index file into the console bucket listing
   > - Choose "Upload", and follow the prompts to choose and upload the index file
7. (Optional) Upload other website content to your bucket

## Step 6: Configure an error document

When you enable static website hosting for your bucket, you enter the name of the error document (for example, _404.html_). After you enable static website hosting for the bucket, you upload an HTML file with this error document name to your bucket.

**To configure an error document**

1. Create an error document, for example _404.html_
2. Save the error document file locally
3. Navigate to you AWS S3 Console
4. In the "Buckets" list, choose the name of the bucket that you want to use to host a static website
5. Enable static website hosting for your bucket, and enter the exact name of your error document (for example, _404.html_)
6. To upload the index document to your bucket, do one of the following
   > - Drag and drop the error document file into the console bucket listing
   > - Choose "Upload", and follow the prompts to choose and upload the index file

## Step 7: Test your website endpoint

After you configure static website hosting for your bucket, you can test your website endpoint

1. Under "Buckets", choose the name of your bucket
2. Choose "Properties"
3. At the bottom of the page, under "Static website hosting", choose your "Bucket website endpoint"

## Step 8: Clean up

If you created your static website only as a learning exercise, delete the AWS resources that you allocated so that you no longer accrue charges. After you delete your AWS resources, your website is no longer available.
