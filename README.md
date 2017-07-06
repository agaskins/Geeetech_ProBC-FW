# Marlin 3D Printer Firmware tweaked for Geeetech Pro B/C machines!

<img align="left" src="https://www.geeetech.com/images/s/Geeetech_20170103021407_2.jpg" />

## IMPORTANT NOTICE FOR THIS GEEETECH VERSION OF MARLIN:
I have made some minor changes to the RCBugFix branch of Marlin with two objectives in mind:
- Making it work well with Geeetech Pro B and Pro C models with minimal editing required (although minor, the changes involved can be complicated for many users).
- Making the Pro B/C a more cutting-edge printer by enabling many features not enabled in Marlin or the stock Geeetech FW.

Also, I try to document my changes as best I can, especially if it might not be obvious why I did something, or if it's very different than the defaults. I would say under normal circumstances it's over-commenting, but I want to make this easy for anyone who isn't a coder, or is not at all familiar with Marlin to understand what I do and why. Most of my comments include the words 'Geeetech' and/or 'ProB/C'. It's pretty consistent, but I will improve consistency even more soon.

Oh, and this might also be a good starting point if you use any other Geeetech GT2560 based printer, so I'd be excited to hear from you if this helps with any printers besides the Pro B/C! I'd even be willing to make this more general and call it the 'GT2560 Marlin' or something if it turns out there's more people than benefit from this!

And finally, anyone is welcome to contribute, comment, make requests, etc. I am pretty new to doing real work on Github, so if you're a Github veteran and notice something I'm doing that seems... well, wrong or stupid, by all means let me know! I'm using Github and not just releasing zip files on the Geeetech forum because I want to do this right and be as transparent and community-minded as possible. Let's make these printers better together! :)

## Improvements over the Geeetech firmware:
- Printer Calibration - I believe I've come up with some values that will improve the print accuracy of the printer. Now, to some degree this must be done per printer, but it seems like everyone with a Pro B or Pro C had prints coming out too large with the defaults. I'm almost positive you'll see an improvement if you've never calibrated, but I really need feedback on this to know for sure just how good these numbers are. PRINT A 20mm CUBE BEFORE AND AFTER UPDATING (https://www.thingiverse.com/thing:1278865)! Then send me your measurements, please!
- Velocity & Jerk Improvements - I did a lot of experimenting to get good acceleration/jerk values. What this means is that your prints should be a little faster. Not race-car fast, but more in line with Repetier's 'estimated print time' instead of several hours longer, for example! Geeetech had the jerk values way too high (especially for the Y axis), and conversely the acceleration way too low to compensate for it. I did many test prints that encouraged step skipping and slowly but surely dialed in these values to something much more useful. If you've never tweaked these values you'll be surprised at how well your printer will perform! But speed was not my goal, reliability was and is! I doubt you'll see as many (hopefully not any) prints that are randomly ruined with part the print offset from itself! If you do let me know and send photos.
- LCD Usability- Several issues fixed! Now clockwise goes up, counter-clockwise goes down. One click on the encoder dial equals one change in menu item or value. Basically, it works like it should now and feels intuitive for increasing/decreasing values. I also made it a little easier to use the speed-jogging feature (turning it faster makes the value change larger).
- SD Cards Recognized by Host Software - The SD card is now recognized by RepetierHost (and others). No more 'volume.init' errors. However, to do this it must work at half speed... but really, we weren't missing much, as even full speed would be painfully slow over the serial port emulation. If you want to do much with it a card-reader is still the better option. But it's there... working.
- PID Based Temperature Control - PID is an alternative to the old 'bang-bang' method (set-wait-check-repeat) of settings temps and it gives more stable temps that reach their target faster. This can even affect print quality to a small degree. See the 'PID Auto-Tune' feature below for related details. Also, this link is very informative (but understanding PID to this degree isn't necessary to benefit from PID autotune): http://forum.seemecnc.com/viewtopic.php?t=11440
- More improvements are in there and will be added to this list ASAP!

## Features enabled that are not in the Geeetech firmware:
- Baby-stepping - allows you to finely adjust your Z axis height after the print starts (double press the rotary encoder while on the home screen).
- Linear Advance - This exciting new feature is enabled in this firmware but NOT active by default. This is because it takes some understanding to use, and you will need to make specific settings in your slicer to print with this feature. See: <http://marlinfw.org/docs/features/lin_advance.html>
- Print Progress - When printing from the SD card you can now see a progress bar!
- PID Auto-Tune - This was available in Geeetech's firmware but not used by default. PID Auto-Tune is a simple to use feature that makes your heat-bed and hot-ends work much better and more reliably! They heat up faster and are have less temperature fluctuations! I included my PID values from my stock hot-end and MK2a heatbed, but you really do need to run it yourself! It's very easy to do. First make sure your nozzle isn't sitting on the bed. Then just go to CONTROL > TEMPERATURE > PID AUTOTUNE E1. Then go back to the INFO screen and you'll notice your hotend is heating up (even though the set-point/right-hand value says 0). That means it's working so just be patient and let it do it's thing! It may take a few minutes to complete. If you have a second extruder you can do the same procedure, just choose PID AUTOTUNE E2 instead. Once it's complete go back into CONTROL and select STORE SETTINGS. Not sure why, but for some reason I don't see a way to do PID Autotune for the bed on the LCD menus, but it is possible and easy to do with a few G-Code commands:
    M303 E-1 S75 <-- Change 75 your typical bed temperature
    M304 P228.27 I36.19 D359.98 <-- Once it's done plug the PID values returned in to this command
    M500 <-- Save the values in EEPROM memory
    M503 <-- (optional) Dump the EEPROM values just to verify it's in there
 - Auto Temp - Not to be confused with Auto-tune (above), this feature lets the printer control the temperature (within a specified range) based on print speed. I honestly haven't used this feature too much myself, but it seems interesting and potentially helpful. I'd be interested to know if you're using it with good results on your Geeetech printer! A better description of this feature can be found here: http://reprap.org/wiki/Marlin#AutoTemp
- And much more that I will document here ASAP! Most of these features are enabled in Configuration.h and Configuration_adv.h, if you are curious.
    
 ## DISCLAIMER:
I am just trying to share the work I've done to update the firmware in my own printer. If you try my tweaked Marlin firmware please be aware that it is NOT official Geeetech firmware! It comes with no warranty of any sort and by using it you need to understand that. That said, it's just Marlin firmware! I haven't really done much more than tweak some configuration files a bit, so there's nothing too crazy beyond stock Marlin firmware with a lot of stuff enabled that Geeetech either left out or wasn't in the code base at that time. I've just enabled and pre-configuring a few things to better suit Geeetech Pro B/C 3D printers. As with any new and unofficial firmware on a device like a 3D printer, don't install it and go off to bed with a 12 hour print running before you try it out! There are changes that affect the hot-ends and heat-bed, and while I do my best to enable any and all fail-safe bits in the code, you need to test it out why making sure everything is working well before you consider it production ready with this, or any, unofficial firmware! 

[The rest of this is the stock Marlin readme file...]

<img align="right" src="../../raw/1.1.x/buildroot/share/pixmaps/logo/marlin-250.png" />

## Marlin 1.1

Marlin 1.1 represents an evolutionary leap over Marlin 1.0.2. It is the result of over two years of effort by several volunteers around the world who have paid meticulous and sometimes obsessive attention to every detail. For this release we focused on code quality, performance, stability, and overall user experience. Several new features have also been added, many of which require no extra hardware.

For complete Marlin documentation click over to the [Marlin Homepage <marlinfw.org>](http://marlinfw.org/), where you will find in-depth articles, how-to videos, and tutorials on every aspect of Marlin, as the site develops. For release notes, see the [Releases](https://github.com/MarlinFirmware/Marlin/releases) page.

## Stable Release Branch

This Release branch contains the latest tagged version of Marlin (currently 1.1.2 – May 2017).

Previous releases of Marlin include [1.0.2-2](https://github.com/MarlinFirmware/Marlin/tree/1.0.2-2) (December 2016) and [1.0.1](https://github.com/MarlinFirmware/Marlin/tree/1.0.1) (December 2014). Any version of Marlin prior to 1.0.1 (when we started tagging versions) can be collectively referred to as Marlin 1.0.0.

## Contributing to Marlin

Click on the [Issue Queue](https://github.com/MarlinFirmware/Marlin/issues) and [Pull Requests](https://github.com/MarlinFirmware/Marlin/pulls) links above at any time to see what we're currently working on.

To submit patches and new features for Marlin 1.1 check out the [bugfix-1.1.x](https://github.com/MarlinFirmware/Marlin/tree/bugfix-1.1.x) branch, add your commits, and submit a Pull Request back to the `bugfix-1.1.x` branch. Periodically that branch will form the basis for the next minor release.

Note that our "bugfix" branch will always contain the latest patches to the current release version. These patches may not be widely tested. As always, when using "nightly" builds of Marlin, proceed with full caution.

## Current Status: In Development

Marlin development has reached an important milestone with its first stable release in over 2 years. During this period we focused on cleaning up the code and making it more modern, consistent, readable, and sensible.

## Future Development

Marlin 1.1 is the last "flat" version of Marlin!

Arduino IDE now has support for folder hierarchies, so Marlin 1.2 will have a [hierarchical file structure](https://github.com/MarlinFirmware/Marlin/tree/breakup-marlin-idea). Marlin's newly reorganized code will be easier to work with and form a stronger starting-point as we get into [32-bit CPU support](https://github.com/MarlinFirmware/Marlin/tree/32-Bit-RCBugFix-new) and the Hardware Access Layer (HAL).

[![Coverity Scan Build Status](https://scan.coverity.com/projects/2224/badge.svg)](https://scan.coverity.com/projects/2224)
[![Travis Build Status](https://travis-ci.org/MarlinFirmware/Marlin.svg)](https://travis-ci.org/MarlinFirmware/Marlin)

## Marlin Resources

- [Marlin Home Page](http://marlinfw.org/) - The Marlin Documentation Project. Join us!
- [RepRap.org Wiki Page](http://reprap.org/wiki/Marlin) - An overview of Marlin and its role in RepRap.
- [Marlin Firmware Forum](http://forums.reprap.org/list.php?415) - Find help with configuration, get up and running.
- [@MarlinFirmware](https://twitter.com/MarlinFirmware) on Twitter - Follow for news, release alerts, and tips & tricks. (Maintained by [@thinkyhead](https://github.com/thinkyhead).)

## Credits

The current Marlin dev team consists of:
 - Roxanne Neufeld [[@Roxy-3D](https://github.com/Roxy-3D)] - English
 - Scott Lahteine [[@thinkyhead](https://github.com/thinkyhead)] - English
 - Bob Kuhn [[@Bob-the-Kuhn](https://github.com/Bob-the-Kuhn)] - English
 - Andreas Hardtung [[@AnHardt](https://github.com/AnHardt)] - Deutsch, English
 - Nico Tonnhofer [[@Wurstnase](https://github.com/Wurstnase)] - Deutsch, English
 - Jochen Groppe [[@CONSULitAS](https://github.com/CONSULitAS)] - Deutsch, English
 - João Brazio [[@jbrazio](https://github.com/jbrazio)] - Portuguese, English
 - Bo Hermannsen [[@boelle](https://github.com/boelle)] - Danish, English
 - Bob Cousins [[@bobc](https://github.com/bobc)] - English
 - [[@maverikou](https://github.com/maverikou)]
 - Chris Palmer [[@nophead](https://github.com/nophead)]
 - [[@paclema](https://github.com/paclema)]
 - Erik van der Zalm [[@ErikZalm](https://github.com/ErikZalm)]
 - David Braam [[@daid](https://github.com/daid)]
 - Bernhard Kubicek [[@bkubicek](https://github.com/bkubicek)]

More features have been added by:
 - Alberto Cotronei [[@MagoKimbra](https://github.com/MagoKimbra)] - English, Italian
 - Thomas Moore [[@tcm0116](https://github.com/tcm0116)]
 - Ernesto Martinez [[@emartinez167](https://github.com/emartinez167)]
 - Petr Zahradnik [[@clexpert](https://github.com/clexpert)]
 - Kai [[@Kaibob2](https://github.com/Kaibob2)]
 - Edward Patel [[@epatel](https://github.com/epatel)]
 - F. Malpartida [[@fmalpartida](https://github.com/fmalpartida)] - English, Spanish
 - [[@esenapaj](https://github.com/esenapaj)] - English, Japanese
 - [[@benlye](https://github.com/benlye)]
 - [[@Tannoo](https://github.com/Tannoo)]
 - [[@teemuatlut](https://github.com/teemuatlut)]
 - [[@bgort](https://github.com/bgort)]
 - Luc Van Daele[[@LVD-AC](https://github.com/LVD-AC)] - Dutch, French, English
 - [[@paulusjacobus](https://github.com/paulusjacobus)]
 - ...and many others

## License

Marlin is published under the [GPL license](https://github.com/COPYING.md) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.

[![Flattr this git repo](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software)
