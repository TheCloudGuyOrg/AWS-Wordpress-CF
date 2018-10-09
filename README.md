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

<h2>Install Wordpress</h2>

Go to the WPRoot URL and run through the installation setup for wordpress.

<h2>Install W3 Total Cache Plug-In</h2>
 * On the Wordpress Dashboard go to Plugins and search for W3 Total Cache, install and activate.
 * Go to W3 Total Cache Plugin and Select Settings and then CDN
 * Enable CDN and select Amazon Simple Storage S3 as CDN type. Save all settings
 * Input Access Key, Secret Key, and Bucket data for the CDN. Keys and Bucket are located in the Output Section of the CFN
 * Select Use CDN links for the Media Library on admin pages
 * select Export changed files automatically
 * Select cookie domain to "www.example.com"
 * Save all settings
 * Upload wp-includes, theme files, custom files and export the media library to the CDN through the W3 Total Cache Plugins
 * Note: If you see the plugin disappear dont worry. Just repeat the steps above. The load balancer switched to the other EC2 Instance before it finished the plugin setup.

 <h2>Configure HTTPS</h2>
 * Change the AWS Load Balancer to redirect port 80 to port 443. Currently Cloudformation will only let you create a forward listener not a redirect
 * Install Really Simple SSL
 * Go back to W3 Total Cache and Export Files again.
