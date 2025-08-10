# Cloud Computing Essentials
- Review a bucket policy to secure a bucket in Amazon S3
- Enable static website hosting on the S3 bucket
- Test access to the webpage hosted on Amazon S3


## Amazon S3
- Amazons Simple Storage Service (S3) can be used to store any type of data and retrieve any amount of data from anywhere. 
- Object storage service : Object has data and meta data (stored as name-value pairs). Objects are stored 
- Object is uniquely identified by a key in an S3 bucket. 
- s3 bucket hosts on an URL, which returns the HTML file, known as the root object. 
- Bucket policy defines: what data and who access has access to the bucket. 
- Policies allows use to set time limits. 
	- Resource Policies: Attached directly to buckets and objects
		- Access Control List : 
		- Bucket Policy: << Recommended
		- ![[Screenshot 2025-08-09 at 22.54.44.png]]
	- User Policies: AWS IAM policies. << reccomended
		- ![[Screenshot 2025-08-09 at 22.56.54.png]]
	- 