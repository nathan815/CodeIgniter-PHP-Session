CodeIgniter-PHP-Session
=======================

This is a library that helps you use PHP's built-in sessions in place of [CodeIgniter](http://codeigniter.com/)'s session class.

**Note:** If you don't use CodeIgniter, some parts of the class probably won't make sense. You can fork this repository and modify it to use with a non-CodeIgniter project if you'd like.

## Why?

CodeIgniter's session class isn't the best because all of the session data is stored in a cookie. Browsers limit the amount of data cookies can store, and they're sent to the server with every request. PHP's native sessions store the data in temporary files on the server and set one small cookie with the session ID in it to identify users.

## Features
- Set multiple variables at once by passing an associative array.
- Flash data: convenient variables that self-destruct after being outputted.
- Forget about checking whether or not a variable is set; the `get()` method returns null if a variable isn't set, instead of generating an error.

## Setup

Download the `php_session.php` file and place it in your `/application/libraries/` directory.

You'll probably want to autoload this library so you don't have to load it in every controller. To do this, open up `/application/config/autoload.php` and add "php_session" to the `$autoload['libraries']` array. If you don't autoload it, you'll have to load it manually in every controller you want to use it in:

```
$this->load->library('php_session');
```

There are some options that you can change as you wish in the `start()` method. Just make sure you know what you're doing.

## Starting the session
To start a session, simply call the `start()` method. If one has already been started, nothing will happen. You may want to call this in a [CodeIgniter hook](http://ellislab.com/codeigniter/user-guide/general/hooks.html) or a [base class](http://philsturgeon.co.uk/blog/2010/02/CodeIgniter-Base-Classes-Keeping-it-DRY). Just like PHP's `session_start()` function, this method must be called before any output is sent to the browser.

```
$this->php_session->start();
```

## Usage

### Setting session variables
To set a session variable, use the `set()` method. You can either pass the key and value as the first and second parameters, or pass an associative array to set multiple values at once.

Let's say you want to set a session variable named "name" with a value of "John Doe". This is how you would do that in a controller:

```
$this->php_session->set('name', 'John Doe');
```

You can also set multiple session variables at once by passing an associative array of keys and values as the first parameter. For example:

```
$data = array('name' => 'John Doe',
              'email' => 'john.doe@gmail.com',
              'timezone_offset' => -5
              );
$this->php_session->set($data);
```

This will set three session variables: name, email, and timezone_offset.


### Getting session variables
To get a session variable, use the `get()` method. This method has one parameter: the key of the variable that you want to get the value of. If you wanted to get the `name` session variable that we set in the above example and set a variable to its value, you would do this:

```
$name = $this->php_session->get('name');
echo "Hello, $name!";
// Output: Hello, John Doe!
```

If it is not set, it will return `null`. No more "undefined index" errors! (In regards to sessions.)

### Deleting session variables
To delete (or unset) a session variable, use the `delete()` method. This method has one parameter: the key of the variable that you want to delete.

```
$this->php_session->delete('name');
```

## Flash data
Flash data is very useful for generating error or informational messages to users that you only want to display once. After you get a flash variable's value, it self-destructs. This concept and its name comes from CodeIgniter's session class.

### Setting flash variables
To set a flash variable, use the `set_flashdata()` method. 

```
$this->php_session->set_flashdata('message', 'You must enter a valid email address!');
```

### Getting flash variables
To get a flash variable, use the `flashdata()` method.

```
$this->php_session->flashdata('message');
// Output: You must enter a valid email address!
```

## Destroying the session
If you wanted to log a user out or just get rid of all of the session data that has been set, simply call the `destroy()` method.

```
$this->php_session->destroy();
```
