
# Guide2Go BETA
Guide2Go is written in Go and creates an XMLTV file from the Schedules Direct JSON API.  
**Configuration files from version 1.0.6 or earlier are not compatible!**

#### Features
- Cache function to download only new EPG data
- No database is required
- Update EPG with CLI command for using your own scripts

#### Requirement
- [Schedules Direct](https://www.schedulesdirect.org/ "Schedules Direct") Account
- [Go](https://golang.org/ "Golang") to build the binary
- Computer with 1-2 GB memory

## Build binary 
The following command must be executed with the terminal / command prompt inside the source code folder.  
Linux, Unix, OSX:
```
go build -o guide2go
```
Windows:

```
go build -o guide2go.exe
```
Switch -o creates the binary *guide2go* in the current folder.  

## Instructions

### Show CLI parameter:  
```guide2go -h```

```
-config string
    = Get data from Schedules Direct with configuration file. [filename.yaml]
-configure string
    = Create or modify the configuration file. [filename.yaml]
-h  : Show help
```

### Create a config file:

```guide2go -configure MY_CONFIG_FILE.yaml```  
If the configuration file does not exist, a YAML configuration file is created. 

**Configuration file from version 1.0.6 or earlier is not compatible.**  
##### Terminal Output:
```
2020/05/07 12:00:00 [G2G  ] Version: 1.1.0
2020/05/07 12:00:00 [URL  ] https://json.schedulesdirect.org/20141201/token
2020/05/07 12:00:01 [SD   ] Login...OK

2020/05/07 12:00:01 [URL  ] https://json.schedulesdirect.org/20141201/status
2020/05/07 12:00:01 [SD   ] Account Expires: 2020-11-2 14:08:12 +0000 UTC
2020/05/07 12:00:01 [SD   ] Lineups: 4 / 4
2020/05/07 12:00:01 [SD   ] System Status: Online [No known issues.]
2020/05/07 12:00:01 [G2G  ] Channels: 214

Configuration [MY_CONFIG_FILE.yaml]
-----------------------------
 1. Schedules Direct Account
 2. Add Lineup
 3. Remove Lineup
 4. Manage Channels
 5. Create XMLTV File [MY_CONFIG_FILE.xml]
 0. Exit

```

**Follow the instructions in the terminal**

1. Schedules Direct Account:  
Manage Schedules Direct credentials.  

2. Add Lineup:  
Add Lineup into the Schedules Direct account.  

3. Remove Lineup:  
Remove Lineup from the Schedules Direct account.  

4. Manage Channels:  
Selection of the channels to be used.
All selected channels are merged into one XML file when the XMLTV file is created.
When using all channels from all lineups it is recommended to create a separate Guide2Go configuration file for each lineup.  
**Example:**  
Lineup 1:
```
guide2go -configure Config_Lineup_1.yaml
```
Lineup 2:
```
guide2go -configure Config_Lineup_2.yaml
```

5. Create XMLTV File [MY_CONFIG_FILE.xml]:  
Creates the XMLTV file with the selected channels.  

#### The YAML configuration file can be customize with an editor.:

```
Account:
    Username: SCHEDULES_DIRECT_USERNAME
    Password: SCHEDULES_DIRECT_HASHED_PASSWORD
Files:
    Cache: MY_CONFIG_FILE.json
    XMLTV: MY_CONFIG_FILE.xml
Options:
    Poster Aspect: all
    Schedule Days: 7
    Subtitle into Description: false
Station:
  - Name: Fox Sports 1 HD
    ID: "82547"
    Lineup: USA-DITV-DEFAULT
  - Name: Fox Sports 2 HD
    ID: "59305"
    Lineup: USA-DITV-DEFAULT
```

**- Account: (Don't change)**  
Schedules Direct Account data, do not change them in the configuration file.  

**- Flies: (Can be customized)**  
```
Cache: /Path/to/cach/file  
XMLTV: /Path/to/XMLTV/file  
```

**- Options: (Can be customized)**  
```
Poster Aspect: all
```
- all:  All available Image ratios are used.  
- 2x3:  Only uses the Image / Poster in 2x3 ratio. (Used by Plex)  
- 4x3:  Only uses the Image / Poster in 4x3 ratio.  
- 16x9: Only uses the Image / Poster in 16x9 ratio.  

**Some clients only use one image, even if there are several in the XMLTV file.**  

```
Schedule Days: 7
```
EPG data for the specified days. Schedules Direct has EPG data for the next 12-14 days  

```
Subtitle into Description: false
```
Some clients only display the description and ignore the subtitle tag from the XMLTV file.  

true: If there is a subtitle, it will be added to the description.  

```XML
<?xml version="1.0" encoding="UTF-8"?>
<programme channel="guide2go.67203.schedulesdirect.org" start="20200509134500 +0000" stop="20200509141000 +0000">
   <title lang="de">Two and a Half Men</title>
   <sub-title lang="de">Ich arbeite für Caligula</sub-title>
   <desc lang="de">[Ich arbeite für Caligula]
Alan zieht aus, da seine Freundin Kandi und er in Las Vegas eine Million Dollar gewonnen haben. Charlie kehrt zu seinem ausschweifenden Lebensstil zurück und schmeißt wilde Partys, die bald ausarten. Doch dann steht Alan plötzlich wieder vor der Tür.</desc>
   <category lang="en">Sitcom</category>
   <episode-num system="xmltv_ns">3.0.</episode-num>
   <episode-num system="onscreen">S4 E1</episode-num>
   <episode-num system="original-air-date">2006-09-18</episode-num>
   ...
</programme>
```

### Create the XMLTV file using the command line (CLI): 

```
guide2go -config MY_CONFIG_FILE.yaml
```
**The configuration file must have already been created.**
