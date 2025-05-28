# LogicAppWithHTTPRequestFromEventGrid
This Logic App is triggered by an HTTP request from Azure Event Grid when a .csv file is uploaded to Azure Blob Storage. It processes the event data and sends an email notification to the Data Engineering team. Ideal for integrating event-driven workflows using Azure services.
![image](https://github.com/user-attachments/assets/6ffda217-75d7-4297-b90e-af392d4ed873)

Step 1: Create HTTP Trigger in Logic App
In your Azure Logic App under the Development Tools tab, click on the Logic app designer, add the trigger, and search for When HTTP request is received.

To get the HTTP URL, click Save and copy the URL. This will be used as we continue the project.

![image](https://github.com/user-attachments/assets/ed6c0cde-7328-41af-8bd4-2823042b7bcc)

Step 2: Create EventGrid Trigger
In your Azure storage account, click the Events tab, then select Event Subscription.

![image](https://github.com/user-attachments/assets/10cb6285-1be4-4d1b-94dd-91ffb26003fd)

Fill in the necessary information as seen in the image below.

![image](https://github.com/user-attachments/assets/b3cf5e64-a8c0-4d6e-8cc2-e1768b36ed62)

We plan to use a trigger when a Blob is created in a particular directory in our Azure Storage account.

We also need to configure the endpoint, a WebHook from our HTTP URL from Azure Logic App.

Click Confirm Selection.

![image](https://github.com/user-attachments/assets/bd6a14f5-235f-4929-ad66-654c59d5f0f7)

Step 3: Set Filters
Under the Filters, we will use the official pattern for filtering in Azure EventGrid. We are focused on only CSV files.

/blobServices/default/containers/<CONTAINER_NAME>/blobs/<BLOB_PREFIX>

Click Create.

![image](https://github.com/user-attachments/assets/3ff53383-0545-403e-b347-1adc99febe96)

Confirm that Event Grid was provisioned successfully.

![image](https://github.com/user-attachments/assets/f975add4-d4b4-442e-929f-92790956824f)

Set Up the Azure Logic App Flow Process
Now that we have Azure EventGrid set up and working as expected, we need to set the action for the Azure LogicApp to receive an HTTP POST request from EventGrid.

Step 1: Set Schema
To understand the schema format, let’s start by uploading a CSV file to the storage account.

![image](https://github.com/user-attachments/assets/406533a6-cf0a-4721-8e22-7b980a66b66b)

In your LogicApp, click Run History and select the latest run History to perform an inspection.

![image](https://github.com/user-attachments/assets/0c400ce7-6c6e-484c-8811-e8a30b548b2b)

Copy the Body of the HTTP request received from EventGrid. This will help pick up the necessary information going forward

![image](https://github.com/user-attachments/assets/30cd1d87-a147-4012-8a5b-d96c51d30f1d)

[
  {
    "topic": "/subscriptions/xxxxxxxxxxxxxxx/resourceGroups/docker_rg/providers/Microsoft.Storage/storageAccounts/airflowdatalakestaging",
    "subject": "/blobServices/default/containers/airflowcontainer/blobs/logicapp_eventgrid/cust_1.csv",
    "eventType": "Microsoft.Storage.BlobCreated",
    "id": "247654f7-e01e-006d-7ce8-a0d3c00646dc",
    "data": {
      "api": "PutBlob",
      "clientRequestId": "xxxxxxxxxxxxxx",
      "requestId": "xxxxxxxxxxxxx",
      "eTag": "0x8DD6EFF9966BE44",
      "contentType": "text/csv",
      "contentLength": 279,
      "blobType": "BlockBlob",
      "accessTier": "Default",
      "blobUrl": "https://airflowdatalakestaging.blob.core.windows.net/airflowcontainer/logicapp_eventgrid/emp.csv",
      "url": "https://airflowdatalakestaging.blob.core.windows.net/airflowcontainer/logicapp_eventgrid/emp.csv",
      "sequencer": "00000000000000000000000000031601000000000000d00b",
      "identity": "$superuser",
      "storageDiagnostics": {
        "batchId": "10359642-d006-0004-00e8-a0ea8c000000"
      }
    },
    "dataVersion": "",
    "metadataVersion": "1",
    "eventTime": "2025-03-29T20:23:42.5895899Z"
  }
]

With the information from the body, we can create a Sample Template of the information we want to capture from Azure EventGrid.

{
  "subject": "",  
  "eventType": "",  
  "id": "",  
  "data": {
    "api": "",  
    "contentType": ""
  },  
  "eventTime": ""  
}

Click on the Use sample payload to generate schema link,then paste the value in.

![image](https://github.com/user-attachments/assets/cbd99b18-2731-4108-a189-fffd5f4ab11e)

Step 2: Set Initialize Variable
Add a new action called Initialize Variable to capture the information from the HTTP Trigger action. In the Value tab, for your Initialize Variable, we need to set an HTML template of the message to be sent.

<hr/><h2>New Blob Created in Storage</h2><hr/><b>File Path:</b>@{triggerBody()[0]?['subject']}<br/><b>Event Type:</b><span>@{triggerBody()[0]?['eventType']}</span><br/><b>API:</b>@{triggerBody()?[0]['data']?['api']}<br/><b>Content Type:</b>@{triggerBody()?[0]['data']?['contentType']}<br/><b>Event Time:</b><span>@{addHours(triggerBody()?[0]['eventTime'],1)}</span><br/><hr/><b>Information:</b><br/><p>The operation was successfull!</p><hr/>

![image](https://github.com/user-attachments/assets/fc166a70-9161-4a2e-aa04-c5b73c199803)

Step 3: Set Send an Email Action
Add a new action. Send an Outlook message and fill in the Dynamic content of the Email variable. Ensure you save the Flow before leaving the tab.

![image](https://github.com/user-attachments/assets/25664a12-55c8-4448-b550-496620b6a03e)

Step 4: Test Flow Logic
With all the necessary actions added, let’s test the flow by adding a new document (DEPT.csv) to the storage account.

With the new document uploaded, wait for a few seconds and confirm the flow in Run history.

![image](https://github.com/user-attachments/assets/4e07728e-97fd-4478-886a-b2e10129b31c)

Check the Email to confirm information was also sent to Outlook with the captured requirements.

![image](https://github.com/user-attachments/assets/170ce929-63ed-4e46-923e-47880fb93db3)












