# Introduction tutorial to IBM Cloud Data Services
These instructions and some files needed in the tutorials are provided at:  https://github.com/jruponen/cloud_data_services_tutorial
  
### Tutorial in short:  
1: Setup Node-RED boilerplate  
2: Collect ESC16 tweets and store into Cloudant  
3: Examine Cloudant data and sync to dashDB  
4: Examine dashDB data and analyze with R
  
For more IBM Cloud Data Services tutorials see:  
https://developer.ibm.com/clouddataservices/



## 1. Set up Node-RED boilerplate (if not done already)
	1. Go to Bluemix catalog  
	2. Select "Node-RED Starter" under "Boilerplate" category on the top  
	3. Fill in your "appname" and "host" and press "Create"  
	- You may use any host name as long as it is unique in the mybluemix.net domain  
	4. Wait until your application is "Staged" and then click "Overview"  
	- Meanwhile you may install the CF command line tool, if not done already  

## 2. Set up a Node-RED flow to collect data
	1. Copy the contents of the attached Node-RED flow on your clipboard:  
	- (https://raw.githubusercontent.com/jruponen/cloud_data_services_tutorial/master/Node-RED_ESC16_Tweets.txt)  
	2. Open your Node-RED application (http://yourhostname.mybluemix.net/red)  
	3. On an empty flow canvas, select Menu / Import / Clipboard  
	4. Paste the text from clipboard and click OK  
	5. Place the flow on canvas and do mouse-click  
	6. Double-click on the first node, "Twitter in", and add your Twitter credentials  
	7. Double-click on the last node, "Cloudant out", and make sure it uses your Cloudant service  
	8. Press "Deploy" button (for either "Full" or "Modified Flows")  
	9. Check on the "Debug" tab that data is coming in  

## 3. Verify data in Cloudant and set up synchronization to dashDB
	1. Go to your Node-RED application overview page on Bluemix dashboard  
	2. Click on the "Cloudant" service bound to it  
	3. Click "Launch"  
	4. In the "Databases" view you should now see "esc16_tweets" database with some docs in it  
	5. Click on the "esc16_tweets"  
	6. On one of the documents, click on the "Pencil" icon in it's top right corner  
	7. Examine the JSON record, this is the tweet data  
	8. Click "Warehousing" on the left  
	9. Click "Create a dashDB Warehouse"  
	10. Enter your Bluemix credentials (your IBM ID and the password)  
	11. Enter warehouse name, e.g: tweets  
	12. Enter source database name: esc16_tweets  
	13. Leave the "Create new dashDB instance" selected  
	14. Press "Create Warehouse" and wait for a warehouse to be created  
	15. Click your warehouse "tweets"  
		- This is what is currently being synced from Cloudant to dashDB  
	16. Click back on your browser  
	17. On the right to the "tweets" warehouse name, click "Open in dashDB"  

## 4. Examine data in dashDB and analyze with R
	1. On the dashDB console, click "Tables"  
	2. Click on the "Table Name" dropdown list and select "ESC16_TWEETS"  
	3. What you see now is the "Table definition"  
	4. To see data, click "Browse Data"  
	5. Select "Run SQL" on the left  
	6. To verify number of rows, clean up the SQL screen and enter (or copy) the following statement:  
		SELECT COUNT(*) FROM ESC16_TWEETS;  
	7. Press "Run"
	8. See the SQL select result on below  
	9. To analyze the data, click on "Analytics" > "R Scripts"  
	10. Click on the "RStudio" button  
	11. You'll need to copy/paste dashDB username and password. These can be found from the previous browser tab (where dashDB is open). Go there and select "Connect" > "Connection Information". Copy/paste "User ID" and "Password" values to the RStudio tab.  
	12. Click "File" > "New Project"  
	13. Click "New Directory" > "Empty Project"  
	14. Enter directory name, e.g: ESC16  
	15. Press "Create Project"  
	16. Wait until RStudio refreshes and select "File" > "New File" > "R Script"  
	17. Copy/paste the attached R Script contents on the empty canvas in RStudio:  
	- https://raw.githubusercontent.com/jruponen/cloud_data_services_tutorial/master/dashDB_ESC16_R.txt  
	18. Press "Run" button to step through each line  
	19. After run "barplot" line, check the "Plots" tab on the right  
	20. Continue to "Run" rest of the lines in order to close the connection  
	21. Select "File" > "Quit RStudio"  
