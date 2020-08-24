Working with Heroku on Android under TermuxIt gets a bit complicated when installing Heroku’s command line interface in Termux to manage the app on Heroku’s own platform. Luckily I found this interesting blog post that describes all that’s needed to make it happen. With a few tweaks here and there one can perfectly develop Django projects on its Android phone.

After downloading and installing Termux you’ll be greeted by a fairly standard Linux terminal, with basic Linux environment commands like package manager apt-get.
Install the basics:
apt-get install wget
apt-get install tar
apt-get install gzip

mkdir heroku
wget http://cli-assets.heroku.com/heroku-linux-arm.tar.gz -O heroku.tar.gz
tar -xvzf heroku.tar.gz -C heroku
mkdir /data/data/com.termux/files/usr/lib/heroku
mv -r heroku/heroku-cli-linux-x64.tar.gz/* /data/data/com.termux/files/usr/lib/heroku
ln -s /data/data/com.termux/files/usr/lib/heroku/bin/heroku /data/data/com.termux/files/usr/bin/heroku

Heroku will not work as its script is pointing to /usr/bin/env whereas our path on Termux differs.
To direct Heroku to the right path, first install nano (or your own favourite editor), then edit Heroku’s script:

apt-get install nano
cd /data/data/com.termux/files/usr/lib/heroku/bin/
nano heroku

#!/usr/bin/env bash
Change this line to:
#!/data/data/com.termux/files/usr/bin/env bash

Now Heroku is not yet working, one more fix and we’ll be done.
As gibb points out in their tutorial, the tarball file does contain a necessary node.js binary but it does not work in Termux. Therefore we’ll have to install it with the Linux environment and point Heroku to its path.

apt-get install nodejs
cd /data/data/com.termux/files/usr/lib/heroku/bin
mv node node.old
ln -s ../../../bin/node node
heroku — version
heroku-cli/6.15.31–958455a (android-arm64) node-v8.10.0

Now to log into your Heroku account in the CLI:
heroku login
When asked, press any key (expect ‘q’) to open a web browser and login your Heroku account.
