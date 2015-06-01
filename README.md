# Indoor-Location
Attached to this email are all of the scripts used by the location project. Here is the quick explanation of the scripts and what can and cant be done with them. The php scripts that run on the RTC Lab VM server(run by Don) are all commented and documented inline. The other scripts that run on the OTS servers are not documented and are not even the same scripts that run on the OTS servers. These scripts were the baseline scripts I sent to OTS to setup the server and they had to be modified to work properly by David. Any OTS service changes have to go through David. 

Explanations:
location.php 
This script is the live script that runs on the RTC lab server. It can be found in the /var/www/html/location_project/ folder. This script takes input via the HTTP get variable "mac" and uses this mac address to contact the OTS location server(http://10.47.128.156:8080) using the same variable (http://10.47.128.156:8080?mac=00:00:00:00:00:00). The OTS server will take about 3-4 seconds to respond since it is searching a very large database. Any service attempting to use this script will have to have its timeout extended since our Android phone was timing out when getting location from OTS. Once the OTS server returns data it returns a JSON object with a type "discovered/associated" and  a location "RT-9-9C3". The script then takes this data and runs an algorithm to determine actual location. This algorithm will have to be improved and further looked at to be made more efficient at triangulating by looking at floor plans for the iit tower. Once the algorithm runs(well documented in the code) it returns the xml+pidflo data. This script will have to be modified to recognize more buildings on campus and how to interpret their building codes and also the building's addresses will have to be coded into this script to expand it into the campus script. 

location_raw.php
This script takes in the MAC address and gives debug output of what the OTS server sees. This is useful for debugging a script that is not working or not producing expected output since it will go out to the OTS server and fetch exactly what the OTS server is saying and a computation of the location can be done by hand.

location_herman.php 
This is the script that was used to return herman hall as the location for the demo. This script shows how to just make a blank script that returns the same location each time. 

location_dev.php
This is the script that I did all my sanboxing in and development. It should be the same script as location.php since location.php was copied from this script. 

547-locaion-api.php
This is the script that runs on the OTS location server. This is just an example of what actually runs on the server since the service is a black box to us however in this script you can see how the script receives a MAC address and runs a bash script located on the server which performs the location lookup. Once the data is returned the script takes the bash output and parses it into a JSON object. Any modifications will have to go through David however this script is a good starting point to plan any modifications. 

547-location.sh
This is the bash script that invokes an expect script that contacts the server and grabs the information from the on campus wifi controller. This too is a base script that was sent to David before he modified it to work on his systems. Use this as a starting point however I highly doubt this script will need to be changed since it is coded to return all the information to the php script to do the heavy lifting. 

547-location-expect.exp
This is the expect script that goes out to the wireless controller, logs in and runs the actual commands on the wireless controller to receive the data used in our project. This script is invoked by the bash script. This script will also not be changed since it provides all the information to the bash script and the heavy lifting is performed by the PHP script. 


VM Details
The VM that runs our RTC location service(LIS) is run by Don. The IP address of the server is 216.47.129.105. For access to this machine you should contact Don to setup credentials. The scripts on this machine can be found in /var/www/html/location_project/

Let me know if you need any more information on these scripts. Each script is fairly well documented and easy to follow. In the case someone is not familiar with the PHP that runs these scripts they can look up the php functions online by googling the function to understand its use and functionality. 
