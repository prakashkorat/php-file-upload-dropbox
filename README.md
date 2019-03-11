
1) Login dropbox using this URL :
	- https://www.dropbox.com/1/oauth2/authorize?client_id=<your client_id here>&response_type=code&redirect_uri=<optional - your redirect url here>

	- After login you get code. This code use in second api.


2) https://api.dropboxapi.com/1/oauth2/token

	Request :

	method : POST
	code : <Set login dropbox generated code here>
	grant_type : authorization_code
	client_id : <your client id>
	client_secret : <your secret>
	redirect_uri : <optional - your redirect url here>

	Response :

	{
		"access_token": "S4OSucqTlTAAAAAAAAAAHVDbcI_Red8DX6Ii6PMod_idBcqYTwVEqxNnAg8-fhO7",
		"token_type": "bearer",
		"uid": "2018021027",
		"account_id": "dbid:AACTR-VoEJIXzGNz8LNxWG30lj2sxwOP6-8"
	}

	- This access token use in third api.

3) This PHP code for upload file into dropbox

	$api_url = 'https://content.dropboxapi.com/2/files/upload'; //dropbox api url
	$token = 'S4OSucqTlTAAAAAAAAAAF_ilnh7ok-XRadqNWwEmQNO4UOz17iGbB2I0MzWr4chp'; // oauth token
	$filename = "test.php";
	$headers = array('Authorization: Bearer '. $token,
		'Content-Type: application/octet-stream',
		'Dropbox-API-Arg: '.
		json_encode(
			array(
				"path"=> '/'. basename($filename),
				"mode" => "add",
				"autorename" => true,
				"mute" => false
			)
		)
	);

	$ch = curl_init($api_url);

	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
	curl_setopt($ch, CURLOPT_POST, true);

	$path = $filename;
	$fp = fopen($path, 'rb');
	$filesize = filesize($path);

	curl_setopt($ch, CURLOPT_POSTFIELDS, fread($fp, $filesize));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	// curl_setopt($ch, CURLOPT_VERBOSE, 1); // debug

	$response = curl_exec($ch);
	$http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
	echo($response.'<br/>');
	echo($http_code.'<br/>');

	curl_close($ch);
