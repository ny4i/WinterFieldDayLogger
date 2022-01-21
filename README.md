# K6GTE Winter Field Day logger (PyQt5)

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)  [![Python: 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)  [![Made With:PyQt5](https://img.shields.io/badge/Made%20with-PyQt5-red)](https://pypi.org/project/PyQt5/)

[Winter Field Day](https://www.winterfieldday.com/) is a once a year 24hr emergency preparidness event for radio amateurs (Hams). During the event, we try and make as many radio contacts with other Hams in a 24 hour period. Bonus points are awarded for operating outside or using alternate power sources, such as battery/solar/wind. You can find out more about amateur radio by visiting the [ARRL](https://www.arrl.org/).

The logger is written in Python 3, and uses the PyQT5 lib. Qt5 is cross platform so it might work on everything. I have tested it on Linux, Rasperry Pi OS and Windows 10. This code is based off of a logger I had done earlier using Python and the curses library wich can be found [here](https://github.com/mbridak/wfd_py_logger) and one written for ARRL Field Day [here](https://github.com/mbridak/FieldDayLogger).

The log is stored in an sqlite3 database file 'WFD.db'. If you need to wipe everything and start clean, just delete this file and re-run wfdlogger

The logger will generate a cabrillo for submission, An ADIF file so you can merge contacts into your normal Log, and a Statistics file with a band mode breakdown.

![Snapshot of main screen](pics/loggerscreenshot.png)

## Caveats

This is a simple logger ment for single op, it's not usable for clubs.
WFD only has a generic digital mode designator 'DI', which gets exported to the cabrillo file. But ADIF and CloudLog needed something else, So I Chose RTTY. Feel free to change it to what ever you will use. Just search for the two places in the code 'RTTY' is used and Bob's your dads brother.

## Changes since 22.1.14 Don't you test this stuff?

* ReImplimented TAB focus behavior to deselect text on field focus.
* Edit cursor position in field no longer resets to the end after validation. This reduces the amount of swearing emitted by the operator.
* Adding quick commands to callsign entry field. Enter the command then press the spacebar. Limited list so far:


`.Q` Quits.
 
`.B#` set current band. So `.B20` changes you to 20M.

`.P#` set power level. `.P10` sets your logged power to 10 watts.
 
`.MCW .MPH .MDI` sets you to CW Phone and Digital respectively.
 
`.E#` Edit contact. `.E10` would edit contact #010.
 

## Running the binary

In the [releases](https://github.com/mbridak/WinterFieldDayLogger/releases) you will find binaries for Linux, Windows and Raspberry Pi.

## Running from source

Install Python 3, then two required libs via pip.

`pip install -r requirements.txt`

Or if you're the Ubuntu/Debian type you can:

`sudo apt install python3-pyqt5 python3-requests`

Just make wfdlogger.py executable and run it within the same folder, or type:

`python3 wfdlogger.py`

### Requirements for bandmap
The [bandmap](./bandmap.md "Band Map Docs") needs a few more python libraries.

pip or pip3 install rich bs4 lxml 

## Building your own binary.

Install pyinstaller.

`python3 -m pip3 install pyinstaller`

Build the binary.

For Linux and Raspberry PI:

`pyinstaller -F linux.spec`

For Windows:

`pyinstaller -F windows.spec`

You will find the binary in the newly created dist directory.

## What to do first
On first run, there will be a dialog box asking you for your call class and section. if you need to change this later, the entry fields can be found at the bottom of the screen.

![Snapshot focused on bottom of window](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/yourstuff.png)

## Logging

Okay you've made a contact. Enter the call in the call field. As you type it in, it will do a super check partial (see below). Press TAB or SPACE to advance to the next field. Once the call is complete it will do a DUP check (see below).
 It will try and Autofill the next fields (see below). When entering the section, it will do a section partial check (see below). Press the ENTER key to submit the Q to the log. If it's a busted call or a dup, press the ESC key to clear all inputs and start again.

# Features

## Radio Polling via flrig

If you run flrig on a computer connected to the radio, it can be polled for band/mode updates automatically. Click the gear icon at the bottom of the screen to set the IP and port for flrig. There is a radio icon at the bottom of the logging window to indicate polling status.

![Snapshot of settings dialog](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/loggerSettingsDialog.png)

## Cloudlog, QRZ, HamDB useage

If you use either Cloudlog logging or QRZ/HamDB lookup you can click the gear icon to enter your credentials. Q's are pushed to CloudLog as soon as they are logged.

The QRZ/HamDB lookup is only used to get the Op name and gridsquare for the call. Mainly because when a Q is pushed to CloudLog it will not show as a pin on the map unless it has a gridsquare. So this is a scratch my own itch feature. HAMDB.org is used by default since it's free. If both are checked it will it will use QRZ then fallback to HAMDB.

## XPlanet marker file

If you use QRZ/HamdDB lookups you can also generate an [XPlanet](http://xplanet.sourceforge.net/) markerfile which will show little pips on the map as contacts are logged.

![Snapshot of xplanet window](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/xplanet.png)

The above launched with an example command:

```bash
xplanet -body earth -window -longitude -117 -latitude 38 -config Default -projection azmithal -radius 200 -wait 5
```

## Editing an existing contact

Double click a contact in the upper left of the screen to edit or delete it.

![Snapshot of edit qso dialog](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/editqso.png)

## Super Check Partial

If you type more than two characters in the callsign field the program will filter the input through a "Super Check Partial" routine and show you possible matches to known contesting call signs. Is this useful? Doubt it.

![Snapshot of main screen focused on super check partial output](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/scp.png)

## Section partial check

As you type the section abbreviation you are presented with a list of all possible sections that start with what you have typed.

![Snapshot of main screen focused on section check partial output](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/sectioncheck.png)

## DUP checking

Once you type a complete callsign and press TAB or SPACE to advance to the next field. The callsign is checked against previous callsigns in your log. It will list any prior contact made showing the band and mode of the contact. If the band and mode are the same as the one you are currently using, the listing will be highlighted, the screen will flash, a bell will sound to alert you that this is a DUP. At this point you and the other OP can argue back and forth about who's wrong. In the end you'll put your big boy pants on and make a decision if you'll enter the call or not.

![Snapshot of main screen focused on dup checking output](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/dupe.png)

## Autofill

If you have worked this person before on another band/mode the program will load the class and section used previously for this call so you will not have to enter this info again.

## CW Macros

The program will check in the current working directory for a file called `cwmacros.txt` it will parse the file and configure the new row of 12 buttons along the bottom half of the window. The macros can be activated by either pressing the corresponding function key, or by directly clicking on the button. You can check the file to glean it's structure, but it's pretty straight forward. Each line has 3 sections separated by the pipe `|` character. Here's an example line.

`F2|Run Exch|{HISCALL} {MYCLASS} {MYSECT}`

The first field is the function key to program. The second is the name of the button. And lastly the third is the text you would like to send.

A limited set substitution macros are offered.

`{MYCALL}`
`{HISCALL}`
`{MYCLASS}`
`{MYSECT}`

These are pulled straight from the onscreen input fields. Combined with normal text this should have you covered for most of your exchange needs.

# When the event is over

After the big weekend, once you've swept up all the broken beer bottles and wiped the BBQ sauce off your chin, go ahead and click the Generate Logs button.

![Snapshot of main screen focused on generate logs button](https://github.com/mbridak/WinterFieldDayLogger/raw/main/pics/genlog.png)

This will generate the following:

An ADIF log 'WFD.adi'.

A Cabrillo log 'Yourcall.log'. Which you edit to fill in your address etc. If your not using Windows, you must ensure whatever editor you use uses CR/LF line endings. Cause whatever they use at the Winter Field Day society will choke with out them. To be safe you might want to run it through 'unix2dos' before submitting it.

A 'Statistics.txt' file which breaks down your band mode usage. Each unique band/mode combo is a multiplier.

## The Bandmap program
See [here](./bandmap.md "Band Map Docs")
