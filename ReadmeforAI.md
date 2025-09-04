# Readme for AI prompts
This document provides a structured background to lay out the parts and approaches to the Escapement data dictionary modeling project.

# Goals
The goal of this project is to disambiguate the multiple different kinds of escapement observation data in use by DFO (and other) salmon observers.

## Expected Outcomes
 - **Harmonization of Synonyms:**  Across all dictionaries, find terms that mean exactly the same thing and gather them into a common 'master' term. 
 - **Map between dictionaries:** Provide 1:1 translation between different dictionaries. 
 - **Disambiguate terms:**  Many terms in use are easy to misinterpret, leading to potential mis-use of the data. This project will make sure that the meanings of these terms is accessible to a new user.
 - **Classify terms:** Many terms may be similar, in that they belong to the same category of things, but are slightly different. The ontology will help define these groupings, and identify what information is needed to keep them distinct
 - **Metadata tags:** Some fields require additional Metadata in order to be used appropriately, or to fit into the ontology. we will identify these terms that need more metadata and determine what the additional metadata needs are.  
 - **Changes:** Where monitoring methods may change over time terms may change in meaning.  We will document these changes as part of the ontology. 

# Approach
We will use the knowledge modeling approach described in the github repo as a target.  however we will need to take several steps before this to prepare the data, develop a method for building the ontology, then scaling that method to new sources. We will also need to figure out how to grow from existing ontologies where appropriate. Lastly, we will need a way to seek and incorporate feedback from SMEs.

1. **Gather Data Dictionaries:** Focusing on escapement data, we pull together long term monitoring data sources and their data dictionaries. They are initially focused on Stream inspection methods such as bankwalks,  but we aim to eventually expand to other sources.  
2. **Format data dictionaries:** All dictionaries will need to be formatted with the same column names. 
3. **Re-use sections:** Some dictionaries have sections that separate out metadata from other components.  Will will try and retain these sections and apply them to other dictionaries where possible. 



# Input Data
The data being used in this case are data dictionaries from long-term stream inspection monitoring projects aimed at counting salmon that are returning to spawn in rivers around BC and the Yukon. Each dicitonary describes the fields used in a long-term monitoring series.   the different monitoring efforts often occur for different species in different contexts using different methods.  STream inspection log data are to be stored in a "SILs" folder, other inspection method types may be added later.

## Raw data
Original data dictionaries are stored together in a folder called "Data/SILs/Dictionaries_Raw"

## Formatted 
Formatted data dictionaries are stored together in a folder called "Data/SILs/Dictionaries_Formatted"

## References
Reference documents such as field guides are used to provide background context for the dictionaries.  These provide information on why and how different fields are collected in greater detail than many dictionaries. They are stored in a "Data/SILs/Field_Guides". other documents concernind broad information about escapement monitoring are 

## Cataloguing 
Dictionaries and their field guides will be described in an excel document.