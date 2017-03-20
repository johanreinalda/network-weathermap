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


* added INFOURLTARGET attribute to NODEs and LINKs, to allow opening in a new window
  if set on NODE or LINK  (or their DEFAULT definition),
  this will add the "target=<string>" attribute to the HTML '<a href=...>' generated in the html file
  for the URLINFO attribute.

  USAGE:
	The value of the INFOURLTARGET attribute will be added to the HTML <a> anchor as the 'target' attribute
	INFOURLTARGET _new
	will generate a overlib link such as 
		<area href="INFOURL" target="_new">

* added code to auto-generate INFOURL html links to devices and interfaces on your AKIPS server (via HTML file and Overlib usage).
  This will NOT overwrite an configured INFOURL.
  This assumes that
	-the NODE name is the device name as known to akips (eg "switch1")
	-the LINK name is as generated in the akips.com site script sample(see above); ie. it is of the format "device:interface".

  Global config attribute:

	AKIPSBASEURL https://akips.yourdomain

  NODE or LINK config attribute:
	AKIPS 1

  Config file usage example: (note that the akips output file contains a data line starting with "switch1:TenGigEth1/0 <in-bps> <out-bps>")

===============
	HTMLSTYLE overlib
	HTMLOUTPUTFILE output.html
	AKIPSBASEURL https://akips.yourdomain.com

	NODE DEFAULT
		INFOURLTARGET _new

	LINK DEFAULT
		INFOURLTARGET _new

	NODE switch1
		AKIPS 1

	NODE switch2
		AKIPS 1

	LINK switch1:Ten-GigabitEthernet1/3
		AKIPS 1
		NODES switch1 switch2
===============

	The output.html file will now contain 'hover-over' html links on the "switch1" tch2" nodes, and the link, similar to:
	<area id="NODE:N107:0" href="https://akips.yourdomain.com/dashboard?mode=device;device_list=switch1" target="_new" shape="rect" coords="488,242,513,259" />
	<area id="NODE:N108:0" href="https://akips.yourdomain.com/dashboard?mode=device;device_list=switch2" target="_new" shape="rect" coords="964,226,1037,275" />
	<area id="LINK:L138:3" href="https://akips.yourdomain.com/dashboard?mode=interface;controls=interface;device=switch1;child=Ten-GigabitEthernet1/3" target="_new" shape="rect" coords="409,291,457,310" />





Changes are applied in 
		weathermap
		lib/Weathermap.class.php
		lib/WeatherMap.keywords.inc.php
		lib/WeatherMapNode.class.php
		lib/WeatherMapLink.class.php
		lib/datasources/WeatherMapDataSource_tabfile.php


