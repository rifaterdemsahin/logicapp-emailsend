# Contractor Marketing Email Workflow - README

## Overview

This Azure Logic App workflow, named **Contractor Marketing - Send Email**, is designed to automate the process of sending marketing emails with an IT contractor's CV. The workflow includes functionality for dynamically setting email details such as the recipient, subject, body, and attachments based on specific input parameters and conditions.

## Workflow Components

### 1. **Triggers**
   - **Manual HTTP Trigger**: The workflow is manually triggered via an HTTP request. This allows external applications or systems to initiate the workflow by sending an HTTP POST request to the provided endpoint.

### 2. **Parameters**
   The workflow accepts the following parameters:

   - **$connections**: Manages the Office365 API connection required for sending emails.
   - **artifacttype** (default: "usacontract"): A string parameter to define the type of artifact or contract.
   - **body** (default: "Looking for the new contract. Here is the CV. I look forward to the opportunity to speak with you soon. Regards, Erdem"): This is the email body that will be sent.
   - **emailto** (default: "PurnimaM@vbeyond.com"): The recipient's email address.
   - **subject** (default: "IT Contractor Erdem CV"): The subject line of the email.

### 3. **Actions**
   The workflow consists of several key actions:

   - **Initialize Variables**: This section initializes variables like `body`, `emailto`, `subject`, `attachments`, and `runid`. These variables are used throughout the workflow to customize the email.
   - **Set Variables**: Several actions set the values for email components dynamically based on input parameters or conditions in the workflow.
   - **Condition Checks**: Conditional logic determines whether to proceed with actions based on whether certain parameters (like `body` or `artifacttype`) are present.
   - **Send Email (Office365)**: The email is sent via the Office365 API, using the dynamically set `subject`, `emailto`, and `body` values, as well as any attachments.
   - **Response**: Once the email is successfully sent, the workflow returns a 200 HTTP status code with a response body indicating success.

### 4. **Endpoints Configuration**
   - The workflow uses specific IP addresses for outgoing traffic and access endpoints, which must be allowed in your network configuration for proper functioning.

### 5. **Workflow States**
   - **Provisioning State**: Indicates the state of the workflow provisioning. In this case, it is marked as `Succeeded`.
   - **State**: The workflow is currently in a `Suspended` state, meaning it is not actively running. It can be resumed as needed.
   - **Versioning**: The workflow includes versioning information for tracking changes.

## Workflow Diagram

1. **Trigger** (Manual HTTP Request)
   - A POST request is made to the endpoint to initiate the workflow.
  
2. **Initialize Variables**
   - Email body, subject, and recipient are initialized based on input or default values.

3. **Set Variables**
   - Dynamic values from the HTTP request headers or predefined parameters are assigned to the workflow's variables.

4. **Conditional Logic**
   - Checks if required parameters (like `body`, `artifacttype`) are present to proceed with the email actions.

5. **Send Email**
   - The email is sent via Office365 API, including attachments if provided.

6. **Response**
   - A response is returned to the HTTP trigger with a success message.

## Deployment

### Prerequisites:
   - An active Azure subscription.
   - Office365 connection (this can be configured in the Azure Logic App).
   - API access and proper IP whitelisting to enable HTTP triggers.

### Steps:
   1. Deploy the Logic App via the Azure portal, ARM templates, or Azure CLI.
   2. Configure the required connections, specifically the **Office365 API connection**, to enable email sending.
   3. Trigger the Logic App via an HTTP request using the provided endpoint.
   4. Customize the email body, subject, and recipient as needed using the workflow parameters.

## Usage

### Triggering the Workflow
   The workflow can be triggered via an HTTP POST request to the following endpoint:

   ```
   https://prod-27.uksouth.logic.azure.com:443/workflows/a0529354359c4f8d91da9b914b00bd03
   ```

   **Example Request:**
   ```bash
   curl -X POST https://prod-27.uksouth.logic.azure.com:443/workflows/a0529354359c4f8d91da9b914b00bd03 \
     -H 'Content-Type: application/json' \
     -d '{
       "subject": "New Contractor CV",
       "emailto": "recruiter@example.com",
       "body": "Here is the updated CV of the contractor for your review."
     }'
   ```

### Customizing Parameters
   You can modify the following parameters within the HTTP request to tailor the email content:

   - **subject**: Change the subject line of the email.
   - **emailto**: Update the recipient's email address.
   - **body**: Customize the body text of the email to fit your needs.
   - **artifacttype**: Specify the contract type if applicable.

## Error Handling

The workflow includes conditional checks and variable initializations to ensure that all required fields are populated. In case of missing parameters or issues with the Office365 API, the workflow will fail gracefully, returning appropriate error messages via the HTTP response.

## Maintenance and Updates

### Resuming the Workflow
If the workflow is in a suspended state, you can manually resume it through the Azure portal or using Azure CLI commands:

```bash
az logic workflow start --resource-group ContractorMarketing --name contractormarketing-sendemail
```

### Modifying the Workflow
Updates to the workflow can be made through the Azure portal, where you can edit triggers, actions, and conditions as necessary. Ensure to test any changes before deploying them to production.

## Contact

For any issues or questions regarding this workflow, please contact your Azure administrator or the developer team responsible for contractor marketing automation.

---

This README provides an overview of the Contractor Marketing Email workflow, describing its structure, functionality, and usage. For further details or troubleshooting, refer to the Azure Logic Apps documentation.
