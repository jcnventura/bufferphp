Automatically Post Updates To The Buffer API With PHP
=====================================================

Original text from [author's blog post in Little
Apps](https://www.little-apps.com/2012/09/automatically-post-updates-buffer-api-php/).

In case your wondering, Buffer is a social networking app that lets you
automatically (well sort of) post updates to your social profiles.  The
reason that I am saying that its not totally automatic is because you can
only have updates posted for the current day and not lets say a week (or
month) from now.  I am writing this article to show you how you can be able
to post updates on whatever day you want using Buffer’s API.

The first thing that you will need is (of course) a Buffer account which can
be created by going to [http://www.bufferapp.com](https://buffer.com).  Once
your signed up, you will need to create an app by going to
[http://bufferapp.com/developers/apps/create](https://buffer.com/developers/apps/create). 
Enter in all the required fields and for the callback URL I just put
“urn:ietf:wg:oauth:2.0:oob”.  Once you’re done, click “Create Application”
and you should be redirected to a page with an “access token” which you will
need to record for future use.

Since we are using PHP for this app and (as of writing this article) there
is only a Ruby library for Buffer’s API.  So I have wrote a PHP class for
utilizing Buffer’s API and it can be downloaded by clicking the link below. 
Now lets show you how to use the PHP class so you can have your updates
posted on future dates.

The first thing your going to need to do is include the PHP class using the
code below.  You will need the access token from the app you created earlier
here.

```

require_once ‘class.bufferapp.php’;
$buffer = new BufferPHP(‘YOUR ACCESS TOKEN’);
```

Now for the next piece of code, you will need the profile ids so Buffer
knows what social profiles to post the updates to.  To get this, I went to
my dashboard ([http://bufferapp.com/dashboard](https://buffer.com/app)) and
clicked on the tabs for the social profile I want and the profile id is
between dashboard and pending (ie: http://bufferapp.com/dashboard/YOUR
SOCIAL PROFILE ID/pending).  Once you have figured out which ones you want,
put them in the following code:

```

$data = array(‘profile_ids’ => array());
$data[‘profile_ids’][] = ‘PROFILE ID’;
$data[‘profile_ids’][] = ‘PROFILE ID’;
```

Next, you will need to figure out what your going to be writing in your
update.  Once you have, enter it in this code:

```$data[‘text’] = ‘This is an example’;```

So lets say you want to add a picture or a link to your update, then this is
where it becomes a little bit more complicated.  For a link it is pretty
simple, you just add in the URL like so:

```$data[‘media’] = array(‘link’ => ‘http://example.com/’);```

If you want to add a image to your URL, then you will need three things. 
The first is the image (obviously).  The second is a thumbnail of the image. 
And third, you will need to know the URLs for the images (which is usually
something like http://example.com/images/whiskers.png and
http://example.com/images/whiskers_thumb.png).  If you have all those three
things then you can add the images to your update like this:

```
$data[‘media’] = array(‘picture’ => ‘http://example.com/images/whiskers.png’, ‘thumbnail’ => ‘http://example.com/images/whiskers_thumb.png’);
```

(Note: You can only have either a link or picture with your update, not
both)

Now that you have the information you need, your going to need to send it to
Buffer’s API using the following code:

```$ret = $buffer->post(‘updates/create’, $data);```

Tadaa!  Once you’ve run this PHP script (and hopefully you didn’t get any
errors), the update should be showing on your dashboard in Buffer.  If its
not, then please post a comment below with what error your running into and
I will try to assist you.  If you’re a “PHP guru” then you can expand this
script so that it posts predefined updates to Buffer every day, week, etc
using CRON but that is up to you.  The PHP class and example script are
public domain so anyone can use it for whatever purpose.
