## Super Hi-Fi 
# Apple Music Content Tagging Proposal

*All Materials &copy;2021 Super Hi-Fi, Patent-Protected and Strictly Confidential*

**Revision Date**: 12 August 2021

## Overview

This document provides content tagging recommendations for Apple Music's non-music assets. This tagging structure aims to optimize the usability of Apple Music's content within the Super Hi-Fi platform while at the same time minimize the amount of work required in order to perform this tagging. It was built using a sampling of Apple Music content that was gathered and reviewed by Super Hi-Fi during our proof-of-concept period. Please consider the document a first draft that may require a few revisions as more is learned about Apple Music's content and ways in which the content is utilized.

There are three main goals of this document:

1. Review the Common Features across all content and provide a content organization scheme that provides multi-level associations.

2. Enumerate each content type and associated sub-types alongside clear tagging recommendations for each incarnation.

3. Identify key areas for further discussion.

## Common Features

There are many commonalities across Apple Music's non-music assets, by using these identifiers within the content organization structure, assets can be tagged in a manner for the AI to calculate optimal playback relationships. This section provides a walk through of certain required fields, unique identifiers and content types, and other industry standard radio programming concepts and categories to tag and group assets with.  

### Identifiers

An identifier is the unique name that labels a specific content asset. As a requirement, every content asset must have a unique identifier that should be issued by a central authority and unique to Apple’s content catalogue. The identifier must satisfy the following regular expression `/^[0-9A-Za-z.\-_]+$/` which means using a combination of alphanumeric characters, dots, dashes, and underscores. No identifier should be reused except for when an update of a new version of an asset occurs. (e.g. if there was too much reverb on the asset, and a remixed asset is swapped out)

#### Content Types 
Audio content you produce to brand listening experiences.
- Interstitial
- Liner
- Music Bed 
- Content

####Content Organization Scheme

#####Channel
The primary property that best describes a group audio assets. 
- country
- one
- hits
- unassigned

#####Experience Type
The style of the programming asset.  
- pla (Playlist)
- anc (Anchor)
- alp (ALP - artist led programming)
- lbk (Lean Back)

#####Show or DJ Hour
The name of the program or show. 
- 80sRadioWithHueyLewis
- hunkerDownRadio
- jaydeDonovan
- appleMusicHits

#####Episode / Listening Hour
The number of the occurrence or installment of the show, or the timestamp of the hourly programming. 
- Episode: epXXX (Pure Throwback - ep225)
- Listening Hour: YYYYMMDDHH (2021081013)

#####Collection
A value that groups pieces of content into groups that represent equivalent assets. Two or more sweepers that have subtle yet intentional variations to one another are considered a collection of liners. Examples:
- “Backporch Radio, on Apple Country”
- “This is Apple Country, and Backporch Radio” 

Both example assets have slightly different station identifiers, but say essentially the same thing. If placed in the same collection, the AI will know that these two assets should not play in close proximity to one another.
          
#####Voice
The attribute to use for including the talent name associated with the asset. Labeling the asset with voice tags will allow for easier asset search and management in the future.
        
## Content Types

### Interstitials

More fully produced; designed to air stand-alone.

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

Example Opener interstitials:

```
{	"data": {
		"assetType": "interstitials", 
		"id": "f3c1d42f41a3436cb2faa58c2de9c84d", 
		"filename": "OPEN-BPCR-VERSION 2 REVISED-NO BED-EP AND MB.wav",
		"metatype": "bookends",
		"collection": "BPCR004",
		"voice": "EP and MB",
		"relatedContextName": "country",
    	"relatedContextId": "backPorchCountryRadio",
		"tags": 
		[ 
			"OPEN", 
			"BED" 
		]
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

* CLOSE: a required tag that indicates that the interstitials is closed.


```
{	"data": {
		"assetType": "interstitials", 
		"id": "88fc6b324fdB4e8abfb12f19b70848cb", 
		"filename": "IMAGING CLOSE EP146.wav",
		"metatype": "bookends",
		"relatedContextName": "hits",
    	"relatedContextId": "rockClassics",
		"tags": 
		[ 
			"OPEN"  
		]
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

* GENERIC: a required tag that indicates that the interstitials is generic for the entire channel.
* DROP: an optional tag used to notate "one-shot" style content (a short word, phrase, and/or sound effect designed to be placed tightly between two tracks).
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

Example Generic Channel-Branded interstitials:

```
{	"data": {
		"assetType": "interstitials", 
		"id": "4054f1f86b574467b745ac4fd5eec18b", 
		"filename": "AM1-Generic-SWP 11 (Clean).wav",
		"metatype": "branding",
		"collection": "AM1-SWP11",
		"voice": "Amber",
		"relatedContextName": "one",
		"tags": 
		[ 
		"GENERIC", 
		"FEMALE" 
		]
	}
}
```
            
#### Show Specific Interstitials

These are fully produced Interstitials that are intended to be used across one specific show on Apple Music (e.g. Rock Classics). 

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

* SPECIFIC: a required tag that indicates that the interstitials is specific for the show.
* SWEEPER: an optional tag used to indicate that the asset style is a sweeper (e.g. a brief promotional or branding segue)
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

```
{	"data": {
		"assetType": "interstitials", 
		"id": "a00d777f50c5419c839880b0ffaa71ee", 
		"filename": "SWP-BPCR-YOU'RE ON-PRODUCED-EP ONLY.wav",
		"metatype": "branding",
		"collection": "BPCR012",
		"voice": "Ep",
		"relatedContextName": "country",
    	"relatedContextId": "backPorchCountryRadio",
		"tags": 
		[ 
			"SPECIFIC", 
			"FEMALE" 
		]
	}
}
```

### Liner

Dry voice audio only with no effects.

#### Generic Channel Liner

These are dry voice audio assets only with no effects intended to be used across all types of experiences within the channel (e.g. Apple Music Country). 

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

* GENERIC: a required tag that indicates that the interstitials is generic for the entire channel.
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.
* SWEEPER: an optional tag used to indicate that the asset style is a sweeper (e.g. a brief promotional or branding segue)

```
{	"data": {
		"assetType": "liners", 
		"id": "da8cae3a48364893abee66d436f638f6", 
		"filename": "AMC-Quick SWP 8 DRY - 051420.wav",
		"metatype": "branding",
		"collection": "AMC012",
		"voice": "Ep",
		"relatedContextName": "country",
		"tags": 
		[ 
			"GENERIC", 
			"FEMALE", 
			"SWEEPER" 
		]
	}
}
```

#### Show Specific Liners

These are dry voice audio assets only with no effects intended to be used across one specific show on Apple Music (e.g. Rock Classics). 

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

* SPECIFIC: a required tag that indicates that the interstitials is specific for the show.
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

```
{	"data": {
		"assetType": "liners", 
		"id": "c4a3a57811c94e38b021d7466745d062", 
		"filename": "1 OUT OF FEEL IT STILL INTO SMILE THROWBACK EP 225_AMH.wav",
		"metatype": "branding",
		"voice": "Nicole Sky",
		"relatedContextName": "hits",
    	"relatedContextId": "pureThrowback",
		"tags": 
		[ 
			"SPECIFIC",  
			"FEMALE" 
		]
	}
}
```

#### Episode-Specific (or Time-Specific) Placed Voicetracks

These are dry voice audio assets only with no effects intended to be used on one specific episode of a show on Apple Music (e.g. Rock Classics). These can also be targeted to a specific hour of a show, or playlist if the duration of the show exceeds one hour.   

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
| tags | "BACK" (required), "FRONT (required)", "MALE", "FEMALE" |

The key value pairs are used to indicate the following concepts:

* The episode and number combination is to be used if tagging a show.
* The hour and date combination is to be used if tagging a listening hour.

The tags above represent the following concepts:

* BACK: a required tag if the related track identifier refers to the previous track prior to the liners.
* FRONT: a required tag if the related track identifier refers to the next track after the liners. 
* MALE: an optional tag used to designate that a male voice is present on the content.
* FEMALE: an optional tag used to designate that a a female voice is present on the content.

```
{	"data": {
		"assetType": "liners", 
		"id": "77e1303cf09440f885ff0e08d8b8d6f2", 
		"filename": "1 Into Dierks Bentley Living.wav",
		"metatype": "voicetrack",
		"voice": "Nick Hoffman",
		"relatedContextName": "country",
    	"relatedContextId": "backPorchCountryRadio",
    	"relatedTrackId": "1369348557",
    	"values":
    	[
    		{
    			"key": "episode", 
    			"value": "ep002" 
    		}
    	],
		"tags": 
		[ 
			"FRONT", 
			"MALE", 
			"HOST" 
		]
	}
}
```
            
### Music Beds

Instrumentals to play under dry voice content (when there is no other music nearby that can accommodate the overlay of voiceover).

#### Generic Channel Music Beds

These are instrumental audio clips intended to be used across all types of experiences within the channel, as an audio layer that can be combined with a liners, or used simply as stand alone. 

| Field | Value |
| --- | --- |
| assetType | "musicbeds" |
| id | [ contentId ] |
| collection | (optional) |
| relatedContextName | [ channel ] |

```
{	"data": {
		"assetType": "musicbeds", 
		"id": "5040801d94a24a0bb5ea743ab4c9926d", 
		"filename": "Country MX 4.wav",
		"relatedContextName": "country"
	}
}
```

#### Show-Specific Music Beds

These are instrumental audio clips intended to be used on only one specific show within Apple Music, as a layer that can be combined with a liners, or used simply as stand alone. 

| Field | Value |
| --- | --- |
| assetType | "musicbeds" |
| id | [ contentId ] |
| collection | (optional) |
| relatedContextName | [ channel ] |
| relatedContextId | [ show ] |

```
{	"data": {
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

```
{	"data": {
		"assetType": "content", 
		"id": "615cfaa1ce924d2ab22a20c974b55505", 
		"filename": "Nadeska Lil Nas X INT-.wav",
		"voice": "Nick Hoffman",
		"relatedContextName": "one",
    	"relatedContextId": "theNadeskaShow",
    	"values":
		[
			{
				"key": "episode", 
				"value": "ep029"
			}
		], 
		"tags": 
		[ 
			"MALE" 
		]
	}
}
```         

## For Further Discussion

We have covered all of the content that was encountered during the proof-of-concept period. There are many additional considerations, use cases and types of content that can be further explored. If Apple Music is looking to use any of the additional content types listed in this section, we can discuss how these content types can best be accommodated for relational playback purposes. 

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

These are branding elements outside of interstitials openings, or show specific branding elements that consist of broader branding aspects (e.g. If Apple Music has any specific "Apple Music" branded audio content).

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

Christmas continues to be one of the largest audience and revenue drivers for the radio and streaming music industry. If Apple Music has any content for the Christmas season up for consideration, or other promotional content that coincides with specific holidays, these can be added to the catalogue. (e.g. U2's Bono dropping a quick promotional "Happy St. Patrick's Day" spot to be played back throughout early March). We have a number of ways that these types of assets can be used for intelligent playback, from simple tagging to powerful CRON rules. 

## Conclusion

After reading through this content tagging recommendation for non-music assets document, The Apple Music Team should be better equipped with an understanding of the content structure and tagging construct of the Super Hi-Fi audio ingestion process. If there are questions or additions that you feel can be added to the reference document, please be in touch and we can certainly continue to update this document. 




