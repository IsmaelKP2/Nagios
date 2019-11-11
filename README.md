# Nagios
ITSI correlation searches for Nagios

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

#First install the Nagios Add-on :

https://splunkbase.splunk.com/app/2703/

Verify that you have an eventgen.conf files and a samples folder 

Please find the samples find in /opt/splunk/etc/apps/Splunk_TA_nagios_core/samples/

service-perfdata.samples for service performance data
nagios_notifications.samples for Alerts data


#Second install EventGen :

http://splunk.github.io/eventgen/SETUP.html#install

make sure to enable the EventGen both in Manage Apps -> SA-EventGen and in Settings->Data Inputs->SA-EventGen

Restart your Instance

Verify that the data is ingested by running the following search index=* spourcetype=nagios*

service_name
service_shortname are two fields that shows the different KPIs

#Correlations Searches

you can find the correlations package here 


----

If you want to build your own 

here is the search :

Index=“Nagios_serviceperf_new” | eval norm_severity=case(  service_status==“CRITICAL”,6,
  service_status==“WARNING”,4,  service_status==“OK”,2)| dedup consecutive=true host_name service_status service_name| eval tmp_entity=host_name | eval sec_grp= “default_itsi_security_group” | ‘match_entities(tmp_entity,sec_group)’| ‘filter_maintenance_entities’


First Line I do an eval to map the severity to the severity in ITSI
The second line severity is into the field service status. The ITSI norm severity is the is the severity Give a normal state severity of six and
the third line here is is done to do a dedup of the event. So that actually you don't get duplicates we do it here on hostname service status and service name. So, provided that you have those are those teams name in your data is going to work.
Fourth Line tmp_entity is the entity we have in ITSI we map it here to the host_name

The last lines you don’t need to worry it's added automatically to the search
https://crontab.guru/




We want now to build the aggregations policy to group the event there is one by default 

but let us us service_name for the grouping 

Tada final results 










