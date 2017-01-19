# OpenWhisk Starter Overview

The OpenWhisk starter overview demonstrates several of OpenWhisks' capabilities

# Preparation

1. [Register](https://console.ng.bluemix.net/registration) for a Bluemix account if you have not done so already

2. Go to the creation page of the Cloudant NoSQL DB service using [this](https://console.ng.bluemix.net/catalog/services/cloudant-nosql-db/) link or search for "cloudant" in the Bluemix catalog

3. Click on "Service Credentials" then on "New Credential" and finally click "Add" (this will allow OpenWhisk to connect to that DB instance)

4. Go to the creation page of Watson Language Translator service using [this](https://console.ng.bluemix.net/catalog/services/language-translator) link or search for "translator" in the Bluemix catalog 

5. Click on the "Create" button to add the new service

# Creating the translation mediator action

1. Go to the [OpenWhisk editor](https://console.ng.bluemix.net/openwhisk/editor) and create a new action (use the default settings) with any name of your choosing

2. Replace the main function with the one below -

```
function main(params) {
	//translateTo values can be any one of the supported 62 languages
	//in this case we use French ("fr") but you can choose any of the other supported ISO 639-1 codes
	return { payload: params['id'], translateTo: "fr" };
}
```

Note that the changes are auto-saved but not live. In order to publish them you will need to click the "Make it Live" button on the bottom right

# Creating the sequence

1. Next, click on "Link into a Sequence"

2. Scroll down and choose "Watson Translator" and then choose "translator" under "Select an Action in this Package"

3. Select the Blue "Language Translator" binding on the bottom left

4. Click on "Add to Sequence" (note the flow of the sequence) and then click on "This Looks Good" to confirm

5. Finally click on "Save Action Sequence" (you can change the sequence name if you like) and "Done" to finish creating the sequence

# Seeing it in action

1. Select the sequence you just created uner "My Sequences" and click on "Run this Sequence"

2. Provide the following JSON input -

```
{
    "id": "Hello"
}
```

Finally, click on "Run with this Value" and you should see the translated *Bonjour* response

# Adding the DB insertion mediator action

1. Create yet another new action, same as before and replace the main function with the one below - 

```
function main(params) {
	return { doc: { _id: params.payload } };
}
```

Make a note of the name you chose for the action, we will need it in the next step

# Extending the sequence

1. Select the sequence you created in the previous step again and then click on "Extend"

2. Select "My Actions" and choose the name of the action you had just created and click "Add to Sequence"

3. Click on "Extend" again, but this time choose "Cloudant"

4. From the many available options, choose "create document" and proceed to add a new binding by clicking the Green "New Binding" button on the bottom left

5. Provide a name for the binding and proceed to select the instance of Cloudant you created earlier while making sure the dbname selected is the **"translation"** DB

6. To finish, click on "Save Configuration", followed by "Add to Sequence" and then on "Save Your Changes"

# Creating the trigger

1. Click on "Automate"

2. Choose "Cloudant Changes" and then click on the Green "New Trigger"

3. Provide a name for the trigger and proceed to select the instance of Cloudant you created earlier while making sure the dbname selected is the **"phrases"** DB

4. Click on "Save Configuration" and "Next" (note that the new flow now includes the trigger as well)

5. Finally, click on the "This Looks Good" button and "Save Rule" (you can change the rule name if you like) and "Done"

# Testing the whole flow

1. Go to the [monitor screen](https://console.ng.bluemix.net/openwhisk/dashboard) and note the Activity Log on the right

2. Go to the web applicaton you deployed earlier and proceed to add a new phrase

3. Refresh the Activity Log and you should see the entire sequence was triggered due to the change in the "phrases" DB and the translation of the new phrase you added appears as the output

4. Go to the Cloudant instance you created (you can find it in your [dashboard](https://console.ng.bluemix.net/dashboard/services)), select it and click "Launch" on the "Manage" tab

5. Select the "translation" database and you should see the translation of the phrase you entered stored in the database as a new document
