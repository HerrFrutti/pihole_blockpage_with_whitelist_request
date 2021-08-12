# Pi-hole: custom blockpage with whitelist request button
Custom Pi-hole blockpage with a button to send a request (via mail) to whitelist the blocked domain.

|Simple:|Materialize:| 
|:-------------------------:|:-------------------------:|
|<img src="https://user-images.githubusercontent.com/6975702/128698855-03cb2097-2542-41f1-b51a-96543a31ec67.png">|<img src="https://user-images.githubusercontent.com/6975702/128746098-681651b2-b162-4a13-be72-cbb9caf6a369.png">|



This project is inspired by this video: https://www.youtube.com/watch?v=pHjCO1zxpMA which explains how to set up a custom blockpage for pi-hole.
After setting up a custom blockpage, I've thaught it would be cool to give my family the opportunity to reqeust whitelisting a domain they can't reach.

## HTTPS isn't supported! (https://discourse.pi-hole.net/t/customize-block-page/4693/42)

## Instructions:
Add the line BLOCKINGMODE=IP to the pihole-FTL.conf, without that pi-hole will not load a blocking page
```
nano /etc/pihole/pihole-FTL.conf
BLOCKINGMODE=IP
```
Define a custom block page inside of the lighttpd.conf, instead of using the default blockpage
change the server.error-handler-404 value to "/pihole/your_block_page.php"

```
nano /etc/lighttpd/lighttpd.conf
server.error-handler-404    = "/pihole/simple-block.php"
```
Restart the lighttpd service
```
/etc/init.d/lighttpd restart
```
Create your custom blockpage
```
nano /var/www/html/pihole/simple-block.php
```
Code your own blockpage or use my example "simple_block.php" or "block_materialize.php" blockpage, to use the whitelist request feature, change the "your@mail.xyz" to your email.
simple_block.php:
```
<html>
  <head>
    <title>website blocked by pi-hole!</title>
  </head>
    <body>
      <p style="text-align: center;"><img style="display: block; margin-left: auto; margin-right: auto;" src="https://upload.wikimedia.org/wikipedia/commons/0/00/Pi-hole_Logo.png" alt="PiHoleLogo" width="256" height="243" /><span style="font-size: xx-large;">WEBSITE BLOCKED!</span></p>
        <div style="text-align: center;">
          <form action="mailto:your@mail.xyz?subject=Pi-hole: Whitelist Request! &body=<?php echo $_SERVER['SERVER_NAME'];?>" method="post">
	    <input type="submit" value="request whitelist" />
	    <input type="hidden" />
          </form>
	</div>
    </body>
</html>

```
