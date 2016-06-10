---
title:  "Uploading files to a LAMP stack and keep track of it with a CMS Part 2"
date:   2014-12-23 9:00:00
description: Content management system built with LAMP
---
#Uploading files to a LAMP stack and keep track of it with a CMS Part 2
>
###### This is technically a continuation of Dave's tutorial on how to create a Blog with LAMP [here](https://daveismyname.com/creating-a-blog-from-scratch-with-php-bp#.Va2tvpNViko), and I've added my own twist and turned it into a small CMS by allowing users to upload photos and keep track of their uploads. I will be referencing a lot of the files back to what was created on Dave's tutorial

So in part 1 we essentially created the backend portion to handle the handover of files from the temp folder into the permenant folder of /img/upload. In this section we will write the front end to handle uploading, editing, and the total overview. Since I assume you to have atleast understood part 1, you must be competent with PHP. And the code here are now usually self documenting so I won't speak much.

### The upload page

{% highlight ruby %}
<?php //include config
require_once('../includes/config.php');
//if not logged in redirect to login page
if(!$user->is_logged_in()){ header('Location: login.php'); }
?>
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Admin - Upload Image</title>
  <?php include('imports.php'); ?>
</head>
<body>

<div id="wrapper">

	<?php include('menu.php');?>
		<p><a href="imageupload.php">Images Index</a></p>
		<h2>Upload Images</h2>
		
		//we use the imageupload-script we made in part 1
		<form action="imageupload-script.php" method="post" enctype="multipart/form-data">
			
			<p><label>Title</label><br />
			<input type='text' name='imgTitle'></p>
			
			<p><label>Description</label><br />
			<textarea name='imgDesc' cols='60' rows='10'></textarea></p>
			
			<label for="file">Filename:</label>
			<input type="file" name="file" id="file"><br>
			
			<label for="inGallery">Show in Gallery: </label>
			<input type="checkbox" name="inGallery" id="inGallery" value="checked"></br>

			<input type="submit" name="submit" value="Submit">
		</form>
</div>
</body>
</html>
{% endhighlight %}

###The edit image page

{% highlight ruby %}
<?php //include config
require_once('../includes/config.php');
//if not logged in redirect to login page
if(!$user->is_logged_in()){ header('Location: login.php'); }
?>
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Admin - Edit Image</title>
  <?php include('imports.php'); ?>
</head>
<body>

<div id="wrapper">

	<?php include('menu.php');?>
	<p><a href="imageupload.php">Images Index</a></p>

	<h2>Edit Image</h2>
	<?php
	//if form has been submitted process it
	if(isset($_POST['submit'])){
		//$_POST = array_map( 'stripslashes', $_POST );
		//collect form data
		extract($_POST);
		//very basic validation
		if($imgTitle ==''){
			$error[] = 'Please enter the title.';
		}
		if(!isset($error)){
			try {
				//$imgTitle = slug($imgTitle);
				//insert into database
				$stmt = $db->prepare('UPDATE picture_directory SET imgTitle = :imgTitle, imgDesc = :imgDesc WHERE id = :id') ;
				$stmt->execute(array(
					
					':imgTitle' => htmlspecialchars($imgTitle),
					':imgDesc' => htmlspecialchars($imgDesc),
					'id' => $id
				));
				$stmt = $db->prepare('DELETE FROM picture_directory_album WHERE imgID = :imgID');
				$stmt->execute(array(':imgID' => $id));
				
				//redirect to index page
				header('Location: imageupload.php?action=updated');
				exit;
			} catch(PDOException $e) {
			    echo $e->getMessage();
			}
		}
	}
	?>


	<?php
	//check for any errors
	if(isset($error)){
		foreach($error as $error){
			echo $error.'<br />';
		}
	}
		try {
			$stmt = $db->prepare('SELECT id, inGallery, imgTitle, imgDesc FROM picture_directory WHERE id = :id') ;
			$stmt->execute(array(':id' => $_GET['id']));
			$row = $stmt->fetch(); 
		} catch(PDOException $e) {
		    echo $e->getMessage();
		}
	?>

		<h2>Edit Images</h2>
		<form action='' method='post'>
			<input type='hidden' name='id' value='<?php echo $row['id'];?>'>

			<p><label>Title</label><br />
			<input type='text' name='imgTitle' value='<?php echo $row['imgTitle'];?>'></p>

			<p><label>Description</label><br />
			<textarea name='imgDesc' cols='60' rows='10'><?php echo $row['imgDesc'];?></textarea></p>

		<input type='submit' name='submit' value='Update'>

		</form>

</div>

</body>
</html>	

{% endhighlight %}

###The admin overview page for images

{% highlight ruby %}
<?php //include config
require_once('../includes/config.php');
//if not logged in redirect to login page
if(!$user->is_logged_in()){ header('Location: login.php'); }
//function for delete 	
if(isset($_GET['delimg'])){
	$stmt = $db->prepare('SELECT FROM picture_directory WHERE id = :id');
	$row = $stmt->fetch();
	$filepath = $row['imgPath'];
	if(is_readable($filepath)){
		if(unlink($filepath)) {echo "Deleted file "; }
		else{echo "File can't be deleted";}
	}else{
		echo "File is not present";
	}
	$stmt = $db->prepare('DELETE FROM picture_directory WHERE id = :id');
	$stmt->execute(array(':id' => $_GET['delimg']));
	header('Location: imageupload.php?action=deleted');
	exit;
}
?>
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Admin - Images</title>
  <?php include('imports.php'); ?>
  <script language="JavaScript" type="text/javascript">
  function delimg(id, title)
  {
	  if (confirm("Are you sure you want to delete '" + title + "'"))
	  {
	  	window.location.href = 'imageupload.php?delimg=' + id;
	  }
  }
  </script>
</head>
<body>

<div id="wrapper">

	<?php include('menu.php');?>
	
	<h2>Images Upload</h2>
	<p><a href="add-imageupload.php">Upload Image</a></p>
	<?php
	if(isset($_GET['action'])){ 
		echo '<h3>Image '.$_GET['action'].'.</h3>'; 
	}
	?>
	<table>
	<tr>
		<th>Title</th>
		<th>ID</th>
		<th>Action</th>
	</tr>
	<?php 
		try {
			$stmt = $db->query('SELECT id, imgTitle FROM picture_directory ORDER BY id DESC');
			while($row = $stmt->fetch()){
				echo '<tr>';
				echo '<td>'.$row['imgTitle'].'</td>';
				echo '<td>'.$row['id'].'</td>';
				?>
				<td>
					<a href="edit-imageupload.php?id=<?php echo $row['id'];?>">Edit</a> | 
					<a href="javascript:delimg('<?php echo $row['id'];?>','<?php echo htmlspecialchars($row['imgTitle']);?>')">Delete</a>
				</td>
				<?php
				echo '</tr>';
			}
		}catch(PDOException $e) {
		    echo $e->getMessage();
		}
	?>
</table>
</div>
</body>
</html>
{% endhighlight %}