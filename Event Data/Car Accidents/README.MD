# Crash Stats in Victoria



<!-- MarkdownTOC -->

1. [Download](#download)
1. [Introduction](#introduction)
1. [Origin of the Data](#origin-of-the-data)
1. [Some questions to get started with](#some-questions-to-get-started-with)
1. [Data File Descriptions](#data-file-descriptions)
	1. [ACCIDENT.csv](#accidentcsv)
	1. [ACCIDENT_LOCATION.csv](#accident_locationcsv)
	1. [NODE.csv](#nodecsv)
	1. [ACCIDENT_EVENT.csv](#accident_eventcsv)
	1. [ATMOSPHERIC_COND.csv](#atmospheric_condcsv)
	1. [ROAD_SURFACE_COND.csv](#road_surface_condcsv)
	1. [SUBDCA.csv](#subdcacsv)
	1. [PERSON.csv](#personcsv)
	1. [VEHICLE.csv](#vehiclecsv)
	1. [ACCIDENT_CHAINAGE.csv](#accident_chainagecsv)

<!-- /MarkdownTOC -->


## Download

[Access and dowload data here on figshare](https://figshare.com/s/4fef11e99fcedba2b9dd)


## Introduction

* This dataset covers car accidents in Victoria from 2006 to August 2017.

* For each accident, there is data on such things as:
	* the vehicle make and model
	* the age and gender
	* the severity of the injuries 
	* location
	* weather conditions
	* ... and much more


* The dataset comprises multiple files which are bound together through shared keys (discussed below)

* It is a relatively large and rich dataset.
	* But, as it is made up multiple datasets, the complexity of your analysis depends very much on how deep you go and how many of the individual datasets you choose to cross-reference.
	* The major challenges will be:
		* Scoping out how data you have on what and how you can use it all
		* Cross referencing, when you choose to do so
		* Decoding a some of the data, which uses alpha-numeric codes to provide descriptions of accidents and conditions, etc.  The meaning of these codes is provided in the accompanying documentation (See [Data File Descriptions](#data-file-descriptions))
	* Though the data is quite clean, and much of it is described in quite some detail in the accompanying documentation, beware that there are some columns in the dataset that are not clear or described.  Just ignore them, there's plenty of other data.


## Origin of the Data

* The data is sourced from the VicRoads Accident Database.
	* See the included user guide (_crashstats_user_guide_and_appendices.pdf_) for more information
	* It has been generously compiled and provided to us by [Jason Thompson](https://msd.unimelb.edu.au/people/jason-thompson) from the _Melbourne School of Design_

* To be included in the data, an accident had to meet the following conditions (see the user guide for more information):
	* Occurred on any road or public place
	* The accident was reported to the Police
	* As a result, a person either died within 30 days or suffered personal injury 


## Some questions to get started with

* Which gender has more accidents
* In what way were they involved in the accident
* How severe were their injuries
* What kinds of vehicles are most involved in accidents


## Data File Descriptions

For a full descriptive reference of the data, including what each of the columns represents and how their contents are coded, please see the included __crashstats_data_appendix.pdf__.

This will be pivotal for any deep analysis of the data.
 
#### ACCIDENT.csv

* Main data set
* Basic details on each accident, such as:
	* Time
	* Descriptive details of the accident
* Unique Key for each accident - ACCIDENT_NO
	* Used through all of the datasets (except ACCIDENT_CHAINAGE, which is probably the least usable dataset by far)
	* Thus, all of the data can be cross-referenced.



#### ACCIDENT_LOCATION.csv

* Location of accidents in terms of street name and intersection


#### NODE.csv

* Probably more useful than ACCIDENT_LOCATION
* Location of accidents in terms of:
	* Postcode
	* Suburb (Local Government Location)
	* Longitude and Latitude (nb not describe in the reference appendix)


#### ACCIDENT_EVENT.csv

* Describes sequence of events involved in the accident
* ACCIDENT_NO is the unique key that binds each event to the single accident.
* Also included references to vehicles and people (see PERSON.csv and VEHICLE.csv)


#### ATMOSPHERIC_COND.csv

* Basic description of atmospheric conditions for each accident
* Accidents identified through ACCIDENT_NO
* Description for each code is in accompanying column, as well as under "atmosph_cond" in the __crashstats_data_appendix.pdf__


#### ROAD_SURFACE_COND.csv

* Similar to ATMOSPHERIC_COND.csv - provides basic description of road surface for each each accident.
* Description for codes are in accompanying table as well as under "surface_cond" in the __crashstats_data_appendix.pdf__



#### SUBDCA.csv

* Provides information additional to a specific column in ACCIDENT.csv
* In ACCIDENT.CSV, the column "DCA_CODE" provides information on the general nature of the accident.
	* The meaning of these codes is outlined in Appendix C of the _crashstats_user_guide_and_appendices.pdf_ 
	* It is also described in the accompanying column "DCA Description"
* This dataset, provides supplementary information, in addition to the DCA Code.
* For each accident, a single or multiple code is supplied.
	* The accompanying "Sub Dca Code Desc" provides a description
	* Otherwise, the entry for "Sub DCA codes" in the __crashstats_data_appendix.pdf__ provides a full outline.
* These are bound together with ACCIDENT_NO
* Where there are multiple events for which supplementary information is being provided, the "SUB_DCA_SEQ" column indicates the sequence of events for the accident.


#### PERSON.csv

* Personal information on people involved in accidents
* Information on role in the accident
* __PERSON_ID__ 
	* Unique key for each person involved in each accident
	* Linked to an accident through ACCIDENT_NO.
	* VEHICLE_ID (see VEHICLE.csv) is unique in the same way
	* Using, unique IDs for each person and vehicle in each accident together allows you to identify connect a specific person to a specific car.


#### VEHICLE.csv

* Information on vehicles involved in each accident
* VEHICLE_ID is unique to each vehicle involved in each accident
	* Linked to an accident through ACCIDENT_NO.
* May contain some incomplete or data lacking description.


#### ACCIDENT_CHAINAGE.csv

* Not understood by author of this document at time of writing
* Something to do with identifying the specific road route vehicles were taking at the time of the accident.
* The detailed documentation included here has more if you're interested.