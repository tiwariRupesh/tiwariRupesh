**RRV BATCH JOB FLOW**

**MS-DVS Module -\>**

RRV Controller :- It has couple of end points ,where in we can initiate
batch job ,persists data into below mentioned tables and checks batch job processing status 

DVS_BATCH -> It stores batch id , batch name , batch creation date .

renewal batch -> It has batch relates entries , it stores these entries
ifsc file , eqifax file , nmac file name ,ifsc_batch status, Equifax
batch status , processor id , smec batch status etc .

renewal_households -> It captures household id,household income , batch id , ifsc file name
, Equifax file name , irs income , Equifax income etc at household.

renewal_households_applicants ->It stores applicants related data
(applicants first name , applicant last name )

**RRVProcessor** -- It is a class which has 3 schedular jobs

**1 databaseToXML()** -- It is a cron job which runs every 2 mintues and
takes data from database , invokes dds-RRV internally , and converts it
into xml data.

**2 xmlToZipFiles()** -- It is a cron job which runs every 3 minutes and
internally dds-RRV service , and converts xml into zip file .

**DVS-RRV Module**

It has aggregator class , based on TDS type , it invokes respective
*irsAggregator , ssaAggregator , equifaxAggregator, medicareAggregator,
dmfAggregator.*

*Inside respective Aggregator, configurations are configured
FILE_NAME_PREFIX,FOLDER_NAME ,templateName ,responseTemplateName ,templatePrefix
,templateSuffix ,responseXMLArrayKey ,tdsType)

It has couple of implementation classes .

**XMLProcessor** -- takes data from database , processes , converts it
into xml data.

**RRVUnzipImpl** -- takes zip file and converts it into zip file

**RRVZipImpl** -- It takes xml data, converts it into zip file
