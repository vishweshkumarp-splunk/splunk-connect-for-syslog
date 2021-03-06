# Vendor - Cisco

## Product - ASA (Pre Firepower)

| Ref            | Link                                                                                                    |
|----------------|---------------------------------------------------------------------------------------------------------|
| Splunk Add-on  | https://splunkbase.splunk.com/app/1620/                                                                 |
| Product Manual | https://www.cisco.com/c/en/us/td/docs/security/asa/asa82/configuration/guide/config/monitor_syslog.html |


### Sourcetypes

| sourcetype     | notes                                                                                                   |
|----------------|---------------------------------------------------------------------------------------------------------|
| cisco:asa      | None                                                                                                    |
| cisco:pix      | Not supported                                                                                           |
| cisco:fwsm     | Not supported                                                                                           |

### Filter type

MSG Parse: This filter parses message content

### Setup and Configuration

* Install the Splunk Add-on on the search head(s) for the user communities interested in this data source. If SC4S is exclusively used the addon is not required on the indexer.
* Review and update the splunk_index.csv file and set the index as required.
* Follow vendor configuration steps per Product Manual above ensure:
    * Log Level is 6 "Informational"
    * Protocol is TCP/IP
    * permit-hostdown is on
    * device-id is hostname and included
    * timestamp is included

### Verification

Use the following search to validate events are present

```
index=<asconfigured> sourcetype=cisco:asa
```

Verify timestamp, and host values match as expected    

## Product - IOS and NX-OS based equipment

| Ref            | Link                                                                                                    |
|----------------|---------------------------------------------------------------------------------------------------------|
| Splunk Add-on  | https://splunkbase.splunk.com/app/1467/                                                                 |
| IOS Manual     | https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst2960/software/release/12-2_55_se/configuration/guide/scg_2960/swlog.html |
| NX-OS Manual   | https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/6-x/system_management/configuration/guide/b_Cisco_Nexus_9000_Series_NX-OS_System_Management_Configuration_Guide/sm_5syslog.html|
| Cisco ACI      | https://community.cisco.com/legacyfs/online/attachments/document/technote-aci-syslog_external-v1.pdf |
| Cisco WLC & AP | https://www.cisco.com/c/en/us/support/docs/wireless/4100-series-wireless-lan-controllers/107252-WLC-Syslog-Server.html#anc8 |
### Sourcetypes

| sourcetype     | notes                                                                                                   |
|----------------|---------------------------------------------------------------------------------------------------------|
| cisco:ios      | This source type is also used for NX-OS, ACI and WLC product lines                                                                                                    |

### Filter type

* Cisco IOS products can be identified by message parsing alone
* Cisco NX OS, WLC, and ACI products must be identified by host or ip assignment update the filter `f_cisco_nx_os` as required

### Setup and Configuration

* Install the Splunk Add-on on the search head(s) for the user communities interested in this data source. If SC4S is exclusively used the addon is not required on the indexer.
* Review and update the splunk_index.csv file and set the index as required.
* IOS Follow vendor configuration steps per Product Manual above ensure:
    * Ensure a reliable NTP server is set and synced
    * Log Level is 6 "Informational"
    * Protocol is TCP/IP
    * permit-hostdown is on
    * device-id is hostname and included
    * timestamp is included
* NX-OS Follow vendor configuration steps per Product Manual above ensure:
    * Ensure a reliable NTP server is set and synced
    * Log Level is 6 "Informational" user may select alternate levels by module based on use cases
    * Protocol is TCP/IP
    * device-id is hostname and included
    * timestamp is included and milisecond accuracy selected
* ACI Logging configuration of the ACI product often varies by use case. 
    * Ensure NTP sync is configured and active
    * Ensure proper host names are configured
* WLC 
    * Ensure NTP sync is configured and active
    * Ensure proper host names are configured
    * For security use cases per AP logging is required


### Verification

Use the following search to validate events are present, for NX-OS, WLC and ACI products ensure each host filter condition is verified

```
index=<asconfigured> sourcetype=cisco:ios | stats count by host
```


Verify timestamp, and host values match as expected    

# Vendor - Fortinet

## Product - Fortigate

| Ref            | Link                                                                                                    |
|----------------|---------------------------------------------------------------------------------------------------------|
| Splunk Add-on  | https://splunkbase.splunk.com/app/2846/                                                                 |
| Product Manual | https://docs.fortinet.com/product/fortigate/6.2                                                         |


### Sourcetypes

| sourcetype     | notes                                                                                                   |
|----------------|---------------------------------------------------------------------------------------------------------|
| fgt_log        | The catch all sourcetype is not used                                                                                                    |
| fgt_traffic    | None                                                                                         |
| fgt_utm        | None                                                                                          |
| fgt_event      | None 

### Filter type

MSG Parse: This filter parses message content

### Setup and Configuration

* Install the Splunk Add-on on the search head(s) for the user communities interested in this data source. If SC4S is exclusively used the addon is not required on the indexer.
* Review and update the splunk_index.csv file and set the index as required.
* Refer to the admin manual for specific details of configuration to send Reliable syslog using RFC 3195 format, a typical logging configuration will include the following features.

```
config log memory filter

set forward-traffic enable

set local-traffic enable

set sniffer-traffic disable

set anomaly enable

set voip disable

set multicast-traffic enable

set dns enable

end

config system global

set cli-audit-log enable

end

config log setting

set neighbor-event enable

end

```

### Verification

An active firewall will generate frequent events, in addition fortigate has the ability to test logging functionality using a build in command

```
diag log test
```

Verify timestamp, and host values match as expected    

```
index=<asconfigured> (sourcetype=fgt_log OR sourcetype=fgt_traffic OR sourcetype=fgt_utm) 
```

### UTM Message type

![FortiGate UTM message](FortiGate_utm.png)

### Traffic Message Type

![FortiGate Traffic message](FortiGate_traffic.png)

###Event Message Type
![FortiGate Event message](FortiGate_event.png)

Verify timestamp, and host values match as expected    

# Vendor - PaloAlto

## Product - NGFW

| Ref            | Link                                                                                                    |
|----------------|---------------------------------------------------------------------------------------------------------|
| Splunk Add-on  | https://splunkbase.splunk.com/app/2757/                                                                 |
| Product Manual | https://docs.paloaltonetworks.com/pan-os/9-0/pan-os-admin/monitoring/use-syslog-for-monitoring/configure-syslog-monitoring.html                                                         |


### Sourcetypes

| sourcetype     | notes                                                                                                   |
|----------------|---------------------------------------------------------------------------------------------------------|
| pan:log        | None                                                                                                    |
| pan:traffic    | None                                                                                         |
| pan:threat     | None                                                                                          |

### Filter type

MSG Parse: This filter parses message content

### Setup and Configuration

* Install the Splunk Add-on on the search head(s) for the user communities interested in this data source. If SC4S is exclusively used the addon is not required on the indexer.
* Review and update the splunk_index.csv file and set the index as required.
* Refer to the admin manual for specific details of configuration
    * Select TCP or SSL transport option
    * Select IETF Format
    * Ensure the format of the event is not customized

### Verification

An active firewall will generate frequent events use the following search to validate events are present per source device

```
index=<asconfigured> sourcetype=pan:*| stats count by host
```

# Vendor - Symantec

## Product - ProxySG/ASG (Bluecoat)

| Ref            | Link                                                                                                    |
|----------------|---------------------------------------------------------------------------------------------------------|
| Splunk Add-on  | https://splunkbase.splunk.com/app/2758/                                                                 |
| Product Manual | https://support.symantec.com/us/en/article.tech242216.html                                                        |


### Sourcetypes

| sourcetype     | notes                                                                                                   |
|----------------|---------------------------------------------------------------------------------------------------------|
| bluecoat:proxysg:access:kv        | Requires version 3.6                                                                                                    |

### Filter type

MSG Parse: This filter parses message content

### Setup and Configuration

* Install the Splunk Add-on on the search head(s) for the user communities interested in this data source. If SC4S is exclusively used the addon is not required on the indexer.
* Review and update the splunk_index.csv file and set the index as required.
* Refer to the Splunk TA documentation for the specific customer format required for proxy configuration
    * Select TCP or SSL transport option
    * Ensure the format of the event is customized per Splunk documentation

### Verification

An active proxy will generate frequent events use the following search to validate events are present per source device

```
index=<asconfigured> sourcetype=bluecoat:proxysg:access:kv | stats count by host
```