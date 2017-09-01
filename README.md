# mediawiki-parsoid-visualeditor-docker
A simple Dockerfile to download everything you need for setting up a Mediawiki with visual editing.

Please note that this will not work without some basic configuration.  You will need to start the apache instance, and go through the configuration wizard (connect to your apache instance through a web browser to see that), and point/link this to a database.

Afterwards you will need to append to the LocalSettings.php file that is generated to enable Parsoid and VisualEditor with the following (changing the url/ip/port values where necessary):

(reference https://www.mediawiki.org/wiki/Extension:VisualEditor for further VisualEditor information)
```
wfLoadExtension( 'VisualEditor' );

// Enable by default for everybody
$wgDefaultUserOptions['visualeditor-enable'] = 1;

// Optional: Set VisualEditor as the default for anonymous users
// otherwise they will have to switch to VE
// $wgDefaultUserOptions['visualeditor-editor'] = "visualeditor";

// Don't allow users to disable it
$wgHiddenPrefs[] = 'visualeditor-enable';

// OPTIONAL: Enable VisualEditor's experimental code features
#$wgDefaultUserOptions['visualeditor-enable-experimental'] = 1;

$wgVirtualRestConfig['modules']['parsoid'] = array(
	// URL to the Parsoid instance
	// Use port 8142 if you use the Debian package
	'url' => 'http://localhost:8000',
	// Parsoid "domain", see below (optional)
	'domain' => 'localhost',
	// Parsoid "prefix", see below (optional)
	'prefix' => 'localhost'
);
```
Next, you'll need to edit the config.yaml located in the /usr/lib/parsoid directory and change the values as necessary to point towards the correct VisualEditor/Mediawiki url (you'll usually need to change the "<baseurl>/w/api.php" part of the parsoid url to just "<baseurl>/api.php" subbing in <baseurl> for whatever is in your LocalSettings.php

Finally you'll need to enable Parsoid to listen by navigating to /usr/lib/parsoid and running:
```
node /bin/server.js
```
(reference: https://www.mediawiki.org/wiki/Parsoid/Developer_Setup for further parsoid information)


So finally, to state again, this should download all the requirements you'd need to set up a simple Mediawiki with visual editing, it does not come preconfigured as every environment is different.  Feel free to extend as you please.

This Dockerfile is referencing (https://github.com/wikimedia/mediawiki-docker/blob/master/stable/Dockerfile) Please look to there for other options and builds.

Also if you're looking for a full hands-off environment type of build check out (https://github.com/wikimedia/mediawiki-containers) as that might be more what you're looking for,

Goodluck!

