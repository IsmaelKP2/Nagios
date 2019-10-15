# Nagios
ITSI correlation searches for Nagios


Used with samples files from the the splunk add-on for Nagios Splunk Build Add-On
https://splunkbase.splunk.com/app/2703/

Documentation for the Add-on here :
https://docs.splunk.com/Documentation/AddOns/latest/NagiosCore/About

This add-on must be installed on a Linux instance of Splunk Enterprise for data collection. The add-on is platform independent for indexers and search heads.

ITSI versions tested for this package :
https://splunkbase.splunk.com/app/1841/
Documentation for ITSI
https://docs.splunk.com/Documentation/ITSI/latest

What is Nagios where to download it 
https://www.nagios.org/projects/nagios-core/

What is a correlation Searches how to configure it :
https://docs.splunk.com/Documentation/ITSI/4.3.1/Configure/Correlationsearchoverview


- How To - 

#First let us ingest the sample files from the Nagios Add-On :

Please find the samples find in /opt/splunk/etc/apps/Splunk_TA_nagios_core/samples/

service-perfdata.samples for perf data
nagios_notifications.samples for Alerts data

1 Ingest Data samples_updated/service-perfdata.samples

2 Monitor 

3 Index once 

4 Next choose your sourcetype 
Select nagios:core:serviceperf

5 Create an index nagios_serviceperf

6 Next review 

7 Next type the search index=nagios_*

Repeat for notifications

find the fields and values 

service_name
service_shortname are two fields that shows the different KPIs

With this we should be able to use EventGen to continously generate events

#Correlations Searches

you can find the correlations package here 
----

If you want to build your own 

here is the search 

We want now to build the aggregations policy to group the event there is one by default 

but let us us service_name for the grouping 

Tada final results 










