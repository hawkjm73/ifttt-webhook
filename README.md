ifttt-webhook
=============

A webhook middleware for the ifttt.com service for use with AutoRemote

#How To Use
1. Host these files on a server which supports php.
1. Change your ifttt.com wordpress server to your host.
2. You can use any username/password combination you want. ifttt will accept the authentication irrespective of what details you enter here.
3. Create a recipe in ifttt which would post to your "wordpress channel". In the "Tags" field, use the AutoRemote url: http://autoremotejoaomgcd.appspot.com/sendmessage
4. The "Title" field must be your AutoRemote key. You can retrieve this from your browser by entering your short url from the AutoRemote app and letting it redirect.

![Connecting to ifttt-webhook](http://i.imgur.com/RA0Jb.png "You can type in any username/password you want")

Any username/password combination will be accepted. A blank password is considered valid, but ifttt invalidates a blank username.

![Screenshot of a channel](http://i.imgur.com/5FaU1.png "Sample Channel for use as a webhook")

The post status should ideally be set to "Publish Immediately", but anything else should work as well.

#How It Works
ifttt uses wordpress-xmlrpc to communicate with the wordpress blog. We present a fake-xmlrpc interface on the webadress, which causes ifttt to be fooled into thinking of this as a genuine wordpress blog. The only action that ifttt allows for wordpress are posting, which are instead used for powering webhooks. All the other fields (title, description, categories) along with the username/password credentials are passed along by the webhook. Do not use the "Create a photo post" action for wordpress, as ifttt manually adds a `<img>` tag in the description pointing to what url you pass. Its better to pass the url in clear instead (using body/category/title fields).

#Why
There has been a lot of [call](http://blog.jazzychad.net/2012/08/05/ifttt-needs-webhooks-stat.html) for a ifttt-webhook. I had asked about it pretty early on, but ifttt has yet to create such a channel. It was fun to build and will allow me to hookup ifttt with things like [partychat][pc], [github](gh) and many other awesome services for which ifttt is yet to build a channel. You can build a postmarkapp.com like email-to-webhook service using ifttt alone. Wordpress seems to be the only channel on ifttt that supports custom domains, and hence can be used as a middleware. The ifttt-webhook also propogates errors on connecting to the webhook back to ifttt. This means that an Internal Server Error will be recognized as an error by ifttt, and reported as such. You won't be getting any debug information from this side (ifttt doesn't show that in logs), so debug on your webhook side by proper logging.

#Payload
The body field of the WordPress post will be passed on as the message to be sent via AutoRemote

#Licence
Licenced under GPL. Some portions of the code are from wordpress itself. You should probably host this on your own server.

#Custom Use
Just clone the git repo to some place, and use that as the wordpress installation location in ifttt.com channel settings.

[pc]: http://partychat-hooks.appspot.com/ "Partychat Hooks"
[gh]: https://help.github.com/articles/post-receive-hooks/ "Github Post receive hooks"
