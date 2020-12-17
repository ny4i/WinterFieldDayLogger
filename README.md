## K6GTE Winter Field Day logger

The logger is written in Python 3, and uses the PyQT5 lib. Qt5 is cross platform so it might work on everything. I myself have only tested it in Linux, YMMV.

The log is stored in an sqlite3 database file 'WFD.db'. If you need to wipe everything and start clean, just delete this file. Just make wfdlogger.py executable and run it within the same folder.

The logger will generate a cabrillo for submission, An ADIF file so you can merge contacts into your normal Log, and a Statistics file with a band mode breakdown.

![Alt text](https://github.com/mbridak/pyqtwfdlogger/raw/master/pics/loggerscreenshot.png)


## Caveats
This is a simple logger ment for single op, it's not usable for clubs.

## Initial Setup
When run for the first time, you will need to set your callsign, class, section, band, mode and power used for the contacts. This can be found at the bottom of the screen.

## Logging
Okay you've made a contact. Enter the call in the call field. As you type it in, it will do a super check partial (see below). Press TAB or SPACE to advance to the next field. Once the call is complete it will do a DUP check (see below). It will try and Autofill the next fields (see below). When entering the section, it will do a section partial check (see below). Press the ENTER key to submit the Q to the log. If it's a busted call or a dup, press the ESC key to clear all inputs and start again.

## Features

#### Radio Polling via rigctld
If you run rigctld a computer connected to the radio, it can be polled for band/mode updates automatically. Click the gear icon at the bottom of the screen to set the IP and port for rigctld. There is a radio icon at the bottom of the logging window to indicate polling status. 

![Alt text](https://github.com/mbridak/pyqtwfdlogger/raw/master/pics/rigctld.png)

#### Cloudlog and QRZ API's
If you use either Cloudlog logging or QRZ lookup you can click the gear icon to enter your credentials. Q's are pushed to CloudLog as soon as they are logged. 


#### Editing an existing contact
Double click a contact in the upper left of the screen to edit or delete it.

![Alt text](https://github.com/mbridak/pyqtwfdlogger/raw/master/pics/editcontact.png)

#### Super Check Partial
If you type more than two characters in the callsign field the program will filter the input through a "Super Check Partial" routine and show you possible matches to known contesting call signs. Is this useful? Doubt it.

![Alt text](https://github.com/mbridak/pyqtwfdlogger/raw/master/pics/scp.png)

#### Section partial check
As you type the section abbreviation you are presented with a list of all possible sections that start with what you have typed.

![Alt text](https://github.com/mbridak/pyqtwfdlogger/raw/master/pics/sectioncheckpartial.png)

#### DUP checking
Once you type a complete callsign and press TAB or SPACE to advance to the next field. The callsign is checked against previous callsigns in your log. It will list any prior contact made showing the band and mode of the contact. If the band and mode are the same as the one you are currently using, the listing will be highlighted, the screen will flash, a bell will sound to alert you that this is a DUP. At this point you and the other OP can argue back and forth about who's wrong. In the end you'll put your big boy pants on and make a decision if you'll enter the call or not.

![Alt text](https://github.com/mbridak/pyqtwfdlogger/raw/master/pics/dupe_check.png)

#### Autofill
If you have worked this person before on another band/mode the program will load the class and section used previously for this call so you will not have to enter this info again.
