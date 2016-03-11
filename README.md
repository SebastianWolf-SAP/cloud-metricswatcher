# cloud-metricswatcher

Intro
-----------

With the current SAP HANA Cloud Platform tools, you can configure email notifications for critical metrics of custom JMX checks and availability checks. 
This configuration is applicable for metrics of a running HCP Java application. For more information, see [Configuring Availability Checks from the Console Client](https://help.hana.ondemand.com/help/frameset.htm?6537f99f89c047c6ad16d49eb3f97a11.html)
and [JMX Checks](https://help.hana.ondemand.com/help/frameset.htm?ef5c05a713154945b347f87b54446c2b.html).

<p align="center">
  <img src="/doc/graphic1.png" width="75%">
</p>

However, you might want to be notified for other critical metrics of running HCP Java applications. Furthermore, you might need to perform healing operations when such metrics can cause issues such as low performance or crashes.
This tutorial is to help you implement a notification application that will watch each metric for a critical state, will notify you via email or SMS when such metrics are received, and will take actions to fix the issues caused by such metrics. Furthermore, the Java applications that the notification application receives metrics from can be located in different accounts and data centers.
The communication between this custom notification application and the HCP Java applications is as follows:

<p align="center">
  <img src="/doc/graphic2.png" width="75%">
</p>

1.	The notification application requests metrics of an HCP Java application from the monitoring service with a REST API call every minute.
Note: The calls are sent per minute because the Java application metrics are refreshed each minute. 
For more information about the REST call, see [Monitoring API](https://api.hana.ondemand.com/monitoring/v1/documentation).

2.	The monitoring service sends back a JSON response with a status code 200 OK.
The response contains the metrics of the requested application and the states of these metrics.
3.	The notification application parses the JSON response and checks for critical metrics.
4.	The notification application notifies you by the following conditions:  
  a.	A metric is critical for the first time – you receive a notification email.  
  b.	A metric is critical three times – you receive an SMS.  
  
5.	The notification application checks if metrics have been critical three times and takes actions for application self-healing (the Java application is restarted).
6.	The notification application repeats steps 1 to 5 for all other Java applications.

Configuration
-------------
Download this project locally and follow the instructions in [Tutorial: Implementing a Notification Application](http://scn.sap.com/docs/DOC-71317).

Authors
-------

**Ivaylo Ruskov**
+ http://github.com/iruskov

**Nikola Simeonov**

Copyright and license
---------------------

Copyright (c) 2015 SAP SE

Except as provided below, this software is licensed under the Apache License, Version 2.0 (the "License"); you may not use this software except in compliance with the License.You may obtain a copy of the License at:

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.