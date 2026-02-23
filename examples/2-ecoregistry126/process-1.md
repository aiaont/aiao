# Step 1
Provide the definitions of all classes, properties and relationships in the ontologies to the LLM.

Pompt: "I have some information about an impact project. I would like you to help me make sense of the information, by using the Anthropogenic Impact Accounting (AIA) ontology suite. We'll do this step by step."

"Here are the definitions, along with some comments, of all the classes and properties in each of the ontologies in the AIA suite:"
...
...
..."

Hm, should we really do this dump up-front?

# Step 2

Provide worked out example to LLM.

# Step 3
Ask the LLM to answer the following questions:

1. Please confirm that you can access all the provided sources.

2. Prompt: 'As the first step, please make a list of all the agents involved in the project. AIA defines an "agent" as follows: "A thing that bears some form of accountability for the occurrence of another thing." AIA provides the following elaboration on the "agent" class: "This class includes all kinds of human and human-comprised agents, such as natural persons, legal persons and cyber personas." Please make sure to include all agents that you can identify, even if they appear only once in the documents. Please provide their full names or titles, as they appear in the documents; i.e., do not abbreviate any names or titles.'

3. Thank you. As the next step, please now identify how each agent is related to the project. If the documents provide a clear label or descriptor for an agent's role, please use that unmodified. If, however, you cannot unambiguously infer from the documents how an agent is related to the project, please indicate so clearly by stating, "Role uncertain." Do not invent or guess roles. As a reminder, here is the AIA definition for "Role": "A control that specifies the scope of and/or requirements for an agent's relation to an activity."

4. Thank you. Next, please identify all the activities that comprised the project. An "activity" is defined by AIA as, "An event that is orchestrated by an agent." "Agent" in that definition is as per its AIA definition, while the definition of "event" is given as, "A change in the state of a thing." "Thing" here can be literally any conceivable thing. In other words, an "activity" is something that an agent does on purpose, and which affects the state of some (typically other) thing. "State" is defined as, "The condition of a thing at a specific point in time, expressed in terms of one or more indicator-value pairs." For now, please do not try to describe the states or the indicators used for them; simply focus on identifying all activities. Only list activities explicitly identified in the documents. Do not invent any activities.

5. Excellent. Let us now focus on the objectives of the identified activities. AIA defines an objective as, "A desired state to be effected by an activity." Please identify the objective(s) of each of the primary intervention activities, as stated in the documents. If you cannot find a clearly formulated or stated objective(s) for an activity, please do not guess or invent an objective; simply then state, "Objective uncertain." As a reminder, AIA's definition of objective is, "A desired state to be effected by an activity."

6. Thank you. AIA defines an indicator as, "A convention for indicating the state of a thing." Please now identify all the indicators used to describe or measure the impact of the project and its activities. For each indicator provide its definition and unit of measure. Only list those indicators that are explicitly mentioned or referenced in the documents. If the documents provide an explicit definition for an indicator, use that as the indicator's definition; otherwise simply state, "No clear definition provided" for that indicator. If the documents provide the unit of measure for an indicator, please use that verbatim as its unit of measure; if no unit of measure is provided for an indicator, simply state, "No unit of measure stated." 

7. Good, thank you. As previously said, AIA defines "state" as, "The condition of a thing at a specific point in time, expressed in terms of one or more indicator-value pairs." Please now identify all the things for which states were explicitly recorded or measured by the project and/or its activities. Please don't make any guesses or assumptions about whether the state for some thing was or should have been recorded/measured - if the documents do not explicitly indicate that such a state were recorded/measured, please don't include it in your result. 
Consider asking the LLM to present its results in the order of thing -> state -> indicator.

# Step 3
Feed the LLM its own answers and then ask it to create JSON-LD (or OWL?) representations to describe the activity and its impact according to the answers that it has extracted.
