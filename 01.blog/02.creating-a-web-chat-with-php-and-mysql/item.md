---
title: 'Creating a web chat with PHP and MySQL'
published: true
visible: true
icon: gear
date: '02-04-2015 15:14'
taxonomy:
    category:
        - Tutorial
    tag:
        - PHP
        - MySQL
        - JavaScript
---

In this Tutorial, I'll show you how you can write your very own web-based Chat App using HTML, CSS, JavaScript/jQuery, PHP and MySQL.

~~Check out the live example~~ (Disabled due to abuse)

[Download the source](https://f-rilling.com/user/data/files/f-rilling.com_rChat.7z)

For a full functioning Web-Chat we'll need three basic components:

*   A Client-Side HTML Document that sends the Chat Text and receives Data from the database
*   A PHP file which writes user input into the database
*   A database (we'll use MySQL in this Example) to save and manage the messages

So let's start by creating our Front End.

## The User Interface

Let's start by creating a basic HTML template with jquery and some pretty standard code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>rChat</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/5.0.0/normalize.min.css" type="text/css" media="screen"
    />
    <link rel="stylesheet" href="css/main.css" type="text/css" media="screen" />
</head>
<body>
    <div class="container">
        <header class="header">
            <h1>rChat</h1>
        </header>
        <main>
            <!-- Our app will go here-->
        </main>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
    <script src="js/rChat.js"></script>
</body>
</html>
```

Most Chat Apps consist of a basic form with an output field to show the messages,
 an input field for our user to write into, optionally an input field for the username and last but not least a button to send the message.

```html
<div class="userSettings">
    <label for="userName">Username:</label>
    <input id="userName" type="text" placeholder="Username" maxlength="32" value="Somebody">
</div>
<div class="chat">
    <div id="chatOutput"></div>
    <input id="chatInput" type="text" placeholder="Input Text here" maxlength="128">
    <button id="chatSend">Send</button>
</div>
```

So our whole html now looks like this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>rChat</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/5.0.0/normalize.min.css" type="text/css" media="screen"
    />
    <link rel="stylesheet" href="css/main.css" type="text/css" media="screen" />
</head>
<body>
    <div class="container">
        <header class="header">
            <h1>rChat</h1>
        </header>
        <main>
            <div class="userSettings">
                <label for="userName">Username:</label>
                <input id="userName" type="text" placeholder="Username" maxlength="32" value="Somebody">
            </div>
            <div class="chat">
                <div id="chatOutput"></div>
                <input id="chatInput" type="text" placeholder="Input Text here" maxlength="128">
                <button id="chatSend">Send</button>
            </div>
        </main>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
    <script src="js/rChat.js"></script>
</body>
</html>
```

Of course, our chat doesn't look like a chat at the moment, but hey, that's what CSS is for right? so let's style it a bit:

```css
/*Border-box reset*/
html {
    box-sizing: border-box;
}

*,
*:before,
*:after {
    box-sizing: inherit;
}


/*Chat styling*/
body {
    display: flex;
    justify-content: center;
    align-items: center;
}

.userSettings{
    margin-bottom: 20px;
}

.chat {
    max-width: 400px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
}

.chat #chatOutput {
    overflow-y: scroll;
    height: 280px;
    width: 100%;
    border: 1px solid #777;
}

.chat #chatOutput p{
    margin:0;
    padding:5px;
    border-bottom: 1px solid #bbb;
    word-break: break-all;
}

.chat #chatInput {
    width: 75%;
}

.chat #chatSend {
    width: 25%;
}
```

Now that looks better. Feel free to style it even more.

## Scripts

Our chat doesn't really do anything right now, so let's look into that.
 First up we'll setup jQuery's `ready()` and store the important elements into variables:

```javascript
$(document).ready(function () {
    var $userName = $("#userName");
    var $chatOutput = $("#chatOutput");
    var $chatInput = $("#chatInput");
    var $chatSend = $("#chatSend");
});
```

So here is what we want to do: Whenever the "Send" Button is pressed, we want our script to send the chat input to the server.
 Also we need an interval that regulary checks for new chat messags from the server to display.

```javascript
$(document).ready(function () {
    var chatInterval = 250; //refresh interval in ms
    var $userName = $("#userName");
    var $chatOutput = $("#chatOutput");
    var $chatInput = $("#chatInput");
    var $chatSend = $("#chatSend");

    $chatSend.click(function () {
        sendMessage();
    });

    setInterval(function () {
        retrieveMessages();
    }, chatInterval);
});
```

Now we need to define what `sendMessage()` and `retrieveMessages()` will do:

```javascript
$(document).ready(function () {
    var chatInterval = 250; //refresh interval in ms
    var $userName = $("#userName");
    var $chatOutput = $("#chatOutput");
    var $chatInput = $("#chatInput");
    var $chatSend = $("#chatSend");

    function sendMessage() {
        var userNameString = $userName.val();
        var chatInputString = $chatInput.val();

        $.get("./write.php", {
            username: userNameString,
            text: chatInputString
        });

        $userName.val("");
        retrieveMessages();
    }

    function retrieveMessages() {
        $.get("./read.php", function (data) {
            $chatOutput.html(data); //Paste content into chat output
        });
    }


    $chatSend.click(function () {
        sendMessage();
    });

    setInterval(function () {
        retrieveMessages();
    }, chatInterval);
});
```
Now we send the content of both the username and the chat-input to the `write.php` via GET.
 `retrieveMessages` on the other hand loads the content of `read.php` into the chat-output.

## Writing into the Database

In our "write.php" we'll want to to our database, to which we must connect first.
We supply the login-data from a file called `_connect.php`.

write.php:

```php
<?php
require("./_connect.php");

//connect to db
$db = new mysqli($db_host,$db_user, $db_password, $db_name);
if ($db->connect_errno) {
    //if the connection to the db failed
    echo "Failed to connect to MySQL: (" . $db->connect_errno . ") " . $db->connect_error;
}

?>
```

_connect.php:

```php
<?php
//Enter the connection details of your MySQL database here
$db_host="localhost";
$db_user="root";
$db_password="";
$db_name="chatapp";

?>
```

We receive the user input from the URL so we can access them with "$_GET['']". Limiting the maximal length of the input makes sure users dont paste huge strings into our db.

**Make sure to use `mysqli\_escape\_string` and `htmlentities` to escape the user input! Otherwise, your chat is vulnerable to [SQL-Injections!](https://en.wikipedia.org/wiki/SQL_injection)**



```php
<?php
require("./_connect.php");

//connect to db
$db = new mysqli($db_host,$db_user, $db_password, $db_name);
if ($db->connect_errno) {
    //if the connection to the db failed
    echo "Failed to connect to MySQL: (" . $db->connect_errno . ") " . $db->connect_error;
}


//get userinput from url
$username=substr($_GET["username"], 0, 32);
$text=substr($_GET["text"], 0, 128);
//escaping is extremely important to avoid injections!
$nameEscaped = htmlentities(mysqli_real_escape_string($db,$username)); //escape username and limit it to 32 chars
$textEscaped = htmlentities(mysqli_real_escape_string($db, $text)); //escape text and limit it to 128 chars
?>

```

Now we are ready to write the user input into the Database. This can be done by sending a MySQL query to the database containing our data.
We want the server to show an error when it doesn't work, and to show the a success message if it succeeds.

```php
<?php
require("./_connect.php");

//connect to db
$db = new mysqli($db_host,$db_user, $db_password, $db_name);
if ($db->connect_errno) {
    //if the connection to the db failed
    echo "Failed to connect to MySQL: (" . $db->connect_errno . ") " . $db->connect_error;
}


//get userinput from url
$username=substr($_GET["username"], 0, 32);
$text=substr($_GET["text"], 0, 128);
//escaping is extremely important to avoid injections!
$nameEscaped = htmlentities(mysqli_real_escape_string($db,$username)); //escape username and limit it to 32 chars
$textEscaped = htmlentities(mysqli_real_escape_string($db, $text)); //escape text and limit it to 128 chars



//create query
$query="INSERT INTO chat (username, text) VALUES ('$nameEscaped', '$textEscaped')";
//execute query
if ($db->real_query($query)) {
    //If the query was successful
    echo "Wrote message to db";
}else{
    //If the query was NOT successful
    echo "An error occured";
    echo $db->errno;
}

$db->close();
?>
```

## The Database

As we haven't created our table already we will now do this. Our table is pretty simple: one column for the time, text and name each plus an id column which increases by itself everytime something gets written, while being our index. The easiest way to achieve this is to enter the following MySQL code in the SQL tab of your PhpMyAdmin:

```sql
CREATE TABLE IF NOT EXISTS `chat` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `username` varchar(32) NOT NULL,
  `text` varchar(128) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM  DEFAULT CHARSET=latin1 AUTO_INCREMENT=1
```

Now our chat is able to store the input the user enters on our chat site,
but we don't see it on the chat HTMl site. Time to change this!

## Reading the Database

the fact that PHP only gets executed when the user request the page makes us unable to simply include the data of the database into our chat site. A way to work around this is having a separate file like "read.php" where we will do this.
Later we will load the contents of this site dynamically trough jQuery.

Our "read.php" looks something like this:

```php
<?php
require("./_connect.php");

//connect to db
$db = new mysqli($db_host,$db_user, $db_password, $db_name); 
if ($db->connect_errno) {
	//if the connection to the db failed
    echo "Failed to connect to MySQL: (" . $db->connect_errno . ") " . $db->connect_error;
}


$query="SELECT * FROM chat ORDER BY id ASC";
//execute query
if ($db->real_query($query)) {
	//If the query was successful
	$res = $db->use_result();

    while ($row = $res->fetch_assoc()) {
        $username=$row["username"];
        $text=$row["text"];
        $time=date('G:i', strtotime($row["time"])); //outputs date as # #Hour#:#Minute#
        
        echo "<p>$time | $username: $text</p>\n";
    }
}else{
	//If the query was NOT successful
	echo "An error occured";
	echo $db->errno;
}

$db->close();
?>
```

What we do here is similar to `write.php`, however in this case we want to read from the database, so we use SELECT in our query.
We then loop over every entry and echo a `p` html tag with its contents. Also we need to format the date, we dont need the whole timestamp.

## Conclusion

Done! Now our chat will update itself every 250 milliseconds. Your chat should be working now.

If you have questions regarding this project send me an email,
 or view the [live demo](https://f-rilling.com/projects/rChat/) or [download the source](https://f-rilling.com/user/data/files/f-rilling.com_rChat.7z)
