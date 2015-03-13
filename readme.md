# AutoHotkey-HL7

#### An [AutoHotkey](http://ahkscript.org/) library/class to parse [HL7](http://en.wikipedia.org/wiki/Health_Level_7) files into structured Autohotkey objects.

Tested in AutoHotkey v1.1.16.05

License: [GPLv3](http://www.gnu.org/copyleft/gpl.html)

- - -

## Usage

Simply include HL7.ahk file somewhere in your script and invoke the HL7 class's parse() method:

````
; in most cases you will be working from an HL7 text file.
FileRead, HL7_Text, Sample_HL7.txt
Parsed_HL7 := HL7.parse(HL7_Text)
````
The parser is smart enough to detect and honor any specified encoding characters, even though most HL7 implementations do not deviate from the "suggested" values.

The HL7 class will parse your input and return an object of the following structure:

````
{
	"MSH" ; header
	[
		[ ; segment
			[ ; field
				[ ; "repeat"
					[ ; component
						[ ; subcomponent
							"Value"
						]
					]
				]
			]
		]
	]
	...
}
````

The input in the case above would have been:

````
MSH|Value
````

A simple sample script and HL7 message are included in the samples subdirectory.

- - -

## Implementation

Your message is returned as an object with each unique segment header as keys.  The values located at those keys are arrays of all segments which had that header.
Because HL7 messages can have any number of segments, with any number of fields, each of which may or may not have multiple repetitions, components, and subcomponents, the parser implies all levels no matter how many of each level are present.
This means that the actual "values" encoded in your HL7 message will be found in the most specific level possible: the subcomponent.
Fields that have no defined repeats, components, or subcomponents will therefore nontheless be returned as though they had one of each.
This structure is purposeful.  It is the only way to maintain a consistent and useful shape across the myriad of message structures made possibly by HL7's lax definition.

## Acknowledgements

The structure of the class is based largely on that of [cocobelgica's amazing JSON module](https://github.com/cocobelgica/AutoHotkey-JSON), of which I have made extensive use in my own personal projects for the last couple of years.

Everything I know about AutoHotkey has been absorbed from either the included help file or the amazing community at [ahkscript.org](http://ahkscript.org).  I've read hundreds of topics and replies by all of the all-star users over there...too numerous to recall.  Thanks to everyone who contributes to that awesome community!
