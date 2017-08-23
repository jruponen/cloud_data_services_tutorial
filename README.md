# Introduction tutorial to IBM Cloud Data Services
These instructions and some files needed in the tutorials are provided at:  https://github.com/jruponen/cloud_data_services_tutorial
  
### This tutorial in short:  
1. Open Watson IoT device simulator
2. Create Weather Company Data service
3. Setup Node-RED boilerplate
4. Import sample Node-RED flow and configure and deploy it
- when deployed observe sensor data, get weather forecasts by location and listen audible alerts
5. Verify stored sensor data in Cloudant NoSQL datastore and setup near real-time sync to relational data store
6. Examine data in relational data store and visualize with R
  
For more IBM Cloud Data Services tutorials see:  
https://developer.ibm.com/clouddataservices/

## Step 1: Open IoT device simulator
1. On a separate browser window, open address: https://quickstart.internetofthings.ibmcloud.com/iotsensor
2. Notice, that you can simulate temperature, humidity and object temperature
3. The IoT device ID is shown in the top right corner of the simulator (will need this later on)

## Step 2: Create Weather Company Data service
1. Go to Bluemix catalog
2. Search for "weather" and select "Weather Company Data"
3. Give your service a name (or use default) and press create

## Step 3: Set up Node-RED boilerplate (if not done already)
1. Go back to Bluemix catalog
2. Select "Node-RED Starter" under "Boilerplate" category on the top  
3. Fill in your "appname" and "host" and press "Create"  
- You may use any host name as long as it is unique under the mybluemix.net domain  
4. Wait until your application is "Staged" and then click "Overview" to go to Node-RED application overview page
- Meanwhile waiting you may install the CF & Bluemix command line tools, if not done already  
5. Select "Connections" and press "Connect Existing"
6. Select "Weather Company Data" and press Connect

## Step 4: Set up a Node-RED flow to collect data
1. Copy the contents of the sample Node-RED flow text file on your clipboard:
https://raw.githubusercontent.com/jruponen/cloud_data_services_tutorial/master/Node-RED_IoT_flow.txt

2. Go to your Node-RED application (http://yournoderedhostname.mybluemix.net/red)  
3. While on an empty canvas in Node-RED, select Menu > Import > Clipboard  
4. Paste the text from clipboard and click OK  
5. Position the flow on canvas and do mouse-click  
6. Double-click on the "IBM IoT" node and add copy-paste IoT device ID value into "Device Id" field, press done
7. Double-click on the "sensordata" Cloudant node and make sure the "Service" value points to your Cloudant service  
8. Press the red "Deploy" button on top right (you may select either "Full" or "Modified Flows")
9. Check on the "Debug" tab on the right and see the data coming in from the IoT simulator and be processed
- Note here, that that the "Manipulate Object" function node changes Temperature readings to Heartrate readings!
- Also note, that it adds timestamp and GPS coordinates to the data
- All the incoming sensor data should also be now stored into the Cloudant data store
- Weather Insights is used to get some live weather forecast for current location
- If your browser supports "Play Audio" node, you'll need audible messages indicating heart rate and forecast

## Step 5: Verify data in Cloudant and set up synchronization to Db2 Warehouse (dashDB)
1. Go to your Bluemix dashboard
2. Click the "Cloudant" service open it's info page
3. Click the green "Launch" button to open Cloudant console
4. In the "Databases" view you should now see "sensordata" database with some docs in it  
5. Click on the "sensordata" database  
6. Click one of the documents and examine the data (this is a one sensor reading in JSON format)

7. On another browser tab, go to Bluemix Catalog
8. Setup a "Db2 Warehouse on Cloud" service (formerly named “dashDB”)
9. When the "Db2 Warehouse on Cloud" info page opens, select “Service credentials” tab and press New Credentials
10. Select “Connections” tab, press Create Connection and select your Node-RED instance

11. Go back to browser tab having Cloudant console open
12. Click the "Warehousing" tab on the left (puzzle-piece icon)
13. Click "Create Warehouse" button
14. Enter your Bluemix credentials (your IBM ID and the password)  
15. Enter warehouse name, e.g: MYWH   
16. In the Data Sources field, press s and select “sensordata” database
17. On the bottom section, click “dashDB service instance” and select your “Db2 Warehouse on Cloud” instance
18. Press Create Warehouse
19. In the warning screen press Continue

## Step 6: Examine data in relational db and analyze with R
1. Still in the Cloudant dashboard, Warehouses screen, press “Open in dashDB”
2. In Db2 Warehouse (dashDB), in the left hand navigation, select Tables and then select “sensordata” table
3. Press Browse Data and review that the data exists (should be synced near real-time)
4. In the left hand navigation select Analytics > R Scripts
5. Press RStudio button
6. Enter dashDB username and password to RStudio login
- you can find these credentials from Node-RED “Runtime” section, under “Environment variables”, in dashDB section
7. Copy the sample R script on canvas
https://raw.githubusercontent.com/jruponen/cloud_data_services_tutorial/master/dashDB_IoT_R.txt

8. Run the script, line by line, and see the sample plot (see "plots" tab in R studio)
9. Remember to run the last line as well, which will close connection to the database
10. Select "File" > "Quit RStudio" (before closing, you may want to save your script)
