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









