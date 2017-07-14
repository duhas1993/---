<?php
  require_once 'src/Google/autoload.php';
  session_start();

  $client_id = 'xxxxxxxxxx';
  $client_secret = 'xxxxxxxxxxx';
  $redirect_uri = 'http://mysite.com/googleAuth.php';

  $client = new Google_Client();
  $client->setApplicationName("MyAppName");
  $client->setClientId($client_id);
  $client->setClientSecret($client_secret);
  $client->setRedirectUri($redirect_uri);
  $client->setScopes(array('https://www.googleapis.com/auth/analytics.readonly'));
  $client->setAccessType('offline');   // Gets us our refreshtoken

  if(!$client->getAccessToken() && !isset($_GET['code'])) {
		$authUrl = $client->createAuthUrl();
		echo "<a class='login' href='$authUrl'>Connect Me!</a>";
  }

  if(isset($_GET['code'])){
    $client->authenticate($_GET['code']);
    $token = $client->getAccessToken();
  }

  if (isset($_SESSION['token'])){
		print "<a class='logout' href='".$_SERVER['PHP_SELF']."?logout=1'>LogOut</a><br>";
		print "Access from google: " . $_SESSION['token']."<br>"; 
		  
		$client->setAccessToken($_SESSION['token']);
		$service = new Google_Service_Analytics($client);    
		//use service
  }
