# Introduction

This guide will take you through the process of manually (i.e., without AI assistance) creating an AIA-annotated JSON-LD representation of an impact project and its impact claims. Although we'll try to be as beginner-friendly as possible, some prior knowledge of JSON-LD and semantic web conventions will likely be beneficial.

This example is loosely based on Gold Standard project 3492, but the result will not be a perfect, 1:1 representation of the project - not all the project documents and data are publicly available, so we'll be making some information up. At the time of writing the publicly accessible information about the project was located at https://registry.goldstandard.org/projects/details/605, while a summary in the form of a press release about the project was available [here](https://rhmanager.preventionatsea.com/index.php?artcat=6&artid=297&p=Article).

The latest versions of the ontologies in the AIA suite at the time of writing were:  
- Anthropogenic Impact Accounting Ontology (aiao): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/aiao), [HTML](https://aiaont.github.io/aiao/aiao.html))  
- Impact Ontology (impactont): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/impactont), [HTML](https://aiaont.github.io/impactont/impactont.html))  
- Claim Ontology (claimont): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/claimont), [HTML](https://aiaont.github.io/impactont/impactont.html))  
- Information Communication Ontology (infocomm): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/infocomm), [HTML](https://aiaont.github.io/infocomm/infocomm.html))

Note that this example will not concern itself with the communication (and hence replication) of the resulting impact claim across multiple communication channels, and will therefore not make use of the Information Communication Ontology.

# Project Overview

...
For the purposes of this example, we shall only focus on the first phase of the project which included only 16 ships.


# Creating the AIA-annotated JSON-LD representation

To set the stage for the rest of our work, let us start off by initialising our JSON-LD context. We already know that we'll be using the main AIA ontology, the Impact Ontology, and the Claim Ontology, so we can create our "@context" node with their namespace declarations inside:

```
	"@context": {
		"aiao": "http://w3id.org/aiao#",
		"claimont": "http://purl.org/claimont#",
		"impactont": "http://purl.org/impactont#"
	}
```

To make things easier for ourselves and for others, let's also add the path to our data domain to the context. This will allow us to use relative identifiers throughout our example (e.g., "/someid1" and ":someid1"), instead of having to provide the full domain path plus identifier each time (e.g., "http://mydataserverthatdoesnotexist.com/imaginarynamespace/someid1"). We do this using the "@base" key and a namespace declaration, and it is best to add those to the very top of the "@context" list:

```
	"@context": {
		"@base": "http://mydataserverthatdoesnotexist.com/imaginarynamespace/",
		"myns": "http://mydataserverthatdoesnotexist.com/imaginarynamespace/",
		"aiao": "http://w3id.org/aiao#",
		...
	}
```

Seeing that impact accounting is all about impact and the agents responsible for that impact, our most sensible next step is to determine what the primary impact goals of the project were. According to the Project Design Document (PDD), the primary impact goal of the project was to reduce the fuel consumption of ships through the application of advanced hull coatings. In AIA terminology a goal is called an "objective," so let us now create a JSON-LD node for the project that indicates its objective:

```
	{
	  "@id": "myns:gs3492",
	  "@type": "aiao:Project",
	  "aiao:hasObjective": "Reduce the fuel consumption of ships through the application of advanced hull coatings."
	}
```

Note that the IDs that we'll be using for JSON-LD nodes throughout this example are not true IRIs. As per W3C recommendation, the value assigned to the "@id" property of a JSON-LD node should be a true, functional IRI. For the purposes of keeping this example as human-readible and as simple as possible, we'll be using word-like identifiers (e.g, "gs3492" and "gs3492-activity1") instead of UUIDs (e.g., "61f0c404-5cb3-11e7-907b-a6006ad3dba0") or something similar.

The objective answers the "why" question of the project. Let us now try to anchor the project in space and time so that we can answer the "when" and "where" questions too. 

The project documentation states that the officially registered start date of the project is 1 January 2010, and that the duration of the project is 28 years. We can therefore add a clear start date and end date to our project's JSON-LD node. To do so, however, we'll use the OWL Time Ontology and XMLSchema, which means we first need to declare their namespaces in our "@context" node:

```
	"@context": {
		...
		"impactont": "http://purl.org/impactont#",
		"time": "http://www.w3.org/2006/time#",
		"xsd": "http://www.w3.org/2001/XMLSchema#"
	}
```

Now we can add the start date and the end date of our project to its JSON-LD node as follows:

```
	{
	  "@id": "myns:gs3492",
	  "@type": "aiao:Project",
	  "aiao:hasObjective": "Reduce the fuel consumption of ships through the application of advanced hull coatings.",
	  "time:hasBeginning": {
		"@type": "time:Instant",
		"time:inXSDDateTimeStamp": {
		  "@value": "2010-01-01T00:00:00Z",
		  "@type": "xsd:dateTimeStamp"
		}
	  },
	  "time:hasEnd": {
		"@type": "time:Instant",
		"time:inXSDDateTimeStamp": {
		  "@value": "2037-12-31T23:59:59Z",
		  "@type": "xsd:dateTimeStamp"
		}
	  }
	}
```

For this specific project, finding an answer to the "where" question might initially seem rather tricky. The "where" that we are most interested in in the context of impact accounting is the "where" of the impact - i.e., which environment was affected by the impact? The PDD states, "Since projects within the PoA comprise shipping, often international, there are no geographic boundaries as such. The PoA boundary potentially includes all shipping in the world." (TODO: Insert reference here.) Furthermore, the project is primarily concerned with reducing the amount of atmospheric CO2 attributable to shipping. Seeing that amospheric CO2 knows no geographical boundries, the environment of impact for the project is really the whole of Earth's atmosphere. 

It is, however, rather difficult (if not impossible - at least at present) to attribute state changes in global atmospheric CO2 concentrations to individual impact projects, so what GHG impact projects typically do instead is to measure state changes at the GHG sources affected by their interventions. In the case of our example, those sources are the ships that received advanced hull coatings. (((The ships, however, were not stationary sources, but travelled across the globe in international waters, which again leaves us with no geographical boundaries for the environment of impact - other than all of Earth's oceans navigable to large ships.))) Let us therefore now proceed to creating a JSON-LD representation for each of the 16 ships in the initial phase of the project. According to the Impact Ontology, each ship is simply a "Thing," so the JSON-LD for the first ship will look like this:

```
    {
      "@id": "myns:ship9xxxx14-86366",
      "@type": "impactont:Thing"
    }
```

In the interest of brevity we shall not show the JSON-LD representations for each of the remaining 15 ships here. Their JSON-LD representations can, however, all be found in the aiao_example.jsonld file that accompanies this guide.

This leads us to our next question, i.e., the "what" - what states of the ships were targeted by the project interventions? Essentially, the project was concerned with how polluting each vessel was, but states in AIA are described through indicators, so to answer the "what" question, we need to identify the indicators that were used to describe the states of the vessels. This information can be found in the PDD, where the indicator of primary concern is identified as, "CO2 emissions from fossil fuel combustion in ship j during year y (tCO2/yr)." Let us therefore now create a JSON-LD representation for the indicator, so that we can subsequently use it to define states:

```
    {
      "@id": "myns:ind-emissions-co2-ship",
      "@type": "impactont:Indicator",
	  "impactont:hasDefinition": "CO2 emissions from fossil fuel combustion in ship j during year y (tCO2/yr)",
      "impactont:hasUnitOfMeasure": "tCO2/yr"
    }
```

You may notice in the project documentation that this indicator was actually calculated from several other indicators. In a real-life accounting scenario, JSON-LD represenations should be created for those indicators alike as well as for the states that they describe. In the interest of keeping the present example as simple as possible, we shall not do that here.

Now that we have the indicator represented, we can proceed with creating JSON-LD representations for each of the states that it described.


