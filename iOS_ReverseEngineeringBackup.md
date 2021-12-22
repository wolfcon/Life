---
title: Reverse Engineering the iOS Backup
date: 2021-12-22
---

# Reverse Engineering the iOS Backup

*Mar 17, 2017*

*[Last Updated: December 17, 2019](#change-log)*

I’ve seen this page getting increased traffic recently, so I’ve updated it to include extra information I have about the Notes, Photos, and Calendars format, and I’ve released my improved backup information extractor tool.

I’ve been working on a tool based on this research, which you can get here: [github.com/richinfante/iphonebackuptools](https://www.github.com/richinfante/iphonebackuptools). You can install it via npm also: `npm i -g ibackuptool` if you want to try it. I wrote a little more about this tool on my blog [over here](https://www.richinfante.com/2017/12/5/reverse-engineering-the-ios-backup-part-two)

## Introduction

I was recently exploring ways to “archive” content from iOS backups (mainly message transcripts) in an easily readable format. In order to accomplish this, I first started looking in the backups folder, which is located in. `~/Library/Application Support/MobileSync/Backup/` on macOS.

As it turns out, these backups are a treasure trove of information about almost every aspect of your iOS device, and they’re unencrypted by default. So even if you’ve got a fancy passcode on your phone, every time you sync with your computer, all of your data is stored **unencrypted** on your hard disk for anyone to see and use. I’ve found a few interesting things to note of so far, one of which appears to be a glich in iOS notes which causes notes some old iOS notes to be retained even though they are deleted. Additionally, the Photos library kindly stores location data in an sql database that we can query!

**Note: This page / project are a work in progress, and these are mostly notes to myself. I’m publishing them to help others if they’re looking to do something with iOS backups. Regardless, use this info at your own risk.**.

## First Glance

Inside this directory, there’s a bunch of directories with 40 character filenames which are SHA-1 hashes. Upon further research, they are sha-1 hashes of the file’s “Domain” concatenated with it’s Relative Path: `sha1(domain + '-' + relative_filename)`. Inside each of the backups, there exist a few notable files on first glance: `Info.plist`, `Manifest.db`, `Manifest.plist`, `Status.plist`

## Files List

- Technical Files
  - [Info.plist](#infoplist)
  - [Manifest.plist](#manifestplist)
  - [Status.plist](#statusplist)
  - [Manifest.db](#manifestdb)
  - [Manifest.mbdb](#manifestmbdb)
- Useful Files
  - [Messages](#messages)
  - [Address Book](#address-book)
  - [Call Log](#call-log)
  - [Voicemails](#voicemails)
  - [Old Notes](#old-notes)
  - [Notes](#notes)
  - [Web History](#web-history)
  - [Photos](#photos)
  - [WiFi History](#wifi)
  - [Calendars](#calendars)

## “Manifest Files”

### Info.plist

- Format: `plaintext plist`
- Provides information about the device, such as:
  - Device Name
  - Software Version
  - Installed Apps
    - Note: Apple has API to look up store entries like this for additional metadata: `http://itunes.apple.com/lookup?bundleId=com.yelp.yelpiphone`
  - Serial Number, etc.

### Manifest.plist

- Format: `binary plist`
- Provives information about the backup itself:
  - Is it encrypted
  - Comprehensive installed apps, including system software
  - Backup KeyBag

### Status.plist

- Format: `binary plist`
- Provides status information about the backup:
  - Version - I believe this is the version of the backup format (i’ve seen 2.4 and 3.2)
  - Status (is it complete, in progress)
  - Date Backup was taken
  - Is it a full backup?

### Manifest.db

- Format: `sqlite database`

- Contains an index of all the files in the backup

  - You should be able to use this index to retrive specific files of your choosing from the backup.

- Appears to be included in only v3.2 backups.

- Certain files in the manifest sometimes don’t actually exist in the backup.

- Contains two Tables

  - ```plaintext
    Files
    ```

    - `fileID TEXT PRIMARY KEY` the name of the file in the backup (derived from a hash of domain and path?)
    - `domain TEXT` What sandbox the file is in
    - `relativePath TEXT` File relative to domain
    - `flags INTEGER` Unix file flags?
    - `file BLOB` - Binary plist containing poperties

  - ```plaintext
    Properties
    ```

    - Two fields: `key TEXT PRIMARY KEY`, `value BLOB`
    - Empty in my backups, seems like a place to store file metadata.

### Manifest.mbdb

- Format: `binary file`
- In v2.4 backups
- Already been reverse engineered, [here](https://www.theiphonewiki.com/wiki/ITunes_Backup#Manifest.mbdb)

### Uses of Manifest files

We can use the `Status.plist` > `Version` to find out the structure of the rest of the backup.

- For version

   

  ```plaintext
  2.4
  ```

   

  (seen on iOS 9 Devices)

  - All of the files are contained in the same directory

- For version

   

  ```plaintext
  3.2
  ```

   

  (seen on iOS 10 & 11 Devices)

  - Backup Files are seperated into folders based on the first two characters in the filename. (I’m assuming this is for preformance especially for looking up files when there’s thousands in a single directory).

## Messages

This file is stored under the file “Library/SMS/sms.db” in the “HomeDomain” domain. It’s hash is: `3d0d7e5fb2ce288813306e4d4636395e047a3d28`, and is formatted as an sqlite database. It contains a few notable tables we can use:

### Message Table

The `message` table lists every message sent and recieved

- `ROWID` - the id of the message used in the `chat_message_join` table
- `text` - Text in the message
- `handle_id` - The handle of the user who sent it (see `handle` and `chat_handle_join` tables)
- `is_from_me` - boolean flag, did I send it?
- `date` - seconds since `2001-01-01 00:00:00` when sent (depends on timezone)
- There are many more flags attached to message records, such as read states, etc.

### Chat Table

The `chat` table lists the individual message threads.

- `ROWID` - the id of the chat used in the `chat_message_join` table
- `chat_identifier` - Who send the message? If it’s a group chat this gets a name such as chat1234567
- `display_name` - The display name. For group chats, if you name them, it is stored here.
- `properties` a binary plist formatted blob that contains information about the chat, such as time last used.

### Chat Message Join Table

The purpose of the `chat_message_join` is to link the rowids of chats and the messages inside of them.

- `chat_id` the id of the chat
- `message_id` the id of the message that is part of the chat

### Handle Table

The `handle` table contains a list of all the message “handles” used by chat participants.

- `ROWID` - the id of the handle, used in the `messages.handle_id` field.
- `id` - This is either the phone number / email of the sender
- `country` - This is the country of the handle - ex: `us`.
- `service` - This is either “iMessage” or “SMS”.

### Chat Handle Join Table

The `chat_handle_join` table joins the user handles with individual chats.

- `chat_id` - the chat the sender is in
- `handle_id` - the id of the handle of the participant

Newer iOS versions retain a lot more information. Heres’s the list of all of the table schemas I’ve collected: [github](https://github.com/richinfante/iphonebackuptools/tree/master/schemas/messages)

Using this info, we can write an sql query to retrive (most) of the messages from a conversation. There seem to be cases where messages are split into two distinct conversations, but we can just read them both using this query:

```
SELECT
  message.*,
  handle.id as sender_name
FROM chat_message_join
INNER JOIN message
  ON message.rowid = chat_message_join.message_id
INNER JOIN handle
  ON handle.rowid = message.handle_id
WHERE chat_message_join.chat_id = ?
```

## Address Book

This database is stored under the file “Library/AddressBook/AddressBook.sqlitedb” under the HomeDomain It’s filename on disk is: `31bb7ba8914766d4ba40d6dfb6113c8b614be442`. It is a little more complicated, with data structures pointing from an `ABPerson` record to tables with entries for phone numbers, etc.

### ABPerson Table

The following information is from an iPad running iOS 13:

- Useful Fields:
  - `ROWID` - unique row identifier, used for joins with ABMultiValue + Others.
  - `First` - First Name
  - `Last` - Last Name
  - `Middle` - Middle Name
  - `FirstPhonetic` - Phonetic First Name
  - `MiddlePhonetic` - Phonetic Middle Name
  - `LastPhonetic` - Phonetic Last Name
  - `FirstPronunciation` - First Name Pronunciation
  - `MiddlePronunciation` - Middle Name Pronunciation
  - `LastPronunciation` - Last Name Pronunciation
  - `PreviousFamilyName` - Maiden Name
  - `Organization` - Company / Organization
  - `OrganizationPhonetic` Organization Phonetic
  - `OrganizationPronunciation` - Organization Pronunciation
  - `Department` - Department
  - `Note` - Notes string
  - `Birthday` - Stored as number of floating point seconds since/before `2001-01-01`
  - `JobTitle` - Job title
  - `Nickname` - Nickname
  - `Prefix` - Prefix
  - `Suffix` - Suffix
  - `FirstSort` - Binary Data (Unknown)
  - `LastSort` - Binary Data (Unknown)
  - `CreationDate` - Stored as seconds since `2001-01-01`
  - `ModificationDate` - Stored as seconds since `2001-01-01`
- Fields with Unknown Uses:
  - `ExternalIdentifier` - `.vcf` file path, format: `//carddavhome/card/.vcf`
  - `ExternalModificationTag`
  - `ExternalUUID`: UUUID4.
  - `guid`: GUID. format: `:ABPerson`
  - `CompositeNameFallback`
  - `StoreID`
  - `DisplayName`
  - `ExternalRepresentation`
  - `FirstSortSection`
  - `LastSortSection`
  - `FirstSortLanguageIndex` - number
  - `LastSortLanguageIndex` - number
  - `PersonLink` integer
  - `ImageURI`
  - `IsPreferredName` - integer
  - `PhonemeData`
  - `AlternateBirthday`
  - `MapsData`
  - `PreferredLikenessSource`
  - `PreferredPersonaIdentfier`
  - `PreferredChannel`
  - `DowntimeWhitelist`
  - `ImageType`
  - `ImageHash`

### ABMultiValue

This PersonFullTextSearch_content method is not reliable across iOS versions and shouldn’t be used. To retrieve contacts, you can use the ABMultiValue Table.

#### ABMultiValue Property IDs

These appear to be directly related to the Objective-C [ABMultiValue Class](https://developer.apple.com/documentation/addressbook/abmultivalue). I extracted these from an iPad running iOS13, but they seem like they don’t change.

- `ROWID` - unique id

- `record_id` - the ROWID of the ABPerson Object.

- `identifier` - unique per (record_id, property) pairs.

- ```plaintext
  property
  ```

  - `3` - Phone Number

  - `4` - Email

  - ```plaintext
    12
    ```

     

    \- Dates

    - Birthday: Stored on main ABPerson Record.
    - Stored as seconds since `2001-01-01` in floating point.

  - ```plaintext
    16
    ```

     

    \- Text Tone

    - Value Contents:
      - `texttone:`, ex: `texttone:Bamboo`
      - `system:`, , ex: `system:By The Seaside`

  - `22` - Website

  - `23` - Relatives

- ```plaintext
  label
  ```

  - Used Globally:
    - `1` - Home
    - `2` - Other
    - `3` - iPhone
    - `4` - Work
    - `5` - Mobile
    - `6` - Main
    - `10` - Home fax
    - `15` - School
    - `16` - Work fax
    - `17` - Pager
  - Email:
    - `18` - iCloud
  - Dates:
    - `19` - Anniversary
  - Websites:
    - `7` - Homepage
  - Relations:
    - `30` - Mother
    - `31` - Father
    - `32` - Parent
    - `33` - Brother
    - `34` - Sister
    - `35` - Son
    - `36` - Daughter
    - `37` - Child
    - `38` - Friend
    - `39` - Spouse
    - `40` - Partner
    - `41` - Assistant
    - `42` - Manager

- `value` - value string for property above

- `guid` - guid for this entry.

### Full text search

The `ABPersonFullTextSearch_content` table contains generated columns for each type of data the contact can contain, such as phones, emails, etc. We can use this to search contacts in the backup based on data. It contains columns like `c16Phone` and `c17Email`. The numbering of the columns depends on the version.

#### Normalizing an address for searching in the table

The Phone column is not very well formatted, but if we preprepare our search for the phone number by stripping all whitespace and formatting, which results in a good success rate for looking up contacts by phone:

```
function preformatPhone(phone) {
  // Strip all formatting characters
  phone = phone.replace(/[\s+\-()]*/g, '')

  // if it's 11 digits, and the first one is a `1`, we can strip it because it's USA's country code.
  if(phone.length == 11 && phone[0] == '1') {
    phone = phone.substring(1)
  }

  return phone
}
```

Preprocessing the phone through in that manner before querying the database seems to work well enough:

```
-- Tested on backup version 2.4
-- This changes for other versions as numbering of columns changes.
-- See the ABPersonFullTextSearch_content Fields section.
SELECT
  c0First as first,
  c1Last as last,
  c2Middle as middle,
  c15Phone as phones
FROM ABPersonFullTextSearch_content WHERE c15Phone like '%phone number goes here%'`
```

Backup Version 3.2 `ABPersonFullTextSearch_content` Fields (iOS 10, iOS 11):

- `docid INTEGER PRIMARY KEY`
- `c0First`
- `c1Last`
- `c2Middle`
- `c3FirstPhonetic`
- `c4MiddlePhonetic`
- `c5LastPhonetic`
- `c6Organization`
- `c7OrganizationPhonetic`
- `c8Department`
- `c9Note`
- `c10Birthday`
- `c11JobTitle`
- `c12Nickname`
- `c13Prefix`
- `c14Suffix`
- `c15DisplayName`
- `c16Phone`
- `c17Email`
- `c18Address`
- `c19SocialProfile`
- `c20URL`
- `c21RelatedNames`
- `c22IM`
- `c23Date`
- `c24SupplementalCompositeNameTerms`

Backup Version 2.4 `ABPersonFullTextSearch_content` Fields (iOS 9?)

- `docid INTEGER PRIMARY KEY`
- `c0First`
- `c1Last`
- `c2Middle`
- `c3FirstPhonetic`
- `c4MiddlePhonetic`
- `c5LastPhonetic`
- `c6Organization`
- `c7Department`
- `c8Note`
- `c9Birthday`
- `c10JobTitle`
- `c11Nickname`
- `c12Prefix`
- `c13Suffix`
- `c14DisplayName`
- `c15Phone`
- `c16Email`
- `c17Address`
- `c18SocialProfile`
- `c19URL`
- `c20RelatedNames`
- `c21IM`
- `c22Date`
- `c23SupplementalCompositeNameTerms`

## Call Log

- sqlite database, filename: `5a4935c78a5255723f707230a451d79c540d2741`
- Stores information about phone calls, including metadata.

```
CREATE TABLE ZCALLDBPROPERTIES (
    Z_PK INTEGER PRIMARY KEY,
    Z_ENT INTEGER,
    Z_OPT INTEGER,
    ZTIMER_ALL FLOAT,
    ZTIMER_INCOMING FLOAT,
    ZTIMER_LAST FLOAT,
    ZTIMER_LIFETIME FLOAT,
    ZTIMER_OUTGOING FLOAT
);

CREATE TABLE ZCALLRECORD (
  Z_PK INTEGER PRIMARY KEY,
  Z_ENT INTEGER,
  Z_OPT INTEGER,
  ZANSWERED INTEGER,
  ZCALL_CATEGORY INTEGER,
  ZCALLTYPE INTEGER,
  ZDISCONNECTED_CAUSE INTEGER,
  ZFACE_TIME_DATA INTEGER,
  ZHANDLE_TYPE INTEGER,
  ZNUMBER_AVAILABILITY INTEGER,
  ZORIGINATED INTEGER,
  ZREAD INTEGER,
  ZDATE TIMESTAMP,
  ZDURATION FLOAT,
  ZDEVICE_ID VARCHAR,
  ZISO_COUNTRY_CODE VARCHAR,
  ZLOCATION VARCHAR,
  ZNAME VARCHAR,
  ZSERVICE_PROVIDER VARCHAR,
  ZUNIQUE_ID VARCHAR UNIQUE,
  ZADDRESS BLOB
);

-- Other tables:
CREATE INDEX Z_CallRecord_date ON ZCALLRECORD (ZDATE COLLATE BINARY ASC);
CREATE TABLE Z_PRIMARYKEY (Z_ENT INTEGER PRIMARY KEY, Z_NAME VARCHAR, Z_SUPER INTEGER, Z_MAX INTEGER);
CREATE TABLE Z_METADATA (Z_VERSION INTEGER PRIMARY KEY, Z_UUID VARCHAR(255), Z_PLIST BLOB);
CREATE TABLE Z_MODELCACHE (Z_CONTENT BLOB);
-- Works on iOS 9+
SELECT
  *,
  datetime(ZDATE + 978307200, 'unixepoch') AS XFORMATTEDDATESTRING
FROM ZCALLRECORD ORDER BY ZDATE ASC
ibackuptool -b <UDID> --report calls
```

## Voicemails

- sqlite file, name: `992df473bbb9e132f4b3b6e4d33f72171e97bc7a`
- Contains voicemail entries. We can actually pull the recordings from the backups!

```
CREATE TABLE voicemail (
  ROWID INTEGER PRIMARY KEY AUTOINCREMENT,
  remote_uid INTEGER,
  date INTEGER,
  token TEXT,
  sender TEXT,
  callback_num TEXT,
  duration INTEGER,
  expiration INTEGER,
  trashed_date INTEGER,
  flags INTEGER
);
```

Get a list of voicemails:

```
-- These datestamps are not 2001-relative.
SELECT *, datetime(date, 'unixepoch') AS XFORMATTEDDATESTRING from voicemail ORDER BY date ASC
```

Also, if you query the `Manifest.db` file, if it’s `iOS10+` and you can retrive the actual voicemail files.

```
SELECT * from FILES where relativePath like 'Library/Voicemail/%.amr'
# List voicemails and saved files.
ibackuptool -b <UDID> --report voicemail
ibackuptool -b <UDID> --report voicemail-files

# Or, We can optionally dump the files to disk!
ibackuptool -b <UDID> --report voicemail-files --export .
```

## Old Notes

- File: notes.sqlite `ca3bc056d4da0bbf88b5fb3be254f3b7147e639c`
- This information is from an iOS 11 backup.
- See [Notes](#notes) for the newer format.
- It appears to be a database from an iOS upgrade containing notes that no longer exist but the database was not deleted and (appears to) be no longer used.
- Stores Old Notes.app notes

```txt
Tables:
ZACCOUNT         ZNOTEATTACHMENT  ZPROPERTY        Z_MODELCACHE
ZNEXTID          ZNOTEBODY        ZSTORE           Z_PRIMARYKEY
ZNOTE            ZNOTECHANGE      Z_METADATA
```

Most of the interesting data is in the `ZNOTE` and `ZNOTEBODY` tables.

```
CREATE TABLE ZNOTE (
    Z_PK INTEGER PRIMARY KEY,
    Z_ENT INTEGER,
    Z_OPT INTEGER,
    ZCONTAINSCJK INTEGER,
    ZCONTENTTYPE INTEGER,
    ZDELETEDFLAG INTEGER,
    ZEXTERNALFLAGS INTEGER,
    ZEXTERNALSEQUENCENUMBER INTEGER,
    ZEXTERNALSERVERINTID INTEGER,
    ZINTEGERID INTEGER,
    ZISBOOKKEEPINGENTRY INTEGER,
    ZBODY INTEGER,  -- References Row in ZNOTEBODY Table
    ZSTORE INTEGER,
    ZCREATIONDATE TIMESTAMP,
    ZMODIFICATIONDATE TIMESTAMP,
    ZAUTHOR VARCHAR,  -- Author's email address
    ZGUID VARCHAR,
    ZSERVERID VARCHAR,
    ZSUMMARY VARCHAR,
    ZTITLE VARCHAR -- Title of the note
);

CREATE TABLE ZNOTEBODY (
    Z_PK INTEGER PRIMARY KEY,
    Z_ENT INTEGER,
    Z_OPT INTEGER,
    ZOWNER INTEGER,
    ZCONTENT VARCHAR, --  HTML formatted content
    ZEXTERNALCONTENTREF VARCHAR,
    ZEXTERNALREPRESENTATION BLOB
  );
```

Many of the notes contained in the tables here no longer exist, and haven’t for some time (2014). Also, new notes I added would not show up here, indicating that there may be a new database for iOS’s upgraded notes. This may be useful since the notes appear to be not deletable by the user. In fact, the user probably doesn’t know there’s copies of these notes still around.

Notes also appear to be stored as HTML documents, which is kind of neat. You can view the all the note contents using the following query.

```
SELECT * from ZNOTE LEFT JOIN ZNOTEBODY ON ZBODY = ZNOTEBODY.Z_PK;
ibackuptool -b <UDID> --report oldnotes # Print ALL Notes data
```

## Notes

- File hash: `4f98687d8ab0d6d1a371110e6b7300f6e465bef2`
- Sqlite database.
- iOS9, iOS10, iOS11
  - iOS 9 has slightly differed columns

```txt
Tables:
ACHANGE                    ZICSEARCHINDEXTRANSACTION
ATRANSACTION               ZICSERVERCHANGETOKEN
ZICCLOUDSTATE              ZNEXTID
ZICCLOUDSYNCINGOBJECT      Z_METADATA
ZICLOCATION                Z_MODELCACHE
ZICNOTEDATA                Z_PRIMARYKEY
-- iOS 11 schema
-- This is a HUGE table
-- There's a lot more information here than previous versions.
CREATE TABLE ZICCLOUDSYNCINGOBJECT (
    Z_PK INTEGER PRIMARY KEY,
    Z_ENT INTEGER,
    Z_OPT INTEGER,
    ZCRYPTOITERATIONCOUNT INTEGER,
    ZISPASSWORDPROTECTED INTEGER,
    ZMARKEDFORDELETION INTEGER,
    ZMINIMUMSUPPORTEDNOTESVERSION INTEGER,
    ZNEEDSINITIALFETCHFROMCLOUD INTEGER,
    ZNEEDSTOBEFETCHEDFROMCLOUD INTEGER,
    ZNEEDSTOSAVEUSERSPECIFICRECORD INTEGER,
    ZCLOUDSTATE INTEGER,
    ZCHECKEDFORLOCATION INTEGER,
    ZFILESIZE INTEGER,
    ZHASMARKUPDATA INTEGER,
    ZIMAGEFILTERTYPE INTEGER,
    ZORIENTATION INTEGER,
    ZSECTION INTEGER,
    ZLOCATION INTEGER,
    ZMEDIA INTEGER,
    ZNOTE INTEGER,
    ZNOTEUSINGTITLEFORNOTETITLE INTEGER,
    ZPARENTATTACHMENT INTEGER,
    ZSCALEWHENDRAWING INTEGER,
    ZVERSION INTEGER,
    ZVERSIONOUTOFDATE INTEGER,
    ZATTACHMENT INTEGER,
    ZSTATE INTEGER,
    ZACCOUNT INTEGER,
    ZTYPE INTEGER,
    ZACCOUNT1 INTEGER,
    ZATTACHMENT1 INTEGER,
    ZATTACHMENTVIEWTYPE INTEGER,
    ZISPINNED INTEGER,
    ZLEGACYNOTEWASPLAINTEXT INTEGER,
    ZNOTEHASCHANGES INTEGER,
    ZPAPERSTYLETYPE INTEGER,
    ZACCOUNT2 INTEGER,
    ZFOLDER INTEGER,
    ZNOTEDATA INTEGER,
    ZTITLESOURCEATTACHMENT INTEGER,
    ZISHIDDENNOTECONTAINER INTEGER,
    ZSORTORDER INTEGER,
    ZOWNER INTEGER,
    ZACCOUNTTYPE INTEGER,
    ZDIDCHOOSETOMIGRATE INTEGER,
    ZDIDFINISHMIGRATION INTEGER,
    ZDIDMIGRATEONMAC INTEGER,
    ZFOLDERTYPE INTEGER,
    ZIMPORTEDFROMLEGACY INTEGER,
    ZACCOUNT3 INTEGER,
    ZPARENT INTEGER,
    ZCREATIONDATE TIMESTAMP, -- Legacy creation?
    ZCROPPINGQUADBOTTOMLEFTX FLOAT,
    ZCROPPINGQUADBOTTOMLEFTY FLOAT,
    ZCROPPINGQUADBOTTOMRIGHTX FLOAT,
    ZCROPPINGQUADBOTTOMRIGHTY FLOAT,
    ZCROPPINGQUADTOPLEFTX FLOAT,
    ZCROPPINGQUADTOPLEFTY FLOAT,
    ZCROPPINGQUADTOPRIGHTX FLOAT,
    ZCROPPINGQUADTOPRIGHTY FLOAT,
    ZDURATION FLOAT,
    ZMODIFICATIONDATE TIMESTAMP,
    ZORIGINX FLOAT,
    ZORIGINY FLOAT,
    ZPREVIEWUPDATEDATE TIMESTAMP,
    ZSIZEHEIGHT FLOAT,
    ZSIZEWIDTH FLOAT,
    ZHEIGHT FLOAT,
    ZMODIFIEDDATE TIMESTAMP, -- Legacy modified?
    ZSCALE FLOAT,
    ZWIDTH FLOAT,
    ZSTATEMODIFICATIONDATE TIMESTAMP,
    ZMODIFICATIONDATEATIMPORT TIMESTAMP,
    ZCREATIONDATE1 TIMESTAMP, -- Creation Date
    ZFOLDERMODIFICATIONDATE TIMESTAMP,
    ZLASTNOTIFIEDDATE TIMESTAMP,
    ZLASTVIEWEDMODIFICATIONDATE TIMESTAMP,
    ZLEGACYMODIFICATIONDATEATIMPORT TIMESTAMP,
    ZMODIFICATIONDATE1 TIMESTAMP,  -- Modified Date
    ZDATEFORLASTTITLEMODIFICATION TIMESTAMP,
    ZPARENTMODIFICATIONDATE TIMESTAMP,
    ZIDENTIFIER VARCHAR UNIQUE,
    ZPASSWORDHINT VARCHAR,
    ZZONEOWNERNAME VARCHAR,
    ZADDITIONALINDEXABLETEXT VARCHAR,
    ZFALLBACKSUBTITLEIOS VARCHAR, Z
    FALLBACKSUBTITLEMAC VARCHAR,
    ZFALLBACKTITLE VARCHAR,
    ZREMOTEFILEURLSTRING VARCHAR,
    ZSUMMARY VARCHAR,
    ZTITLE VARCHAR, -- Legacy Title?
    ZTYPEUTI VARCHAR,
    ZURLSTRING VARCHAR,
    ZUSERTITLE VARCHAR,
    ZDEVICEIDENTIFIER VARCHAR,
    ZCONTENTHASHATIMPORT VARCHAR,
    ZFILENAME VARCHAR,
    ZLEGACYCONTENTHASHATIMPORT VARCHAR,
    ZLEGACYIMPORTDEVICEIDENTIFIER VARCHAR,
    ZLEGACYMANAGEDOBJECTIDURIREPRESENTATION VARCHAR,
    ZSELECTEDINKCOLORSTRING VARCHAR,
    ZSELECTEDINKIDENTIFIER VARCHAR,
    ZSNIPPET VARCHAR,
    ZTHUMBNAILATTACHMENTIDENTIFIER VARCHAR,
    ZTITLE1 VARCHAR,  -- Display Title
    ZACCOUNTNAMEFORACCOUNTLISTSORTING VARCHAR,
    ZNESTEDTITLEFORSORTING VARCHAR,
    ZNAME VARCHAR,
    ZUSERRECORDNAME VARCHAR,
    ZTITLE2 VARCHAR,  -- Another title, seems to be related to folders.
    ZASSETCRYPTOINITIALIZATIONVECTOR BLOB,
    ZASSETCRYPTOTAG BLOB,
    ZCRYPTOINITIALIZATIONVECTOR BLOB,
    ZCRYPTOSALT BLOB,
    ZCRYPTOTAG BLOB,
    ZCRYPTOWRAPPEDKEY BLOB,
    ZENCRYPTEDVALUESJSON BLOB,
    ZSERVERRECORDDATA BLOB,
    ZSERVERSHAREDATA BLOB,
    ZUNAPPLIEDENCRYPTEDRECORD BLOB,
    ZUSERSPECIFICSERVERRECORDDATA BLOB,
    ZFALLBACKIMAGECRYPTOINITIALIZATIONVECTOR BLOB,
    ZFALLBACKIMAGECRYPTOTAG BLOB,
    ZMARKUPMODELDATA BLOB,
    ZMERGEABLEDATA BLOB,
    ZMETADATADATA BLOB,
    ZCRYPTOMETADATAINITIALIZATIONVECTOR BLOB,
    ZCRYPTOMETADATATAG BLOB,
    ZENCRYPTEDMETADATA BLOB,
    ZMETADATA BLOB,
    ZLASTNOTIFIEDTIMESTAMPDATA BLOB,
    ZLASTVIEWEDTIMESTAMPDATA BLOB,
    ZREPLICAIDTOUSERIDDICTDATA BLOB,
    ZCRYPTOVERIFIER BLOB
);
-- For iOS 9
SELECT
  *,
  datetime(ZCREATIONDATE + 978307200, 'unixepoch') AS XFORMATTEDDATESTRING
  FROM ZICCLOUDSYNCINGOBJECT

-- For iOS 10-11
SELECT
  *,
  -- Apple appears to have incremented the creation date stamp, but grab them both
  datetime(ZCREATIONDATE + 978307200, 'unixepoch') AS XFORMATTEDDATESTRING,
  datetime(ZCREATIONDATE1 + 978307200, 'unixepoch') AS XFORMATTEDDATESTRING1
  FROM ZICCLOUDSYNCINGOBJECT
ibackuptool -b <UDID> --report notes # Print ALL Notes data
```

## Web History

- File: Sqlite Database, `e74113c185fd8297e140cfcf9c99436c5cc06b57`
- This information is from an iOS 11 backup.

```
-- Contains entry for each website
CREATE TABLE history_items (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  url TEXT NOT NULL UNIQUE,
  domain_expansion TEXT NULL,
  visit_count INTEGER NOT NULL,
  daily_visit_counts BLOB NOT NULL,
  weekly_visit_counts BLOB NULL,
  autocomplete_triggers BLOB NULL,
  should_recompute_derived_visit_counts INTEGER NOT NULL,
  visit_count_score INTEGER NOT NULL DEFAULT 0
);

-- Contains entries for each visit
CREATE TABLE history_visits (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    -- Reference to the actual website entry in history_items
    history_item INTEGER NOT NULL REFERENCES history_items(id) ON DELETE CASCADE,
    visit_time REAL NOT NULL,
    title TEXT NULL,
    load_successful BOOLEAN NOT NULL DEFAULT 1,
    http_non_get BOOLEAN NOT NULL DEFAULT 0,
    synthesized BOOLEAN NOT NULL DEFAULT 0,
    redirect_source INTEGER NULL UNIQUE REFERENCES history_visits(id) ON DELETE CASCADE,
    redirect_destination INTEGER NULL UNIQUE REFERENCES history_visits(id) ON DELETE CASCADE,
    origin INTEGER NOT NULL DEFAULT 0,
    generation INTEGER NOT NULL DEFAULT 0,
    attributes INTEGER NOT NULL DEFAULT 0,
    score INTEGER NOT NULL DEFAULT 0
  );

-- Other tables:
CREATE TABLE sqlite_sequence(name,seq);
CREATE TABLE history_tombstones (id INTEGER PRIMARY KEY AUTOINCREMENT,start_time REAL NOT NULL,end_time REAL NOT NULL,url TEXT,generation INTEGER NOT NULL DEFAULT 0);
CREATE TABLE metadata (key TEXT NOT NULL UNIQUE, value);
CREATE TABLE history_client_versions (client_version INTEGER PRIMARY KEY,last_seen REAL NOT NULL);
CREATE TABLE history_event_listeners (listener_name TEXT PRIMARY KEY,last_seen REAL NOT NULL);
CREATE TABLE history_events (id INTEGER PRIMARY KEY AUTOINCREMENT,event_type TEXT NOT NULL,event_time REAL NOT NULL,pending_listeners TEXT NOT NULL,value BLOB);
```

I used the following SQL query to dump all possbile information about each visit:

```
SELECT * from history_visits LEFT JOIN history_items ON history_items.ROWID = history_visits.history_item
ibackuptool -b <udid> --report webhistory
```

## Photos

- File: Sqlite Database,`12b144c0bd44f2b3dffd9186d3f9c05b917cee25`
- This information is from an iOS 11 backup.
- Stores photo asset information,
- There’s a LOT of information here, including easily queryable location data & timestamps.
- Here’s all of the tables:

```txt
ZADDITIONALASSETATTRIBUTES              ZPERSONREFERENCE
ZADJUSTMENT                             ZSCENECLASSIFICATION
ZALBUMLIST                              ZSEARCHDATA
ZASSETANALYSISSTATE                     ZSIDECARFILE
ZASSETDESCRIPTION                       ZUNMANAGEDADJUSTMENT
ZCLOUDFEEDENTRY                         Z_15CLUSTERREJECTEDPERSONS
ZCLOUDMASTER                            Z_15REJECTEDPERSONS
ZCLOUDMASTERMEDIAMETADATA               Z_15REJECTEDPERSONSNEEDINGFACECROPS
ZCLOUDRESOURCE                          Z_19ALBUMLISTS
ZCLOUDSHAREDALBUMINVITATIONRECORD       Z_1KEYWORDS
ZCLOUDSHAREDCOMMENT                     Z_20ASSETS
ZDEFERREDREBUILDFACE                    Z_27MEMORIESBEINGCURATEDASSETS
ZDETECTEDFACE                           Z_27MEMORIESBEINGEXTENDEDCURATEDASSETS
ZDETECTEDFACEGROUP                      Z_27MEMORIESBEINGMOVIECURATEDASSETS
ZDETECTEDFACEPRINT                      Z_27MEMORIESBEINGREPRESENTATIVEASSETS
ZFACECROP                               Z_36INVALIDMERGECANDIDATES
ZGENERICALBUM                           Z_36MERGECANDIDATES
ZGENERICASSET                           Z_METADATA
ZKEYWORD                                Z_MODELCACHE
ZLEGACYFACE                             Z_PRIMARYKEY
ZMEMORY                                 Z_RT_GenericAsset_boundedByRect
ZMOMENT                                 Z_RT_GenericAsset_boundedByRect_node
ZMOMENTLIBRARY                          Z_RT_GenericAsset_boundedByRect_parent
ZMOMENTLIST                             Z_RT_GenericAsset_boundedByRect_rowid
```

The most interesting one I’ve seen so far is the `ZGENERICASSET` table. It stores a lot of information about each photo, *including location information*.

```
CREATE TABLE ZGENERICASSET (
    Z_PK INTEGER PRIMARY KEY,
    Z_ENT INTEGER,
    Z_OPT INTEGER,
    ZAVALANCHEPICKTYPE INTEGER,
    ZCLOUDDOWNLOADREQUESTS INTEGER,
    ZCLOUDHASCOMMENTSBYME INTEGER,
    ZCLOUDHASCOMMENTSCONVERSATION INTEGER,
    ZCLOUDHASUNSEENCOMMENTS INTEGER,
    ZCLOUDISDELETABLE INTEGER,
    ZCLOUDISMYASSET INTEGER,
    ZCLOUDLOCALSTATE INTEGER,
    ZCLOUDPLACEHOLDERKIND INTEGER,
    ZCOMPLETE INTEGER,
    ZCUSTOMRENDEREDVALUE INTEGER,
    ZDEPTHSTATES INTEGER,
    ZFACEAREAPOINTS INTEGER,
    ZFAVORITE INTEGER,
    ZGROUPINGSTATE INTEGER,
    ZHASADJUSTMENTS INTEGER,
    ZHEIGHT INTEGER,
    ZHIDDEN INTEGER,
    ZKIND INTEGER,
    ZKINDSUBTYPE INTEGER,
    ZLOCALRESOURCESSTATE INTEGER,
    ZORIENTATION INTEGER,
    ZPLAYBACKSTYLE INTEGER,
    ZPLAYBACKVARIATION INTEGER,
    ZSAVEDASSETTYPE INTEGER,
    ZTHUMBNAILINDEX INTEGER,
    ZTRASHEDSTATE INTEGER,
    ZVIDEOCPDURATIONVALUE INTEGER,
    ZVIDEOCPVISIBILITYSTATE INTEGER,
    ZVISIBILITYSTATE INTEGER,
    ZWIDTH INTEGER,
    ZADDITIONALATTRIBUTES INTEGER,
    ZCLOUDFEEDASSETSENTRY INTEGER,
    ZMASTER INTEGER,
    ZMOMENT INTEGER,
    ZSEARCHDATA INTEGER,
    Z_FOK_CLOUDFEEDASSETSENTRY INTEGER,
    Z_FOK_MOMENT INTEGER,
    ZADDEDDATE TIMESTAMP,
    ZADJUSTMENTTIMESTAMP TIMESTAMP,
    ZCLOUDBATCHPUBLISHDATE TIMESTAMP,
    ZCLOUDLASTVIEWEDCOMMENTDATE TIMESTAMP,
    ZCLOUDSERVERPUBLISHDATE TIMESTAMP,
    ZCURATIONSCORE FLOAT,
    ZDATECREATED TIMESTAMP,  ---- Timestamp
    ZDURATIONFLOAT,
    ZFACEADJUSTMENTVERSION TIMESTAMP,
    ZLASTSHAREDDATE TIMESTAMP,
    ZLATITUDE FLOAT,  ---- Interesting...
    ZLONGITUDE FLOAT,  ---- Interesting...
    ZMODIFICATIONDATE TIMESTAMP,
    ZSORTTOKEN FLOAT,
    ZTRASHEDDATE TIMESTAMP,
    ZAVALANCHEUUID VARCHAR,
    ZCLOUDASSETGUID VARCHAR,
    ZCLOUDBATCHID VARCHAR,
    ZCLOUDCOLLECTIONGUID VARCHAR,
    ZCLOUDOWNERHASHEDPERSONID VARCHAR,
    ZDIRECTORY VARCHAR,
    ZFILENAME VARCHAR, ---- Filename
    ZGROUPINGUUID VARCHAR,
    ZMEDIAGROUPUUID VARCHAR,
    ZORIGINALCOLORSPACE VARCHAR,
    ZUNIFORMTYPEIDENTIFIER VARCHAR,
    ZUUID VARCHAR,
    ZLOCATIONDATA BLOB,  ---- Interesting...
    ZIMAGEURLDATA BLOB,
    ZTHUMBNAILURLDATA BLOB,
    ZWALLPAPEROPTIONSDATA BLOB
  );
```

Grab all photo names and location information:

```
SELECT
  ZDATECREATED, -- Creation Timestamp
  ZLATITUDE,  -- Latitude
  ZLONGITUDE, -- Longitude
  ZFILENAME -- On-Disk filename
FROM ZGENERICASSET
ORDER BY ZDATECREATED ASC
ibackuptool -b <udid> --report photolocations
```

## WiFi

- We can also collect all of the wifi locations and their last accessed time. Combined with open source data on WiFi access point locations, we can obtain a location history for recent visits to locations.

- binary plist format, filename: `ade0340f576ee14793c607073bd7e8e409af07a8`

- This PLIST contains a key

   

  ```plaintext
  List of known networks
  ```

   

  which then in turn is an array of items,

  - `lastJoined` - Last time the user manually joined the network.

  - `lastAutoJoined` - Last time the device automatically connected. We could use this to place a phone at a specific location/timestamp even if the phone simply went near the access point.

  - `SSID_STR` - user readable SSID String

  - `BSSID` - MAC address of AP - useful for geolocating the actual access point using a database such as [find-wifi.mylnikov.org](https://find-wifi.mylnikov.org/)

  - `SecurityMode` - Is there security enabled?

  - `HIDDEN_NETWORK` - Is it hidden?

  - ```plaintext
    networkKnownBSSListKey
    ```

     

    \- For enterprise/multi-ap networks, the phone maintains a list of all access points that have been connected.

    - `CHANNEL` - The AP’s channel
    - `BSSID` - The AP’s BSSID
    - `lastRoamed` - The last time the AP was connected.

```
ibackuptool -b <UDID> --report wifi
```

## Call history

- File: call_history.db `2b2b0084a1bc3a5ac8c27afdf14afb42c61a19ca`
- Stores call history

## Calendars

- File: Calendar.sqlitedb `2041457d5fe04d39d0ab481178355df6781e6858`
- Stores calendars and reminders
- This information is from an iOS 11 backup.

```txt
Tables:
Alarm                      Identity
AlarmChanges               Location
Attachment                 Notification
AttachmentChanges          NotificationChanges
Calendar                   OccurrenceCache
CalendarChanges            OccurrenceCacheDays
CalendarItem               Participant
CalendarItemChanges        ParticipantChanges
Category                   Recurrence
CategoryLink               RecurrenceChanges
ClientCursor               ResourceChange
ClientCursorConsumed       ScheduledTaskCache
ClientSequence             Sharee
Contact                    ShareeChanges
ContactChanges             Store
EventAction                StoreChanges
EventActionChanges         SuggestedEventInfo
ExceptionDate              _SqliteDatabaseProperties
```

## Locations

- File: consolidated.db `4096c9ec676f2847dc283405900e284a7c815836`
- Stores compass calibration info??

## Legal

**DISCLAIMER: This tool enables the extraction of personal information from iPhone backups located on a computer drive. The tool is for testing purposes and should ONLY be used on iPhone backups where the owner’s consent has been given. Do not use this tool for illegal purposes, ever.**

**The project contributors and Richard Infante will not be held responsible in the event any criminal charges be brought against any individuals misusing this tool and/or the information contained within, to break the law.**

## Change Log

- `2017-03-17` - Initial Revision
- `2017-12-05` - Updated additional info about iOS 11 backup format and add notes about Calendars, Notes, Photos format.
- `2017-12-06` - Updated iOS notes with new database information
- `2017-02-06` - Add link to messages schemas
- `2017-12-10` - Add Voicemail, Call Logs, and WiFi
- `2019-12-17` - Add information about contacts, sms. Minor updates + extra information. Grammar & spelling fixes.

Found a typo or technical problem? [file an issue!](https://github.com/richinfante/richinfante.com/issues/new?body=  **Page the issue appears on:**  > [richinfante.com/2017/3/16/reverse-engineering-the-ios-backup](https://www.richinfante.com/2017/3/16/reverse-engineering-the-ios-backup)   **Description:**  > What did I mess up?  )

> Repost from [Reverse Engineering the iOS Backup](https://www.richinfante.com/2017/3/16/reverse-engineering-the-ios-backup)

