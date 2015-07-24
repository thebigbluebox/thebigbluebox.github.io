---
title:  "Uploading files to a LAMP stack and keep track of it with a CMS"
date:   2014-12-22 9:00:00
description: Content management system built with LAMP
---
#Uploading files to a LAMP stack and keep track of it with a CMS Part 1
>
######This is technically a continuation of Dave's tutorial on how to create a Blog with LAMP [here](https://daveismyname.com/creating-a-blog-from-scratch-with-php-bp#.Va2tvpNViko), and I've added my own twist and turned it into a small CMS by allowing users to upload photos and keep track of their uploads. I will be referencing a lot of the files back to what was created on Dave's tutorial

If we are continuing from the tutorial, we should have a completed admin console that looks like this:



###The upload script
Lets create the upload script, which will handle the interaction of the post request that has the files attached to it. Along with storing the file we will also add the metadata from the request's body and store it in the MySQL database

Lets first define a few variables such as the extensions that we will allow for upload, we will define it in an array called $allowedExts. Next we will store the temp file name into an array that is done by parsing the period before the extesnion using the explode command, so we will define it in $temp. Our extension will naturally use the the end of the array assuming the file was named correctly to store the extension of the file, we will define this use $extension. From the request we will assume the form will also contain an title and description field which we will parse from the request body. I've defined the two with $imgTitle and $imgeDesc.

>Your variables should look like this
{% highlight ruby %}
		$allowedExts = array("gif", "jpeg", "jpg", "png");
		$temp = explode(".", $_FILES["file"]["name"]);
		$extension = end($temp);
		$imgTitle = $_POST['imgTitle'];
		$imgDesc = $_POST['imgDesc'];
{% endhighlight %}

Now that we've checked some basic request body information we can now move on to verifying that the file isn't too large and that the file type from the request header matches one of the cases and the extension is valid and within our $allowedExts array. We will require a title for every image file that is uploaded so we will print an error if the title is missing.

{% highlight ruby %}
		echo $_FILES["file"]["size"];
		if($_FILES["file"]["size"] > 200000000){
			echo "File is too large";
		}
		echo $_FILES["file"]["type"];
		if ((($_FILES["file"]["type"] == "image/gif")
		|| ($_FILES["file"]["type"] == "image/jpeg")
		|| ($_FILES["file"]["type"] == "image/jpg")
		|| ($_FILES["file"]["type"] == "image/pjpeg")
		|| ($_FILES["file"]["type"] == "image/x-png")
		|| ($_FILES["file"]["type"] == "image/png"))
		&& ($_FILES["file"]["size"] < 200000000)
		&& in_array($extension, $allowedExts)) {
		if($imgTitle == null){
			echo "<p class=\"error\"> Please Enter a Title</p>";
			echo '<a href="javascript:history.go(-1)"><h2> Go Back </h2></a>';
		}
{% endhighlight %}

We added to the above code we will now check for any errors that could have occured during the upload, of which we will send back and not continue the upload.

{% highlight ruby %}
		else{
			if ($_FILES["file"]["error"] > 0){
			echo "<p class=\"error\">Return Code: " . $_FILES["file"]["error"] . "</p>". "<br>";
			} 
			else {
				//diagnostic comments to make sure it uploaded
				echo "../img/Upload: " . $_FILES["file"]["name"] . "<br>";
				echo "Type: " . $_FILES["file"]["type"] . "<br>";
				echo "Size: " . ($_FILES["file"]["size"] / 1024) . " kB<br>";
				echo "Temp file: " . $_FILES["file"]["tmp_name"] . "<br>";
				
				//if the file exists already within the upload directory we will stop the upload
				if (file_exists("../img/upload/" . $_FILES["file"]["name"])) {
					echo "<p class=\"error\">".$_FILES["file"]["name"] . " already exists</p> ";
				}
{% endhighlight %}

After the file has gone through our extensive verification we can finally move on to the most crucial step of the file upload, which is to move the file from the temporary folder to the permenant folder in /img/upload. We accomplish this using the move_upload_file command. We will also be using parameterized sql statements to store and track the uploaded files. The MySQL database will hold the title, description, and the file path.

{% highlight ruby %}	
				else {
				move_uploaded_file($_FILES["file"]["tmp_name"],
				"../img/upload/" . $_FILES["file"]["name"]);
				echo "Stored in: " . "../img/upload/" . $_FILES["file"]["name"];
				if(!isset($error)){
					try {
						//creates the file path to store in the database
						$path = "/img/upload/".$_FILES["file"]["name"];
						
						//insert into database
						$stmt = $db->prepare('INSERT INTO picture_directory (imgTitle,imgDesc,imgPath) VALUES ( :imgTitle, :imgDesc, :imgPath)') ;
						$stmt->execute(array(
							':imgTitle' => htmlspecialchars($imgTitle),
							':imgDesc' => htmlspecialchars($imgDesc),
							':imgPath' => $path
						));
						
						//redirect to index page
						header('Location: imageupload.php?action=added');
						exit;
					} catch(PDOException $e) {
					    echo $e->getMessage();
					}
					}
				}
			}
		} 
	}?>
{% endhighlight %}

The end script should look something like this with the authentication and included header files for styling

{% highlight ruby%}
<?php //include config
require_once('../includes/config.php');
//if not logged in redirect to login page
if(!$user->is_logged_in()){ header('Location: login.php'); }
?>
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Admin - Image Upload Script</title>
  <?php include('imports.php'); ?>
</head>
<body>

<div id="wrapper">

	<?php include('menu.php');?>
		<p><a href="imageupload.php">Images Index</a></p>
	<?php 
		$allowedExts = array("gif", "jpeg", "jpg", "png");
		$temp = explode(".", $_FILES["file"]["name"]);
		$extension = end($temp);
		$imgTitle = $_POST['imgTitle'];
		$imgDesc = $_POST['imgDesc'];

		echo $_FILES["file"]["size"];
		if($_FILES["file"]["size"] > 200000000){
			echo "File is too large";
		}
		echo $_FILES["file"]["type"];
		if ((($_FILES["file"]["type"] == "image/gif")
		|| ($_FILES["file"]["type"] == "image/jpeg")
		|| ($_FILES["file"]["type"] == "image/jpg")
		|| ($_FILES["file"]["type"] == "image/pjpeg")
		|| ($_FILES["file"]["type"] == "image/x-png")
		|| ($_FILES["file"]["type"] == "image/png"))
		&& ($_FILES["file"]["size"] < 200000000)
		&& in_array($extension, $allowedExts)) {
		if($imgTitle == null){
			echo "<p class=\"error\"> Please Enter a Title</p>";
			echo '<a href="javascript:history.go(-1)"><h2> Go Back </h2></a>';
		}
		else{
			if ($_FILES["file"]["error"] > 0){
			echo "<p class=\"error\">Return Code: " . $_FILES["file"]["error"] . "</p>". "<br>";
			} 
			else {
				echo "../img/Upload: " . $_FILES["file"]["name"] . "<br>";
				echo "Type: " . $_FILES["file"]["type"] . "<br>";
				echo "Size: " . ($_FILES["file"]["size"] / 1024) . " kB<br>";
				echo "Temp file: " . $_FILES["file"]["tmp_name"] . "<br>";
				if (file_exists("../img/upload/" . $_FILES["file"]["name"])) {
					echo "<p class=\"error\">".$_FILES["file"]["name"] . " already exists</p> ";
				}
				else {
				move_uploaded_file($_FILES["file"]["tmp_name"],
				"../img/upload/" . $_FILES["file"]["name"]);
				echo "Stored in: " . "../img/upload/" . $_FILES["file"]["name"];
				if(!isset($error)){
					try {
						$path = "/img/upload/".$_FILES["file"]["name"];
						
						//insert into database
						$stmt = $db->prepare('INSERT INTO picture_directory (imgTitle,imgDesc,imgPath) VALUES ( :imgTitle, :imgDesc, :imgPath)') ;
						$stmt->execute(array(
							':imgTitle' => htmlspecialchars($imgTitle),
							':imgDesc' => htmlspecialchars($imgDesc),
							':imgPath' => $path
						));
						
						//redirect to index page
						header('Location: imageupload.php?action=added');
						exit;
					} catch(PDOException $e) {
					    echo $e->getMessage();
					}
					}
				}
			}
		} 
	}?>
</div>
</body>
</html>
{% endhighlight %}