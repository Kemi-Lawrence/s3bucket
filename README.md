# Static Website Hosting on S3 Bucket with CloudFront CDN

## Objective
This is a guide for creating a static website hosted on an S3 bucket with a public read policy assigned which also utilized CloudFront as a CDN to deliver content efficiently.


## Steps

### 1. Create an S3 Bucket
- Log in to the AWS Management Console.
- Navigate to the S3 service.
- Click on "Create bucket".
- Follow the prompts to create a new bucket.
- Choose the name of the bucket that you want to enable static website hosting for.
- Click on create bucket

![s3 bucket created after the following the steps above](/images/s3_bucket_created.png)

### 2. Edit Bucket Permission
- Click on the name of the bucket list created
- Choose Properties.
- Under Static website hosting, choose Edit
- Choose Use this bucket to host a website.

![the view when you select host a website](/images/static_website_hosting.png)

- Under Static website hosting, choose Enable.
 ![enabled static website](/images/static_website_enabled.png)


### 3. Upload Website Content to S3 Bucket
- Start by uploading your static website file (HTML) first to the S3 bucket.
- Next, upload your website folder (CSS, JavaScript, images, etc.) to the S3 bucket.

![the upload button for either files or folder is to be clicked. HTML and about the template files were uploaded first, before other folders](/images/click_upload_button.png)

- After the upload, it would show a prompt that it was successful, if there was an error, it would be stated.

![successful prompt after uploading files and folders for the website](/images/successful_upload.png)

- Lastly, the files and folders would be seen as hyperlink upon successful upload.

![hyperlink shown on the upload of website](/images/website_file_upload.png)

### 4. Configure CloudFront Distribution
- Navigate to the CloudFront service.
- Click on "Create Distribution".
- In the "Origin Domain Name", select your S3 bucket from the dropdown.

![creat distribution](/images/create_cloudfront_distribution_configuration.png)

- Enter name for this origin, select your S3 bucket name
- Origin Access, in the dropdown menu select: Origin access control settings (recommended)

![create a new Origin Access Control](/images/create_new_OAC.png)

- Configure other settings as needed.
- Under the "Default root object", type `index.html` which it is already created as a file in the S3 bucket. 
- Click on "Create Distribution".
- Status of the distrubtion would be seen as "Enabled" if it was created successfully.

![Distribution created successfully](/images/cloudfront_distribution_enabled.png)

- Upon successful creation of distribution, S3 bucket policy needs to be updated by coping the policy provided in the distribution to use it in the S3 bucket
![S3 bucket policy was generated](/images/cloudfront_distribution_configuration.png)


### 5. Configure S3 Bucket Policy for Public Read Access
- Navigate to the S3
- Select the name of the bucket created.
- Select the "Permissions" tab.
- Click on edit button on "Bucket Policy" to paste the policy copied from the distribution:
![success](/images/edit_s3_bucket_policy.png)

```json
{
	"Version": "2008-10-17",
	"Id": "PolicyForCloudFrontPrivateContent",
	"Statement": [
		{
			"Sid": "AllowCloudFrontServicePrincipal",
			"Effect": "Allow",
			"Principal": {
				"Service": "cloudfront.amazonaws.com"
			},
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::adekemi/*",
			"Condition": {
				"StringEquals": {
					"AWS:SourceArn": "arn:aws:cloudfront::536131725462:distribution/EXC7EZJE8WC6N"
				}
			}
		}
	]
}
```
- Save changes
![After changes have been made](/images/s3_bucket_policy_configuration.png)

### 6. Testing/Verification of URL
- Once the CloudFront distribution is deployed and your S3 bucket policy has been successfully updated, access your website using the CloudFront domain name.
`https://d3j5c5oz802ne8.cloudfront.net`

![static website is live](/images/production_stage.png)


### Conclusion

You have successfully created a static website hosted on an S3 bucket with public read access as it utilizes the CloudFront as a CDN for faster content delivery.