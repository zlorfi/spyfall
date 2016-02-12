# Meteor Spyfall

Spyfall on your mobile device!

This is the code that currently runs [http://spyfall.meteor.com](http://spyfall.meteor.com). It's a simple site I've built as a learning project, so definitely don't use it as an example of Meteor best practices. Pull requests welcome!

## Disclaimer

[Spyfall](http://international.hobbyworld.ru/catalog/25-spyfall/) is a party game designed by Alexandr Ushan and published by [Hobby World](http://international.hobbyworld.ru/). This is an unofficial fan project designed to complement the physical game, and is not endorsed in any way by the designer or publisher.

## Translations

If you'd like to see the site in your own language, there are two ways to contribute a translation - Transifex or pull request.

### Transifex

Transifex is an easy to use translation service. It has a nice interface and is great for non-programmers. Simply go to the following link:

[https://www.transifex.com/projects/p/spyfall/](https://www.transifex.com/projects/p/spyfall/)

Then click the big "Help Translate Spyfall" button. After creating a user account you should be able to start working on a translation right away. Once you're done, I'll be sent an email and will add your translation to the live site as soon as possible.

### Pull request

If you're comfortable with Github and JSON files, feel free to simply create a new file in the `spyfall/i18n` directory, using `en.i18n.json` as a base.

### Notes for translators

Please make sure that the translations you provide are as close as possible to the original meaning - try not to alter roles or locations, as I'd like the game to still be playable when different players in the same game are using different languages.

## Running your own instance with custom locations

[Install meteor](https://www.meteor.com/install)

Clone the repository:

	git clone https://github.com/evanbrumley/spyfall.git ./spyfall

Enter the spyfall directory:

	cd spyfall/spyfall

Edit the locations file as required:

	nano lib/locations.js

Run the meteor server to test locally:

	meteor --settings settings/example.json

Make a production settings file:

	cp settings/example.json settings/production.json
	nano settings/production.json  # Edit as required

Deploy to meteor:

	meteor deploy myurl.meteor.com --settings settings/production.json
	
## Deploy to your own server

Install [mongodb](https://github.com/mongodb/mongo)

Install Apache 2.4 with [Phusion Passenger 5.x](http://https://github.com/phusion/passenger) and add an new spyfall.conf Virtual Host file:

	<VirtualHost *:80>
	  ServerName spyfall.YOURHOST.com
	  DocumentRoot /var/www/spyfall/public
	  PassengerStickySessions On
	  SetEnv MONGO_URL mongodb://localhost:27017/spyfall
	  SetEnv ROOT_URL http://spyfall.YOURHOST.com
	  SetEnv METEOR_SETTINGS '{ "public": { "url": "/" } }'

	  # Set these ONLY if your app is a Meteor bundle!
	  PassengerAppType node
	  PassengerStartupFile main.js
	  PassengerAppRoot /var/www/spyfall

	  # Possible values include: debug, info, notice, warn, error, crit,
	  # alert, emerg.
	  LogLevel warn

	  CustomLog ${APACHE_LOG_DIR}/spyfall_YOURHOST_com_access.log combined
	  ErrorLog ${APACHE_LOG_DIR}/spyfall_YOURHOST_com_error.log

	</VirtualHost>

Clone this repo and bundle your app:

	meteor bundle spyfall.tar.gz
	
Copy the bundle over to your server and unpack it:

	tar xvzf spyfall.tar.gz /var/www/spyfall
	
Install all necessary npm packages:

	npm install fibers semver source-map-support underscore
	
Create the necessary folders for Passenger:

	cd /var/www/spyfall && mkdir tmp public
	
Restart Apache:

	sudo service apache2 restart

## Links

[BoardGameGeek Discussion Thread](http://www.boardgamegeek.com/thread/1279239/app/page/1)
