stat-reports
============

###About###
 - Dota 2 custom games helper library for allowing user reporting

# GetDotaStats -StatCollectionReports specifications 1.0 (Method names pending) #

## Client --> Server ##

#### INITIALISE ####
|Field Name|DataType|Field Description
|----------|--------------|-----------------
|type      |String        |Always "INITIALISE", as that's this packet
|modID     |String        |The modID allocated by GetDotaStats
|steamIDs  |JSON          |An array of steamIDs of all the users in-game.

#### REPORT ####
|Field Name|DataType|Field Description
|----------|--------------|-----------------
|type      |String        |Always "REPORT", as that's this packet
|modID     |String        |The modID allocated by GetDotaStats
|reportedID|String        |The SteamID of the user reported.
|reporterID|String        |The SteamID of the reporter.
|reportID  |Integer       |The ID that matches a report type as set by the modder via the site. (The client will know which to use as a result of another call)
|reportMisc|String        |Misc. description that the user feels like adding. It will most likely do nothing, but may be displayed to reported user, depending on future implementation design.

#### LIST ####
|Field Name|DataType|Field Description
|----------|--------------|-----------------
|type      |String        |Always "LIST", as that's this packet
|modID     |String        |The modID allocated by GetDotaStats.
|steamID   |String        |The SteamID of the user being interrogated.

## Server --> Client ##

#### success ####
|Field Name|DataType|Field Description
|----------|--------------|-----------------
|type      |String        |Always "success", as that's this packet

#### failure ####
|Field Name|DataType|Field Description
|----------|--------------|-----------------
|type      |String        |Always "failure", as that's this packet
|error     |String        |A string describing the error. Only useful for debugging purposes

#### initialise ####
|Field Name      |DataType|Field Description
|----------------|--------------|-----------------
|type            |String        |Always "initialise", as that's this packet
|reports         |JSON          |An array of steamIDs and their reports.
| -steamID       |String        |The steamID being interrogated.
| --reportsLeft  |Integer       |The global amount of reports this user has left to make in this mod. This number is ignored for specific reportIDs that have their own reportsLeft filled.
| --reportID     |Integer       |The ID that matches a report type as set by the modder via the site. (The client will know which to use as a result of another call)
| ---reportCount |Integer       |The number of reports this user has in this report type.
| ---reportAVG   |Real          |The average number of reports this user has per game.
| ---reportsLeft (Optional)|Integer       |The number of reports this user has left in this category. This field is optional, and if missing, the reportsLeft one level up will be used.
|reportTypes     |JSON          |An array of report types, as set via the site.
| -reportID      |Integer       |The ID that matches a report type as set by the modder via the site.
| --reportName   |String        |The name of the report type, as set by the modder via the site.
| --reportDesc   |String        |The description of the report type, as set by the modder via the site.
| --reportLimit  |Real          |The reportAVG limit set by the modder via the site. A user above this limit should be punished.

#### list ####
|Field Name      |||DataType|Field Description
|----------------|-----|-----|------------|-----------------
|type            |||String       |Always "list", as that's this packet
|saveID          |||Integer      |The next saveID to use
|reports         |||JSON         |An array of steamIDs and their reports.
|| reportsLeft   ||Integer      |The global amount of reports this user has left to make in this mod. This number is ignored for specific reportIDs that have their own reportsLeft filled.
|| reportID      ||Integer      |The ID that matches a report type as set by the modder via the site. (The client will know which to use as a result of another call)
||| reportCount    |Integer      |The number of reports this user has in this report type.
||| reportAVG      |Real         |The average number of reports this user has per game.
||| reportsLeft (Optional)|Integer       |The number of reports this user has left in this category. This field is optional, and if missing, the reportsLeft one level up will be used.
