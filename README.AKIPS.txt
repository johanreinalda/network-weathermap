Modifications to "network-weathermap" to support AKIPS
Original by Howard Jones, see https://github.com/howardjones/network-weathermap

Based on the 0.98-readconfig-performance branch.

For AKIPS data exporting to use in Weathermap, see
	http://akips.com/showdoc/scripts

Modification include:

* command-line option added to the main "weathermap" script to generate a page refresh other then the
  hardcoded 300 seconds. Mostly so that AKIPS data, which refreshes at 60 second interval, can be used
  to create new weathermaps every minute (or whatever interval you like)
  Usage:
	php ./weathermap --refreshrate 60 --config configs/<your-config>.conf

* modification to handle "link" names in the config file that include the following character:
	: - /
  AKIPS, by defaults, exports the link data as "device:port",
  where the ports can something like eth1/0/2, or Ten-GigabitEthernet1/4/8

  Note this changes the "tab datafile" import module
	lib/datasources/WeatherMapDataSource_tabfile.php

* added INFOURLTARGET attribute to NODEs and LINKs, to allow opening in a new window
  if set on NODE or LINK  (or their DEFAULT definition),
  this will add the "target=<string>" attribute to the HTML 'href' generated
  for the URLINFO attribute.
	Changes in 
		lib\Weathermap.class.php
		lib\WeatherMap.keywords.inc.php
		lib\WeatherMapNode.class.php
		lib\WeatherMapLink.class.php
