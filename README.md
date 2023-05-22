**RRV BATCH JOB FLOW**

**DVS-RRV -\>** It is a module , reads data from database , converts it
into xml , futher processes , converts xml into zip and pushed it to
SFTP external service to validate the data .

**MS-DVS -**

RRV Controller(com.getinsured.iex.hub.rest.controller) -it is a
controller layer in ms-dvs . It has couple of end points ,where in we
can initiate batch job , check batch job processing status and persists
data into below mentioned tables.

**Important tables**

dvs_batch -- It stores batch id , batch name , batch creation date .

renewal_batch -- It has batch related entries and stores these entries
ifsc file , eqifax file , nmac file name ,ifsc_batch status, Equifax
batch status , processor id , smec batch status etc .

renewal_households -- It captures household id,household income , batch
id , ifsc file name , Equifax file name , irs income , Equifax income
etc at household.

renewal_households_applicants -It stores applicants related data
(applicants first name , applicant last name )

RRVProcessor(com.getinsured.iex.hub.rrv.scheduler) -- It is a class
in ms-dvs module which has 3 schedular jobs .It internally accesses
dds-rrv module and to enable it , we have to add below dependency in
ms-dvs module .

\<dependency\>

\<groupId\>com.getinsured.dvs\</groupId\>

\<artifactId\>dvs-rrv\</artifactId\>

\<scope\>provided\</scope\>

\</dependency\>

**1 databaseToXML()** -- It is a cron job which runs every 2 mintues and
takes data from database , invokes respective aggregrator based on tds
type (dds-RRV module) internally , and converts it into xml data.

**2 xmlToZipFiles()** -- It is a cron job which runs every 3 minutes and
internally RRVZip interface (dds-RRV)service , and converts xml into zip
file.

**3 readIncomingDirectory() -** It is a cron job which runs every 1
minute and we reads the data from sftp external service , unzip the file
, stores the data into database.

**DVS-RRV Module**

It has aggregator class , based on TDS type , it invokes respective
*irsAggregator , ssaAggregator , equifaxAggregator, medicareAggregator,
dmfAggregator.*

*Inside* respective *Aggregator, configurations are configured
(*FILE_NAME_PREFIX,

FOLDER_NAME ,templateName ,responseTemplateName ,templatePrefix
,templateSuffix ,responseXMLArrayKey ,tdsType)

It has couple of implementation classes.

**XMLProcessor** -- takes data from database , processes , converts it
into xml data.

**RRVZipImpl** -- it takes xml data, converts it into zip file .

**RRVUnzipImpl** -- takes unzip file and converts it into zip file.
