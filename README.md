# Google Nest Doorbell

Integration of Google Nest Doorbell events (chime, motion, sound)

# How to get Google API credentials
You should first grab a cup of coffee. No really, do it. This stuff is going to be intense. As far as I know this is the only way to subscribe to Google's Smart Device Management events. I have no idea why they made it this hard but I hope I can pull you through this. It requires you to pay a $5 fee to Google as a one time 'developer' fee.

After that the process consists of:
* Creating an OAuth client
* Creating a project in Google's Device Access Console
* Link the OAuth client to the new project
* Create a subscription (for your events)
* Create a service account that can access this subscription
* Enter the data you gathered into the Google Nest Homey plugin

## Create an OAuth Client ID
* Visit: https://console.cloud.google.com/apis/credentials
* Click "+ Create credentials" on top of the page
* Pick "OAuth Client ID"
* Application type: "Web application"
* Name: "Homey API" (or whatever you choose)
* Authorized redirect URIs: click "+ ADD URI"
* Enter: "https://www.google.com" (without quotes, copy paste, use https!)
* Click "Save"

--[Client ID & Client Secret]-- Write down your **Client ID** and **Client secret**. You will need these later on. If you lose them you can get them back in the credentials page.

## Get access to "Google Devices Access Console"
#### Register and pay the fee
* Visit: https://console.nest.google.com/device-access
* Login with your account and pay the one-time $5 fee

#### Create a project
* If you paid the fee; visit: https://console.nest.google.com/device-access
* Click "Create project"
* Give your project a name eg. "Homey project"
* Enter your OAuth client ID from before, click next
* Enable events (this is important!!)
* Finally click "Create project"
* When your project is created, open it and make note of the Pub/Sub topic; it will look like this: projects/sdm-prod/topics/enterprise-A_RANDOM_ID_HERE
* --[Topic name]-- Copy that entire topic, write it down as your **Topic name**
* This page also shows your "**Project ID**" on the top
* --[Project ID]-- copy that and write it down as your **Project ID**

## Link your account
Now it's time to link your account so that your smart device events will be published to Google's pub/sub servers.

* Visit the following URL: https://nestservices.google.com/partnerconnections/PASTE_YOUR_PROJECT_ID_HERE/auth?redirect_uri=https://www.google.com&access_type=offline&prompt=consent&client_id=PASTE_YOUR_CLIENT_ID_HERE&response_type=code&scope=https://www.googleapis.com/auth/sdm.service
* Replace PASTE_YOUR_PROJECT_ID_HERE with your **Project ID**
* Replace PASTE_YOUR_CLIENT_ID_HERE with your OAuth **Client ID** from before
* If you visit this URL you will be presented with a permissions page
* Enable all checkboxes so that your own API has all the needed permissions
* (the last checkbox gets enabled when the second last is clicked and then you click somewhere else on the page)
* Click next
* Login with your Google account (the same as your smart devices)
* Now it will probably say "Google didn't very this app" (that is correct, it's your app, so)
* Click advanced: "Continue to whatever (unsafe)"
* In the consent screen click "Allow"
* You will be redirect back to Google with an URL that looks like this: https://www.google.com/?code=4/A_BIG_LONG_CODE_HERE&scope=https://www.googleapis.com/auth/sdm.service
* --[Authorization code]-- Copy A_BIG_LONG_CODE_HERE (which of course is a random access code, we call this your "**Authorization code**")

## Activate your PULL subscription
* Visit: https://console.cloud.google.com/cloudpubsub/subscription/create
* Be sure that your project is selected at the top
* Make up a subscription ID eg: "homey-subscription"
* Click "Select a cloud pub/sub topic" and click "Enter topic manually"
* Enter your "**Topic name**" here from before!
* Don't retain acknowledged messages (can cost money)
* Don't change any other settings
* Click create at the bottom
* After you are done creating this topic you will have a subscription name under "Subscription details"
* --[Subscription name]-- Write that down as your "**Subscription name**", you will need this.

#### Last but not least
We need to create a "Google service account" that acts as the Identity that accesses your secure API. 
* Visit: https://console.cloud.google.com/iam-admin/serviceaccounts
* Click "+ Create service account"
* Give it a name eg. "Homey service account"
* Enter a description eg. "Homey service account"
* Click "+ Create"
* Click "Select a role" under "Grant this service account access to project"
* Go to "Basic" and select "Owner"
* Click "Continue"
* Click "Done"
* Open the service account you just create
* At the bottom under "Keys" click "Add key" and click "Create new key"
* Select key type "JSON" and click "Create"
* It will now download a .json file
* Open the downloaded file with notepad, right click on it in your explorer "Open with" and select notepad
* --[Credentials file]-- Copy the contents of this file, these are your credentials. We will call it your "**Credentials file**"

## You are done, pat yourself on the back and grab another cup of coffee
Finally you are left with 5 important credentals: 

* **Subscription name**
* **Credentials file**
* **Authorization code**
* **Client ID**
* **Client secret**

You will need all these to activate your Google Nest device. These are the 2 things you need to activate the homey plugin for your Nest doorbell. Now you can go ahead and add a device, select "Google Nest Doorbell". It will ask for a subscription name and a credentials file, you should e-mail these to yourself so you can copy paste the values. Paste them both in (don't add any white space) and finalize the process.
