# Introduction

This guide will take you through the process of manually (i.e., without AI assistance) creating an AIA-annotated JSON-LD representation of an impact project and its impact claims. Although we'll try to be as beginner-friendly as possible, some prior knowledge of JSON-LD and semantic web conventions will likely be beneficial.

This example is loosely based on Gold Standard project 3492, but the result will not be a perfect 1:1 representation of the project - not all the project documents and data are publicly available, so we'll be making some information up. 

The latest versions of the ontologies in the AIA suite at the time of writing were:  
- Anthropogenic Impact Accounting Ontology (aiao): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/aiao), [HTML](https://aiaont.github.io/aiao/aiao.html))  
- Impact Ontology (impactont): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/impactont), [HTML](https://aiaont.github.io/impactont/impactont.html))  
- Claim Ontology (claimont): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/claimont), [HTML](https://aiaont.github.io/impactont/impactont.html))  
- Information Communication Ontology (infocomm): v2.0.0 (Oct 21, 2025) ([GitHub](https://github.com/aiaont/infocomm), [HTML](https://aiaont.github.io/infocomm/infocomm.html))

Note that this example will not concern itself with the communication (and hence replication) of the resulting impact claim across multiple communication channels, and will therefore not make use of the Information Communication Ontology.

# Project Overview

Gold Standard project 3492 was a programme of activities undertaken by International Paint Ltd. with the goal of reducing CO2 emissions from shipping. The mechanism of intervention was the application of advanced hull coatings to the ships to reduce their hydrodynamic drag, thereby reducing their fuel consumption and consequently their CO2 emissions. At the time of writing the publicly accessible information about the project was located at https://registry.goldstandard.org/projects/details/605, while a summary in the form of a press release about the project was available [here](https://rhmanager.preventionatsea.com/index.php?artcat=6&artid=297&p=Article).

For the purposes of this example, we shall focus on the first phase of the project which included only 16 ships.

# Creating the AIA-annotated JSON-LD representations

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

Seeing that impact accounting is all about impact and who was responsible for that impact, our most sensible next steps are to determine what the primary impact goals of the project were, and who the primary agents were. We'll start with the impact goals.

According to the Project Design Document (PDD), the primary impact goal of the project was to reduce the fuel consumption of ships through the application of advanced hull coatings. In AIA terminology a goal is called an "objective," so let us now create a JSON-LD node for the project that indicates its objective:

```
	{
	  "@id": "myns:gs3492",
	  "@type": "aiao:Project",
	  "aiao:hasObjective": "Reduce the fuel consumption of ships through the application of advanced hull coatings."
	}
```

Note that the node identifiers that we'll be using for JSON-LD nodes throughout this example are not true IRIs. As per W3C recommendation, the value assigned to the "@id" property of a JSON-LD node should be a true, functional IRI. For the purposes of keeping this example as human-readible and as simple as possible, we'll be using word-like identifiers (e.g., "gs3492" and "gs3492-activity1") instead of UUIDs (e.g., "61f0c404-5cb3-11e7-907b-a6006ad3dba0") or something similar.

The project documentation identifies International Paint Ltd. as the project proponent, and therefore accountable for the project's impact. We'll create a JSON-LD representation for International Paint Ltd. as an aiao:Agent like this...

```
{
      "@id": "myns:internationalPaintLtd",
      "@type": "aiao:Agent"
}
```
... and a JSON-LD representation for the role of "Project Proponent" like this:

```
{
      "@id": "myns:role-projectProponent",
      "@type": "aiao:Role"
}
```

Then we'll explicitly link International Paint Ltd. to the project proponent role within the context of the GS 3492 project using an agent-activity relation:

```
    {
      "@id": "myns:gs3492-agentActivityRel1",
      "@type": "aiao:AgentActivityRelation",
      "aiao:hasActivity": {
        "@id": "/gs3492"
      },
      "aiao:hasAgent": {
        "@id": "/internationalPaintLtd"
      },
      "aiao:isGovernedBy": {
        "@id": "/role-projectProponent"
      }
    }
```

Admittedly, our aiao:Agent node for International Paint Ltd. isn't very descriptive in its current state. It seems like, if it had not been for the word-like identifiers that we're using in this guide, there would have been no way for a reader to tell that the aiao:Agent node represented International Paint Ltd. This, however, is exactly why a real-life use case would use proper, universal IRIs as node identifiers - those IRIs will typically point to official registers in which a lot of identifying information (legal entity name, registration number, etc.) is recorded for each identifier. Where such a register is not available, as much identifying information as necessary should be added to the aiao:Agent node itself. To keep things simple for the purposes of this guide, we shall leave our aiao:Agent node as is.

So far we have answers to the "why" and "who" questions of the project. Let us now try to anchor the project in space and time so that we can answer the "when" and "where" questions too. 

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

For this specific project, finding an answer to the "where" question might seem rather tricky. The "where" that we are most interested in in the context of impact accounting is the "where" of the impact - i.e., which environment was affected by the impact? The PDD states, "Since projects within the PoA comprise shipping, often international, there are no geographic boundaries as such. The PoA boundary potentially includes all shipping in the world." Furthermore, the project is primarily concerned with reducing the amount of atmospheric CO2 attributable to shipping. Seeing that atmospheric CO2 knows no geographical boundries, the environment of impact for the project is really the whole of Earth's atmosphere. 

It is, however, rather difficult (if not impossible - at least at present) to attribute state changes in global atmospheric CO2 concentrations to individual impact projects, so what GHG impact projects typically do instead is to measure state changes at the GHG sources affected by their interventions. In the case of our example, those sources are the ships that received advanced hull coatings. Let us therefore now proceed to creating a JSON-LD representation for each of the 16 ships in the initial phase of the project. According to the Impact Ontology, each ship is simply a "Thing," so the JSON-LD for the first ship will look like this:

```
    {
      "@id": "myns:ship9xxxx14-86366",
      "@type": "impactont:Thing"
    }
```

In the interest of brevity we shall not show the JSON-LD representations for each of the remaining 15 ships here. Their JSON-LD representations can all be found in the aiao_example.jsonld file that accompanies this guide.

Seeing that the ships were not stationary sources, but travelled across the globe in international waters, it still leaves us with no geographical boundaries for the project. It is therefore best not to try and record a specific project or impact location.

Our next question is the "what", i.e., what states of the ships were targeted by the project interventions? Essentially, the project was concerned with how polluting each vessel was, but states in AIA are described through indicators, so to answer the "what" question, we need to identify the indicators that were used to describe the states of the vessels. This information can be found in the PDD, where the indicator of primary concern is identified as, "CO2 emissions from fossil fuel combustion in ship j during year y (tCO2/yr)." Let us therefore now create a JSON-LD representation for the indicator, so that we can subsequently use it to describe states:

```
    {
      "@id": "myns:ind-emissions-co2-ship",
      "@type": "impactont:Indicator",
	  "impactont:hasDefinition": "CO2 emissions from fossil fuel combustion in ship j during year y (tCO2/yr)",
      "impactont:hasUnitOfMeasure": "tCO2/yr"
    }
```

You may notice in the project documentation that this indicator was actually calculated from several other indicators. In a real-life accounting scenario, JSON-LD represenations should be created for those indicators alike, and for the states that they describe. In the interest of keeping the present example as simple as possible, we shall not do that here.

Now that we have the indicator represented, we can proceed with creating a JSON-LD representation for each of the states of interest. States in AIA should _always_ have clear temporal locations, i.e., it should always be clear exactly when a state occurred (or would have occurred). Unfortunately, the project information available to us does not give us the exact dates for each of the states that were quantified for each ship. We do know, however, that the project followed the convention of describing a baseline state (counterfactual) and a project state (real) for each ship; when following that convention, the baseline and project states have the same temporal location. Furthermore, states are typically described per monitoring period. We shall therefore apply some creativity and imagine that the first monitoring period for the first ship ran from 10 September 2014 to 9 September 2015. We can represent it in our JSON-LD graph as follows:

```
	{
	  "@id": "myns:gs3492-ship9xxxx14-86366-monitoringPeriod1",
	  "@type": "time:ProperInterval",
	  "time:hasBeginning": {
		"@type": "time:Instant",
		"time:inXSDDateTimeStamp": {
		  "@value": "2014-09-10T00:00:00Z",
		  "@type": "xsd:dateTimeStamp"
		}
	  },
	  "time:hasEnd": {
		"@type": "time:Instant",
		"time:inXSDDateTimeStamp": {
		  "@value": "2015-09-09T23:59:59Z",
		  "@type": "xsd:dateTimeStamp"
		}
	  }
	}
```

Then we create a baseline state node and a project state node for the first ship's first monitoring period like thus:

```
    {
      "@id": "myns:state-baseline-ship9xxxx14-86366-monper1",
      "@type": "impactont:State",
      "impactont:hasModality": "counterfactual",
      "impactont:hasTemporalLocation": {
		"@id": "/gs3492-ship9xxxx14-86366-monitoringPeriod1"
	  },
      "impactont:isDefinedByIndicator": {
        "@id": "/ind-emissions-co2-ship"
      }
    },

    {
      "@id": "myns:state-project-ship9xxxx14-86366-monper1",
      "@type": "impactont:State",
      "impactont:hasModality": "real",
      "impactont:hasTemporalLocation": {
		"@id": "/gs3492-ship9xxxx14-86366-monitoringPeriod1"
	  },
      "impactont:isDefinedByIndicator": {
        "@id": "/ind-emissions-co2-ship"
      }
    }
```

One thing that is still missing from these state nodes, though, is the actual results of the quantification of the states. Right now we know when the states occurred and we know that they are described in terms of the CO2 emissions from fossil fuel combustion of the ship during the ship's first monitoring period, but we have not assigned the actual "tCO2/yr" value to each state. This is where quantification methodologies become important. 

According to the project documentation, "Gold Standard methodology: Reducing Vessel Emissions Through the Use of Advanced Hull Coatings (version 2)" was used to calculate the project's impact. The methodology prescribes, among many other things, how to determine the "tCO2/yr" per ship and monitoring period. An activity's or project's choice of methodology is of critical importance in impact accounting, as different methodologies often exist for the same indicator, but with different results. AIA therefore strongly recommends that all quantified states be clearly linked to the methodologies according to which they were determined (measured/calculated).

A methodology in AIA is a type of control, so we'll now create an aiao:Control node for the project's chosen methodology:

```
{
    "@id": "myns:meth-ind-emissions-co2-ship",
    "@type": "aiao:Control",
    "terms:title": "Gold Standard methodology: Reducing Vessel Emissions Through the Use of Advanced Hull Coatings",
    "terms:hasVersion": 2
}
```

Wait. What are the 'terms:title' and 'terms:hasVersion' properties? Ah. Those are properties from the [DCMI Metadata Terms](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/) that we are using here to add extra information to our methodology's node. They are completely optional, but very handy. To use them like this, however, we do need to remember to add a namespace declaration for DCMI Metadata Terms to our "@context" node:

```
    "@context": {
      ...
      "xsd": "http://www.w3.org/2001/XMLSchema#",
      "terms": "http://purl.org/dc/terms/"
    }
```

To connect a state value to the methodology according to which it was determined, we need to create a representation for the determination activity, i.e., the activity through which the state value was measured or calculated, as per the methodology. According to our project's documentation, though, the calculations were not performed by International Paint Ltd. themselves, but by MGM Innova as an external consultant. We'll therefore also have to create new aiao:Agent, aiao:Role, and aiao:AgentActivityRelation nodes to represent MGM Innova and their role in the project:

```
    {
      "@id": "myns:mgmInnova",
      "@type": "aiao:Agent"
    },		

    {
      "@id": "myns:role-externalConsultant1",
      "@type": "aiao:Role"
    },
	
    {
      "@id": "myns:gs3492-impactCalculation",
      "@type": "aiao:Activity",
      "aiao:hasObjective": "Calculate project impact according to GS methodology: Reducing Vessel Emissions Through the Use of Advanced Hull Coatings",  
      "aiao:isGovernedBy": "/meth-ind-emissions-co2-ship"
    },	
	
    {
      "@id": "myns:gs3492-agentActivityRel2",
      "@type": "aiao:AgentActivityRelation",
      "aiao:hasActivity": {
        "@id": "/gs3492-impactCalculation"
      },
      "aiao:hasAgent": {
        "@id": "/mgmInnova"
      },
      "aiao:isGovernedBy": {
        "@id": "/role-externalConsultant1"
      }
    }
```

Now we are ready to add the results of the state quantification activity to our state nodes to complete the state descriptions. We'll do so using the 'source' property from the DCMI Metadata Terms. Note that the actual quantified values are not provided in the project information available to us, so we'll just use the imaginary values of 119 000 for the baseline state and 101 002 for the project state:

```
    {
      "@id": "myns:state-baseline-ship9xxxx14-86366-monper1",
	  ...
      "impactont:hasIndicatorValue": {
        "@type": "xsd:decimal",
        "@value": "119000",
        "terms:source": {
          "@id": "/gs3492-impactCalculation"
        }
      },
	  ...
    },

    {
      "@id": "myns:state-project-ship9xxxx14-86366-monper1",
	  ...
      "impactont:hasIndicatorValue": {
        "@type": "xsd:decimal",
        "@value": "101002",
        "terms:source": {
          "@id": "/gs3492-impactCalculation"
        }
      },
	  ...
    }
```

Again, in the interest of keeping this guide digestible, we shall not provide the JSON-LD representations here for the monitoring periods, baseline states, and project states of each of the remaining 15 ships. Those representations can be found in the guide's accompanying aiao_example.jsonld file.

With the states sufficiently represented, we can now also represent the impact of the project on the first ship during its first monitoring period as follows:

```
    {
      "@id": "myns:impact-baselineVsProject-ship9xxxx14-86366-monper1",
      "@type": "impactont:Impact",
      "impactont:hasStateA": {
        "@id": "/state-baseline-ship9xxxx14-86366-monper1"
      },
      "impactont:hasStateB": {
        "@id": "/state-project-ship9xxxx14-86366-monper1"
      },
      "impactont:hasProvenance": {
        "@id": "/gs3492"
	  }
    }
```

Proper impact accounting, though, never accepts statements at face value, so let's package all these things as claims that can be verified or falsified, based on their substantiations.

The state claims will be substantiated by the voyage logs of the ships, so we can create a claimont:Substantiation node for the first ship's voyage log as follows:

```
    {
      "@id": "myns:voyageLog-ship9xxxx14-86366",
      "@type": "claimont:Substantiation"
    }
```

Then we can create an aiao:StateClaim node for the baseline state of the first ship during its first monitoring period like thus:

```
    {
      "@id": "myns:claim-state-baseline-ship9xxxx14-86366-monper1",
      "@type": "aiao:StateClaim",
      "claimont:hasClaimant": {
        "@id": "/internationalPaintLtd"
      },
      "claimont:hasSubject": {
        "@id": "/ship9xxxx14-86366"
      },	  
      "claimont:hasPredicate": "Had baseline state during monitoring period 1 of Gold Standard project 3492",
      "claimont:hasObject": {
        "@id": "/state-baseline-ship9xxxx14-86366-monper1"
      },
      "claimont:isSupportedBy": {
        "@id": "/voyageLog-ship9xxxx14-86366"
      }
    }
```

The node above reads, "International Paint Ltd. (the claimant) hereby claims that the baseline state of ship 'ship9xxxx14-86366' during its first monitoring period of its participation in Gold Standard project 3492 was as described by state node 'state-baseline-ship9xxxx14-86366-monper1'. This claim is substantiated by the voyage log for the ship (voyageLog-ship9xxxx14-86366)."

The claim for the ship's project state during the same period will be near-identical to its baseline state claim:

```
    {
      "@id": "myns:claim-state-project-ship9xxxx14-86366-monper1",
      "@type": "aiao:StateClaim",
      "claimont:hasClaimant": {
        "@id": "/internationalPaintLtd"
      },
      "claimont:hasSubject": {
        "@id": "/ship9xxxx14-86366"
      },
      "claimont:hasPredicate": "Had project state during monitoring period 1 of Gold Standard project 3492",
      "claimont:hasObject": {
        "@id": "/state-project-ship9xxxx14-86366-monper1"
      },	  
      "claimont:isSupportedBy": {
        "@id": "/voyageLog-ship9xxxx14-86366"
      }
    }
```

Finally, we can create an aiao:ImpactClaim node to represent International Paint Ltd.'s claim that they are accountable for (i.e., "deserves the credit for") the reduction in CO2 emissions between the first ship's baseline state and project state. This claim will be supported by the project documentation (PDD, monitoring reports, etc.), so we'll first create substantiation nodes for those:

```
    {
      "@id": "myns:gs3492-projectDesignDocument",
      "@type": "claimont:Substantiation"
    },
    {
      "@id": "myns:gs3492-monitoringReport1",
      "@type": "claimont:Substantiation"
    }	
```

Then we can create the impact claim's node:

```
    {
      "@id": "myns:claim-impact-project-ship9xxxx14-86366-monper1",
      "@type": "aiao:ImpactClaim",
      "claimont:hasClaimant": {
        "@id": "/internationalPaintLtd"
      },
      "claimont:hasSubject": {
        "@id": "/impact-baselineVsProject-ship9xxxx14-86366-monper1"
      },
      "claimont:hasPredicate": {"@id": "impactont:hasProvenance"},
      "claimont:hasObject": {
        "@id": "/gs3492"
      },
      "claimont:isSupportedBy": [
        {
          "@id": "/gs3492-projectDesignDocument"
        },
        {
          "@id": "/gs3492-monitoringReport1"
        }
      ]
    }
```

And with this, we now have AIA-annotated JSON-LD representations of our impact project data, ready to be ingested by a verification system or shared with other impact accounting and aggregation systems!