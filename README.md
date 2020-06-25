# AWS
FACE DETECTION AWS APP
INTRODUCTION:
Face recognition refers to the technology capable of identifying or verifying the identity of subjects in images or videos. 
This makes face recognition the most user friendly biometric modality. It also means that the range of potential applications of face recognition is wider, as it can be deployed in environments where the users are not expected to cooperate with the system, such as in surveillance systems. Other common applications of face recognition include access control, fraud detection, identity verification and social media.

PROPOSED SYSTEM:
Amazon Rekognition can detect faces in images and videos. With Amazon Rekognition, you can get information about where faces are detected in an image or video, facial landmarks such as the position of eyes, and detected emotions (for example, appearing happy or sad). You can also compare a face in an image with faces detected in another image.
When you provide an image that contains a face, Amazon Rekognition detects the face in the image, analyzes the facial attributes of the face, and then returns a percent confidence score for the face and the facial attributes that are detected in the image.

WORKING OF AWS REKOGNITION:
Rekognition Image uses deep neural network models to detect and label thousands of objects and scenes in your images, and we are continually adding new labels and facial recognition features to the service. With Rekognition Image, you only pay for the images you analyze and the face metadata you store.

MODULES:
•	EC2
•	S3
•	Rekognition

  
EC2:
       EC2 allows a user to rent virtual computers on which to run their own computer applications.
•	Build face detection application on our own
•	We are going to build our own application that’s why we need a virtual computer
       EC2 provides virtual computers or machines for us to create our application(FAC) on top of the OS and HW provided by AWS.
•	Select OS
•	Take and configure ram and processors
•	Configure the storage(memory)
•	Configuring the security

S3:
•	It is the storage for the internet provided by AWS
•	Designed to make web scale computing easy.

Objects:
	Object is the data and metadata.
In S3 we only store only objects.
We may store mp3, mp4, pdf, text files but we refer those as objects.
BUCKET:
Container for objects
OBJECT:
Actual data and metadata
KEYS:
Unique identifier for an object

REKOGNITION:
        Amazon Rekognition can detect faces in images and videos. This section covers non-storage operations for analyzing faces. With Amazon Rekognition, you can get information about where faces are detected in an image or video, facial landmarks such as the position of eyes, and detected emotions (for example, appearing happy or sad). You can also compare a face in an image with faces detected in another image.
      When you provide an image that contains a face, Amazon Rekognition detects the face in the image, analyzes the facial attributes of the face, and then returns a percent confidence score for the face and the facial attributes that are detected in the image.

WORKING:
•	Upload an image in telegram bot. It directly comes to EC2.
•	EC2(compute service) uploads or sends the image to S3.
•	In S3 the image gets stored.
•	Now EC2 invokes Rekognition service to understand the features of the given image.
•	Rekognition takes the image from S3, recognise that particular image and understands and interprets certain results
•	Rekognition passes that results to EC2
•	EC2 directly interprets that results and sends the result to the telegram Bot.


IMPLEMENTATION CODE:

<?php

/*

Install php - sudo yum install php
curl -sS https://getcomposer.org/installer | php cd /var/www/html
sudo mkdir face cd face
sudo php -d memory_limit=-1 ~/composer.phar require aws/aws-sdk-php

In case if you get memory error -
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024 sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1

sudo wget https://i.pinimg.com/originals/b9/7e/a3/b97ea33b5842c7894b804923c6c05580.jpg sudo mv b97ea33b5842c7894b804923c6c05580.jpg sample.jpg

*/ error_reporting(0);
require_once( DIR 	. '/vendor/autoload.php'); use Aws\S3\S3Client;
use Aws\Rekognition\RekognitionClient;


$bucket = 'funky-aws-bucket';
$keyname = 's1.jpg';

$s3 = S3Client::factory([ 'profile'	=> 'default', 'region'	=> 'us-east-2', 'version'	=> '2006-03-01',
'signature' => 'v4'
]);

try {
// Upload data.
$result = $s3->putObject([ 'Bucket'	=> $bucket,
'Key'	=> $keyname, 'SourceFile'	=> DIR . "/$keyname", 'ACL'	=> 'public-read'
]);

// Print the URL to the object.
$imageUrl = $result['ObjectURL']; if($imageUrl) {
echo "Image upload done... Here is the URL: " . $imageUrl;
}
} catch (Exception $e) {
echo $e->getMessage() . PHP_EOL;
}

  <?php

/*

Install php - sudo yum install php
curl -sS https://getcomposer.org/installer | php cd /var/www/html
sudo mkdir face cd face
sudo php -d memory_limit=-1 ~/composer.phar require aws/aws-sdk-php

In case if you get memory error -
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024 sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1

sudo wget https://i.pinimg.com/originals/b9/7e/a3/b97ea33b5842c7894b804923c6c05580.jpg sudo mv b97ea33b5842c7894b804923c6c05580.jpg sample.jpg
Incase if you are getting any class NOT found error, follow these steps sudo yum remove php*
sudo yum remove httpd* sudo yum clean all sudo yum upgrade -y
sudo amazon-linux-extras install php7.2
sudo yum install php-json php-xml php-cli php-mbstring sudo yum install httpd

*/
// error_reporting(0);
require_once(_DIR _. '/vendor/autoload.php'); use Aws\S3\S3Client;
use Aws\Rekognition\RekognitionClient;


$bucket = 'funky-aws-bucket';
$keyname = 's1.jpg';

$s3 = new S3Client([
'region'	=> 'us-east-2', 'version'	=> '2006-03-01',
'signature' => 'v4'
]);

try {
// Upload data.
$result = $s3->putObject([ 'Bucket'	=> $bucket,
'Key'	=> $keyname, 'SourceFile'	=> DIR_. "/$keyname", 'ACL'	=> 'public-read-write'
]);

// Print the URL to the object.
$imageUrl = $result['ObjectURL']; if($imageUrl) {
echo "Image upload done... Here is the URL: " . $imageUrl;

$rekognition = new RekognitionClient([ 'region'	=> 'us-east-2',
'version'	=> 'latest',
]);

$result = $rekognition->detectFaces([ 'Attributes'	=> ['DEFAULT'], 'Image' => [
'S3Object' => [
'Bucket' => $bucket, 'Name'	=>	$keyname,
'Key'	=>	$keyname,
],
],
]);

echo "Totally there are " . count($result["FaceDetails"]) . "
faces";
}
} catch (Exception $e) {
echo $e->getMessage() . PHP_EOL;
}


CONCLUSION:
We have build our own application i.e. face detection application using aws where the face is detected in the image which is uploaded by ourselves. The future work will be like detecting the faces based on the facial expressions like eyes opened or out, smiling or not etc.
