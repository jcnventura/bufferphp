Automatically Post Updates To The Buffer API With PHP
=====================================================

Original text from [author's blog post in Little Apps](https://www.little-apps.com/2012/09/automatically-post-updates-buffer-api-php/).

In case you're wondering, Buffer is a social networking app that lets you
automatically post updates to your social profiles.  I am writing this
article to show you how you can be able to post updates using Buffer's API.

The first thing that you will need is (of course) a Buffer account which can
be created by going to [https://www.buffer.com](https://buffer.com).  Once
you're signed up, you will need to create an app by going to
[https://buffer.com/developers/apps/create](https://buffer.com/developers/apps/create).
Enter in all the required fields and for the callback URL just put
"urn:ietf:wg:oauth:2.0:oob".  Once you're done, click "Create Application"
and you should be redirected to a page with an "access token" which you will
need to record for future use.

Since we are using PHP for this app, and as of writing this article there is
only a Ruby library for Buffer's API, I have written this PHP class for
utilizing Buffer's API.  Now let's show you how to use the PHP class so you
can have your updates posted on future dates.

The first thing you're going to need to do is include the PHP class using
the code below.  You will need the access token from the app you created
earlier here.

```
// If not using composer use the following line, replacing [path].
// require_once '[path]/src/BufferPHP.php';

use jcnventura\BufferPHP\BufferPHP;

$buffer = new BufferPHP('YOUR ACCESS TOKEN');
```

Now for the next piece of code, you will need the profile ids so Buffer
knows what social profiles to post the updates to.  To get this, I went to
my dashboard ([https://buffer.com/app](https://buffer.com/app)) and clicked
on the tabs for the social profile I want and the profile id is between
dashboard and pending (ie: http://bufferapp.com/dashboard/YOUR SOCIAL
PROFILE ID/pending).  Once you have figured out which ones you want, put
them in the following code:

```
$data = array('profile_ids' => array());
$data['profile_ids'][] = 'PROFILE ID';
$data['profile_ids'][] = 'PROFILE ID';
```

Next, you will need to figure out what you're going to write in your update.
Once you have, enter it in this code:

```$data['text'] = 'This is an example';```

So lets say you want to add a picture or a link to your update, then this is
where it becomes a little bit more complicated.  For a link it is pretty
simple, you just add in the URL like so:

```$data['media'] = array('link' => 'http://example.com/');```

If you want to add a image to your URL, then you will need three things.
The first is the image (obviously).  The second is a thumbnail of the image.
And third, you will need to know the URLs for the images (which is usually
something like http://example.com/images/whiskers.png and
http://example.com/images/whiskers_thumb.png).  If you have all those three
things then you can add the images to your update like this:

```
$data['media'] = array(
  'picture' => 'http://example.com/images/whiskers.png',
  'thumbnail' => 'http://example.com/images/whiskers_thumb.png',
);
```

(Note: You can only have either a link or a picture with your update, not
both)

Now that you have the information you need, you're going to need to send it
to Buffer's API using the following code:

```$ret = $buffer->post('updates/create', $data);```

Tadaa!  Once you've run this PHP script (and hopefully you didn't get any
errors), the update should be showing on your dashboard in Buffer.  If it is
not, then please raise an issue with what error you're running into and I
will try to assist you.  If you're a "PHP guru" then you can expand this
script so that it posts predefined updates to Buffer every day, week, etc.
using CRON but that is up to you.  The PHP class and example script are
public domain so anyone can use it for whatever purpose.
