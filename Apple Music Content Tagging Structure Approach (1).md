## Super Hi-Fi 
# Apple Music
# Content Tagging Structure Approach

*All Materials &copy;2021 Super Hi-Fi, Patent-Protected and Strictly Confidential*

**Date**: 12 August 2021

## Overview

This document provides content tagging recommendations for Apple Music's non-music assets. This tagging structure aims to optimize the usability of Apple Music's content within the Super Hi-Fi platform while at the same time minimize the amount of work required in order to perform this tagging. It was built using a sampling of Apple Music content that was gathered and reviewed by Super Hi-Fi during our proof-of-concept period. Please consider the document a first draft that may require a few revisions as more is learned about Apple Music's content and ways in which the content is utilized.

There are three main goals of this document:

- Highlight the Common Features SHF observed across the POC content and share the organization scheme that was employed during that phase.

- Enumerate each Content Type and associated sub-types alongside clear tagging recommendations for each example.

- Identify key areas for production and technical teams to discuss further.

## Common Features

There are many commonalities across Apple Music's non-music assets. By using identifiers within the content organization structure, assets can be tagged in a manner for the AI to calculate optimal playback relationships. This section provides a walk through of certain required fields, unique identifiers and content types, and other industry standard radio programming concepts and categories to tag and group assets.  

### Identifiers

An identifier is the unique value associated with a specific content asset. As a requirement, every content asset must have a unique identifier that should be issued by a central authority and unique to Apple’s content catalogue. The identifier must satisfy the following regular expression (`/^[0-9A-Za-z.\-_]+$/`) which means using a combination of alphanumeric characters, dots, dashes, and underscores. No identifier should be reused except for when an update of a new version of an asset occurs (e.g. if there was too much reverb on an asset and a remixed version with less processing is swapped in).

#### Content Types 

During the POC phase, there were four unique types of content used to create the show mixdowns. There are several other types of content defined in the *Super Hi-Fi Music & Content Integration* document.

- interstitials: relatively short, fully produced elements
- liners: dry, unproduced voiceover
- musicbeds: music that is intended to play underneath liners
- content: longer form, fully produced segments

#### Content Organization Scheme

The following attributes help place a piece of content at the proper location within the hierarchy of Apple Music experiences. 

##### Channel

Channels are defined as brand properties that fall under the Apple Music suite of music experiences. For example, **Apple Music Country** is represented with a Channel value of **"country"**.

- country
- one
- hits
- unassigned

##### Experience Type

Experience Type represents the listening experience style of the show or station. A three letter code was created for each Experience Type. 

- pla: playlist experiences
- anc: anchored experiences
- alp: artist-led programming
- lbk: lean-back listening

##### Show or DJ Hour

Content that is specific to a show or DJ Hour has its name associated in metadata tag, represented in camelCase.

- 80sRadioWithHueyLewis: 80s Radio with Huey Lewis
- hunkerDownRadio: Hunker Down Radio
- jaydeDonovan: Jayde Donovan
- appleMusicHits: Apple Music Hits listening experience

##### Episode / Listening Hour

The episode number of the show show or the timestamp of the hourly programming. 

- Episode: epXXX (e.g. Pure Throwback Episode 225 = ep225)
- Listening Hour: YYYYMMDDHH (e.g. Apple Music Hits on August 12th, 2021 at 1 PM PST = 2021081213)

##### Collection

An optional string value that groups nearly equivalent pieces of content so their use is more measured across the experience. For example:

- “This is Backporch Radio on Apple Country”
- “On Apple Country, this is Backporch Radio”

Both examples have slightly different station identifiers, but say essentially the same thing. If placed in the same virtual collection, the AI will know that these two assets should not play in close proximity to one another.
          
##### Voice

The attribute to use for including the talent name associated with the asset. Labeling the asset with consistent voice tags will allow for easier asset search and management in the future. For example:

- Jayde Donovan
- Bree
- Kacey Jo
- Nicole Sky
        
## Content Types

### Interstitials

Interstitials are fully produced content elements that play between tracks.

#### Opener Interstitials

These are fully produced interstitials that are intended to be used at the opening of a show. 

| Field | Value |
| --- | --- |
| assetType | "interstitials" |
| id | [ contentId ] |
| metatype | "bookends" |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |
| tags | "OPEN" (required), "BED" |

The tags above represent the following concepts:

* OPEN: a required tag that indicates that the interstitials is an opening show asset.
* BED: an optional tag that indicates that the interstitials contains a music bed. 

Example Opener Interstitial:
```
{
	"data": {
		"assetType": "interstitials", 
		"id": "f3c1d42f41a3436cb2faa58c2de9c84d", 
		"filename": "OPEN-BPCR-VERSION 2 REVISED-NO BED-EP AND MB.wav",
		"metatype": "bookends",
		"collection": "BPCR004",
		"voice": "EP and MB",
		"relatedContextName": "country",
    	"relatedContextId": "backPorchCountryRadio",
		"tags": [ "OPEN", "BED" ]
	}
}
```

#### Closer Interstitials

These are fully produced interstitials that are intended to be used at the close of a show. 

| Field | Value |
| --- | --- |
| assetType | "interstitials" |
| id | [ contentId ] |
| metatype | "bookends" |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] | 
| tags | "CLOSE" (required) |

The tags above represent the following concepts:

* CLOSE: a required tag that indicates that the interstitial is used exclusively to end a show.

Example Closer Interstitial:

```
{
	"data": {
		"assetType": "interstitials", 
		"id": "88fc6b324fdB4e8abfb12f19b70848cb", 
		"filename": "IMAGING CLOSE EP146.wav",
		"metatype": "bookends",
		"relatedContextName": "hits",
    	"relatedContextId": "rockClassics",
		"tags": [ "OPEN" ]
	}
}
```

#### Generic Channel-Branded Interstitials

These are fully produced interstitials that are intended to be used across all types of experiences within the channel (e.g. Apple Music Country). 
            
| Field | Value |
| --- | --- |
| assetType | "interstitials" |
| id | [ contentId ] |
| metatype | "branding" |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| tags | "GENERIC" (required), "DROP", "MALE", "FEMALE" |

The tags above represent the following concepts:

* GENERIC: a required tag that indicates that the interstitial is generic for the entire channel.
* DROP: an optional tag used to notate "one-shot" style content (a short word, phrase, and/or sound effect designed to be placed tightly between two tracks).
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a female voice is present on the content.

Example Generic Channel-Branded Interstitial:

```
{
	"data": {
		"assetType": "interstitials", 
		"id": "4054f1f86b574467b745ac4fd5eec18b", 
		"filename": "AM1-Generic-SWP 11 (Clean).wav",
		"metatype": "branding",
		"collection": "AM1-SWP11",
		"voice": "Amber",
		"relatedContextName": "one",
		"tags": [ "GENERIC", "FEMALE" ]
	}
}
```
            
#### Show-Specific Interstitials

These are fully produced interstitials that are intended to be used across one specific show on Apple Music (e.g. Rock Classics). 

| Field | Value |
| --- | --- |
| assetType | "interstitials" |
| id | [ contentId ] |
| metatype | "branding" |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |
| tags | "SPECIFIC" (required), "SWEEPER", "MALE", "FEMALE" |

The tags above represent the following concepts:

* SPECIFIC: a required tag that indicates that the interstitial is specific for the show.
* SWEEPER: an optional tag used to indicate that the asset style is a sweeper (e.g. a brief promotional or branding segue)
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

Example Show-Specific Interstitials:

```
{
	"data": {
		"assetType": "interstitials", 
		"id": "a00d777f50c5419c839880b0ffaa71ee", 
		"filename": "SWP-BPCR-YOU'RE ON-PRODUCED-EP ONLY.wav",
		"metatype": "branding",
		"collection": "BPCR012",
		"voice": "Ep",
		"relatedContextName": "country",
    	"relatedContextId": "backPorchCountryRadio",
		"tags":	[ "SPECIFIC", "FEMALE" ]
	}
}
```

### Liners

Liners are dry voices that are intended to play over songs, music beds, and/or hybrid interstitials that contain music beds.

#### Generic Channel Liners

These are dry voice audio assets only with no effects, intended to be used across all types of experiences within the channel (e.g. Apple Music Country). 

| Field | Value |
| --- | --- |
| assetType | "liners" |
| id | [ contentId ] |
| metatype | "branding" |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| tags | "GENERIC" (required), "MALE, "FEMALE", "SWEEPER" |

The tags above represent the following concepts:

* GENERIC: a required tag that indicates that the liner is generic for the entire channel.
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.
* SWEEPER: an optional tag used to indicate that the asset style is a sweeper (e.g. a brief promotional or branding segue)

Example Generic Channel Liner:

```
{
	"data": {
		"assetType": "liners", 
		"id": "da8cae3a48364893abee66d436f638f6", 
		"filename": "AMC-Quick SWP 8 DRY - 051420.wav",
		"metatype": "branding",
		"collection": "AMC012",
		"voice": "Ep",
		"relatedContextName": "country",
		"tags": [ "GENERIC", "FEMALE", "SWEEPER" ]
	}
}
```

#### Show-Specific Liners

These are dry voice audio assets only with no effects, intended to be used across one specific show on Apple Music (e.g. Rock Classics). 

| Field | Value |
| --- | --- |
| assetType | "liners" |
| id | [ contentId ] |
| metatype | "branding" |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |
| tags | "SPECIFIC" (required), "MALE", "FEMALE" |

The tags above represent the following concepts:

* SPECIFIC: a required tag that indicates that the liner is specific for the show.
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

Example Show-Specific Liner:


```
{
	"data": {
		"assetType": "liners", 
		"id": "c4a3a57811c94e38b021d7466745d062", 
		"filename": "1 OUT OF FEEL IT STILL INTO SMILE THROWBACK EP 225_AMH.wav",
		"metatype": "branding",
		"voice": "Nicole Sky",
		"relatedContextName": "hits",
    	"relatedContextId": "pureThrowback",
		"tags": [ "SPECIFIC",  "FEMALE" ]
	}
}
```

#### Episode-Specific (or Time-Specific) Placed Voicetracks

These are dry voice audio assets only with no effects, intended to be used on one specific episode of a show on Apple Music (e.g. Rock Classics). These can also be targeted to a specific hour of a show, or playlist if the duration of the show exceeds one hour.   

| Field | Value |
| --- | --- |
| assetType | "liners" |
| id | [ contentId ] |
| metatype | "voicetrack" |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |
| relatedTrackId | [ track identifier ] |
| values | "key": episode, "value": [ epXXX ] |
| values | "key": hours, "value": [ YYYYMMDDHH ] |
| tags | "BACK" or "FRONT" (required), "MALE", "FEMALE" |

The key value pairs are used to indicate the following concepts:

* The episode and number combination is to be used if tagging a show.
* The hour and date combination is to be used if tagging a listening hour.

The tags above represent the following concepts:

* BACK: a required tag if the related track identifier refers to the track that plays before the liner.
* FRONT: a required tag if the related track identifier refers to the the track that plays after the liner. 
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

Example Episode Specific Placed Voicetrack:
```
{
	"data": {
		"assetType": "liners", 
		"id": "77e1303cf09440f885ff0e08d8b8d6f2", 
		"filename": "1 Into Dierks Bentley Living.wav",
		"metatype": "voicetrack",
		"voice": "Nick Hoffman",
		"relatedContextName": "country",
    	"relatedContextId": "backPorchCountryRadio",
    	"relatedTrackId": "1369348557",
    	"values":[{"key": "episode", "value": "ep002"}],
		"tags": [ "FRONT","MALE","HOST" ]
	}
}
```
            
### Music Beds

Music beds are used underneath liners (when desired).

#### Generic Channel Music Beds

These are instrumental audio clips intended to be used across all types of experiences within the channel, as an audio layer that can be combined with a liners or used simply as stand alone. 

| Field | Value |
| --- | --- |
| assetType | "musicbeds" |
| id | [ contentId ] |
| collection | (optional) |
| relatedContextName | [ channel ] |

Example Generic Channel Music Bed:

```
{
	"data": {
		"assetType": "musicbeds", 
		"id": "5040801d94a24a0bb5ea743ab4c9926d", 
		"filename": "Country MX 4.wav",
		"relatedContextName": "country"
	}
}
```

#### Show-Specific Music Beds

These are instrumental audio clips intended to be used on only one specific show within Apple Music, as a layer that can be combined with a liner or used simply as stand alone. 

| Field | Value |
| --- | --- |
| assetType | "musicbeds" |
| id | [ contentId ] |
| collection | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |

Example Show-Specific Music Bed:

```
{
	"data": {
		"assetType": "musicbeds", 
		"id": "2d18565001a3420cb70065bff3e1ddae", 
		"filename": "Bed - Closing Talk Break_Into Not Your Girl.wav",
		"relatedContextName": "country",
    	"relatedContextId": "theTieraShow"
    }
}
```

### Long Form Content

These are episode specific longer form, pre-produced segments (e.g., a 15-minute interview with pre-produced music beds, branding, and musical excerpts).

| Field | Value |
| --- | --- |
| assetType | "content" |
| id | [ contentId ] |
| collection | (optional) |
| voice | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |
| values | "key": episode, "value": [ epXXX ] |
| values | "key": hours, "value": [ YYYYMMDDHH ] |
| tags | "MALE", "FEMALE" |

The key value pairs are used to indicate the following concepts:

* The episode and number combination is to be used if tagging a show.
* The hour and date combination is to be used if tagging a listening hour.

The tags above represent the following concepts:

* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

Example Long Form Content:


```
{
	"data": {
		"assetType": "content", 
		"id": "615cfaa1ce924d2ab22a20c974b55505", 
		"filename": "Nadeska Lil Nas X INT-.wav",
		"voice": "Nick Hoffman",
		"relatedContextName": "one",
    	"relatedContextId": "theNadeskaShow",
    	"values": [{"key": "episode", "value": "ep029"}], 
		"tags": [ "MALE" ]
	}
}
```         

## For Further Discussion

Now that all of the content encountered during the POC phase has been covered, there are many additional considerations and types of content that should be discussed. If Apple Music is looking to use any of the additional content types listed in this section, they should be incorporated into the tagging structure as it is being developed. 

### SFX

These are shorter instrumental sound clips with specific audio qualities that are intended to be used as catchy stand alone segues, or in concert with other assets.  

| Field | Value |
| --- | --- |
| assetType | "sfx" |
| id | [ contentId ] |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |
| values | "key": episode, "value": [ epXXX ] |
| values | "key": hours, "value": [ YYYYMMDDHH ] |
| tags | "BOOM", "WOOSH", "PEAK", "PLATEAU", "DONUT" |

The key value pairs are used to indicate the following concepts:

* The episode and number combination is to be used if tagging a show.
* The hour and date combination is to be used if tagging a listening hour.

The tags above represent the following concepts:

* BOOM: a required tag used to designate that the audio asset contains a type of bang or exploding noise.
* WOOSH: a required tag used to designate that the audio asset contains a swift sibilant sound representing a spurt of movement or flow.
* PEAK: a required tag used to designate that the audio asset begins with a low dB value, gradually rising until the playout of the audio file.
* PLATEAU: a required tag used to designate that the audio asset begins with a low dB value, increasing within the first 10-15% of playback, maintaining an increased dB level throughout the majority of playback, until decreasing volume in the back 10-15% of playback.
* DONUT: a required tag used to designate that the audio asset begins with a high value dB value, decreasing throughout the first 10-15% of playback, maintaining a decreased dB level throughout the majority of playback, until increasing volume in the back 10-15% of playback.

### Jingles/Sonic Logos

These are branding elements outside of interstitials openings, or show-specific branding elements that consist of broader branding aspects (e.g. If Apple Music has any specific "Apple Music" branded audio content).

| Field | Value |
| --- | --- |
| assetType | "interstitials" |
| id | [ contentId ] |
| metatype | "sonic" |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |
| values | "key": episode, "value": [ epXXX ] |
| tags | "SONIC" (required) |

This tag above represent the following concept:

* SONIC: a required tag used to designate that asset contains sonic branding audio characteristics.

### Seasonal/Promotional 

Holiday Music continues to be one of the largest audience and revenue drivers for the radio and streaming music industry. If Apple Music has any content for the upcoming holiday season up for consideration, or other promotional content that coincides with specific holidays, these can be added to the catalogue. (e.g. U2's Bono dropping a quick promotional "Happy St. Patrick's Day" spot to be played back throughout early March). Super Hi-Fi has a number of ways that these types of assets can be used for intelligent playback, from simple tagging to powerful CRON rules. 

## Conclusion / Next Steps

Our goal together is to come up with a tagging ontology that provides the right amount of descriptors that support the various ways that Apple Music utilizes and catalogs its content. Hopefully, this document served as a good first step. As was mentioned before, there will most likely be a few revisions required after the Apple Music Content team reviews it. After this review, the next step is to discuss the material contained herin and identify areas that might need additional simplification and/or elaboration.

----

*All Materials &copy;2021 Super Hi-Fi, Patent-Protected and Strictly Confidential*