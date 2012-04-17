Have you ever wondered about how to keep Drupal's sibling Static HTML Site from corrupting Drupal's pristine file structure? Have you ever wanted to serve these sub-directories from the same path level? 
<h3>I have. And I was able to come up with a solution.</h3>
I racked my brain on this issue for about 2 days and finally figured out a sane approach using good old Apache rewrite conditions and rules.

<h3>Here is our directory structure:</h3>
<pre>z3cka@ubuntre-vm:/var/www$ tree -adL 3 <i>&lsaquo;-- tree command to print directory structure</i>
├── <b>drupalstatic</b> &lsaquo;-- Parent directory for Drupal and Static Siblings
│   ├── <b>drupal</b> &lsaquo;-- Drupal's root
│   └── <b>static</b> &lsaquo;-- Static Sibling's root
│       └── <b>sub-static</b> &lsaquo;-- one of Static Sibling's sub-directories
</pre>
The trick is using apache mod_rewrite rules to traverse into the static portion of the directory structure first, and if nothing is found, pass the uri to Drupal. Meanwhile keeping the directory structure intact all unbeknownst to the user.

This is a huge win for keeping 2 separate trees of files from mixing in with one another for the sake of being organized and easy of revision control.

Currently it is using .htaccess on my local dev server. I puting this into it's own revision control before placing it all in a directive of an apache vhost config.

The site lives here: http://ubuntre-vm/drupalstatic/ ... but must currently be accessed directly with the "drupal-static" hostname via http://drupal-static/ by adding this to your hosts file*:

<pre>131.216.164.67 drupal-static</pre>
<i>*note: you must be on our local network too, sorry :-P</i>

This is because of how the url rewrites are currently set up... but I think this could be changed to work in a subirectory as well (ie: http://ubuntre-vm/drupalstatic/) .... but this is not the priority as of right now.
