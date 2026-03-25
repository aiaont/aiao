# Introduction

This directory provides the results of an exercise to model a project according to AIA, using an LLM. It is intended to serve as an example for other users of the AIA Ontology Suite who would like to use an LLM to assist them in the application thereof.

The LLM used for this exercise was OpenAI's ChatGPT 5.2.  

The versions of the AIA ontologies used were:

- aiao: v3.0.0 (Mar 26, 2026)
- impactont: v3.0.0 (Mar 26, 2026)
- claimont: v2.0.0 (Oct 21, 2025)
- infocomm: v2.0.0 (Oct 21, 2025)

The full conversation with the LLM can be found in the [chatgpt-chat-export.pdf](chatgpt-chat-export.pdf) file.

# The process

Below are the steps that we followed to initiate the conversation with the LLM. We recommend that other users follow these steps as well.

1. Prime the LLM with the AIA Ontology Suite. Example prompt:  

	> "I have some information about an impact project. I would like you to help me create AIA-annotated JSON-LD representations of the project and its impact claims. We'll do this step by step.
	> 
	> Here are the links to the four ontologies in the Anthropogenic Impact Accounting (AIA) suite:
	>   - https://aiaont.github.io/impactont/impactont.html
	>   - https://aiaont.github.io/claimont/claimont.html
	>   - https://aiaont.github.io/infocomm/infocomm.html
	>   - https://aiaont.github.io/aiao/aiao.html
	>
	>   As a first step, please confirm that you can access all four links and understand the content for each of the ontologies."

2. Feed the LLM some exemplary AIA models of other projects or activities. Example prompt:

   > "Next, here is a link to a guide written by humans for humans on how to annotate their impact project information/data with AIA:
   > https://github.com/aiaont/aiao/tree/main/examples/1-gs3492  
   > Please read through the guide and let me know if anything is unclear."
   
   Note that you can also now share the example in this directory (based on EcoRegistry Project 126) with the LLM.

3. Provide the LLM with the information or data for which you would like to create an AIA model. Example prompt:  
	> "Attached is the data and information that I have about the impact project. Before we proceed, please confirm that you can access all the attached documents."

4. Ask the LLM to provide a short, high-level summary of your project information/data  using AIA vocabulary. The purpose of this step is to ensure that the LLM understands exactly what your project is about and what the impact targets of the project are, i.e., what the "things" (typically environments) are that the project tries to have an impact on. Example prompt:  
   >"Please now provide a high-level summary of the project, using AIA vocabulary. Be sure to describe the primary agents and activities, and - most importantly - the 'things' for which states are quantified to measure the impact of the project."
   >
   >Review the LLM's response to this question carefully and only proceed once it is clear that your LLM has an accurate understanding of the project. (You will see in the chatgpt-chat-export.pdf file that we did not initially have this step in our conversation with the LLM and that it had negative consequences. We therefore strongly recommend that you include this step in your "LLM priming" recipe.)

Your LLM should now be ready to start constructing the AIA model of your data, under your supervision. We strongly suggest you let the LLM build the model incrementally, i.e., model some part or aspect thereof, present it to you for your review, and only proceed with the next part or aspect once you are satisfied with what its most recent output.

# Changes made to LLM's final model

Below is a list of changes that we manually applied to the LLM's final output, after careful review:

1. Added `impactont:TemporalLocation` to all the `time:Instant` and `time:Interval` nodes.
2. The LLM described the baseline activity as a real activity, but it was actually counterfactual, so we changed the modality of the baseline activity from "real" to "counterfactual," and also renamed its node from "gridEmissions-baseline-verifiedPeriod-real" to "gridEmissions-baseline-verifiedPeriod-counterfactual."
3. Simplified occurrences of

   ```
   "@type": [
       "aiao:Activity",
        "impactont:Event"
   ]
   ```

   to just

   ```
   "@type": "aiao:Activity"
   ```

   Reason: An `aiao:Activity` is explicitly defined as a subclass of `impactont:Event`; it is therefore not necessary to add `impactont:Event` to an instance's type if we have already declared it as an instance of `aiao:Activity`.

4. Renamed "verifiedPeriod" to "monitoringPeriod" (to align with industry jargon).
5. Renamed the "issuance-2024-03-18" node to "issuance1."
6. Renamed the crediting period node from "creditingPeriod\_<dates>" to "creditingPeriod1."
7. Replaced `dcterms:conformsTo` with `aiao:isGovernedBy`.
8. Listed the validated PDD as a control governing the project activity.
9. Rearranged the nodes in the graph.

The [aia-model.jsonld](aia-model.jsonld) file in this directory reflects the changes described above.

# Share your results

If you used an LLM to create an AIA model of your impact data, we would love to know about it! You can share your process and results with us by creating a pull request on GitHub for adding it to our directory of examples; alternatively, you can share it with us on our [LFDT wiki](https://lf-hyperledger.atlassian.net/wiki/spaces/CASIG/pages/828047391/Community+registry+of+annotated+data).
