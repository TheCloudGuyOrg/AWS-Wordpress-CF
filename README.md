# AWS-Wordpress-CF
<h1>Install Instructions</h1>

<h2>Update param.json</h2>

copy param_example.json to param.json

Update the following values:
* EnvironmentSize
* KeyPair
* KMSKey - To Encrypt S3 Bucket
* DestinationBucketName - Destination for S3 Logs
* LogFilePrefix - Prefix for S3 Logs
* DatabaseName
* DatabaseUser
* DatabasePassword
* MultiAZDatabase - True for MultiAZ, False not for MultiAZ

Go to https://api.wordpress.org/secret-key/1.1/salt to generate salt keys and replace in param.json file.

<b>Do not use salts in the example or your wordpress site will be vulnerable to attack</b>

Update the following salt values from the wordpress api site:
* AuthKey
* SecureAuthKey
* LoggedInKey
* NonceKey
* AuthSalt
* SecureAuthSalt
* LoggedInSalt
* NonceSalt

<h2>Create Stack</h2>

aws cloudformation create-stack --stack-name 'STACKNAME-REPLACE' --template-body file://Wordpress.json --parameters file://param.json
