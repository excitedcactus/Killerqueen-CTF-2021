# Just Not My Type

> Tags: web, php  
> Points: 200  
> Solves: 399

## Challenge Description

![image](https://user-images.githubusercontent.com/56489087/139526386-9fd17ce3-5cfe-4aa4-b657-c2e5d60e1c17.png)

## Target Page

![image](https://user-images.githubusercontent.com/56489087/139526418-1c888c6a-13d9-4007-93e1-bc728ec2a781.png)

Upon quick analysis, we can see the we are sending POST requests with URL encoded data. Only one page is loaded so our scope and target are pretty clear.

## Type.php

We are provided a PHP file.


```php
<h1>I just don't think we're compatible</h1>

<?php
$FLAG = "shhhh you don't get to see this locally";

if ($_SERVER['REQUEST_METHOD'] === 'POST') 
{
    $password = $_POST["password"];
    if (strcasecmp($password, $FLAG) == 0) 
    {
        echo $FLAG;
    } 
    else 
    {
        echo "That's the wrong password!";
    }
}
?>

<form method="POST">
    Password
    <input type="password" name="password">
    <input type="submit">
</form>
```

The file uses a boolean to determine if the flag is echo'd. 

Googling the strcasecmp function shows us that it can return 0, 1, or -1:

![image](https://user-images.githubusercontent.com/56489087/139526730-73025c73-67c8-4b8c-99c8-03994c361772.png)

Based on the title of the challenge and the comparision that is being used for the strcasecmp function and 0, we can determine that this may be vulnerable to type juggling.

## Comparison differences

![image](https://user-images.githubusercontent.com/56489087/139526805-0f6de70c-05d8-44d3-a586-d887964bb99b.png)

If they were using a strict comparison, 0 === 0 is the only comparison that could evaluate to true.

![image](https://user-images.githubusercontent.com/56489087/139526825-20e4a9a6-4f6c-4ae9-acb6-7986ac43a531.png)

Because they are using a loose comparison, there are more comparison of 0 that could evaluate to true.

## Exploit

After testing it locally, I found out that if we use an empty array(), the strcasecmp function will spit out NULL.

> strcmp(array(), "thePassword") -> NULL

We can send a POST request with an empty array to PHP like this:

> password[]=

We can then modify the request in Burp and forward it.

![image](https://user-images.githubusercontent.com/56489087/139527083-61a4c27b-d424-4892-bf66-2615e9651a8b.png)

Because of type juggling a loose comparison, NULL == 0. We have achieved an auth bypass.

![image](https://user-images.githubusercontent.com/56489087/139527179-f5824df1-4fb4-4f96-85b6-ea5bf49b67c0.png)

