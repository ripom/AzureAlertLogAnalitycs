# Introduction 
This project is intended to help to create and deploy Azure Alerts for log analitycs queries and Azure Action Groups using code.
The Alerts and Action Group creation is very boring task especially if is done manually.
To let simplify the creation and managibility using code, the best way to create and manage is to use ARM template (Terraform support for log analitycs queries is not in place yet).

# Getting Started
The project is based on Azure Resource Manager (ARM) template and it is composed by two couples of files (one for deployment template and one for deployment parameter):
1. One couple to create the Action Groups resources to assign to the alerts.
2. One couple to create the Alerts resources.

Once the Alerts and the Action Group have been designed, it's enought customize the parameters files to easily deploy the resources.

First of all it's necessary to define the Action Groups.
The action Group parameter file has one parameter names actionGrp, this parameter is an array of objects and it's possible to define in the array more than one Action Group to create in that Subscription/ResourceGroup.
The actionGrp parameter has six fields:
1. Name - of the Action Group
2. Shortname - of the Action Group
3. Status - of Action Group (Can be true for enabled or false for disabled)
4. smsReceivers - it's an array of objects and you can define more than one smsReceivers
5. emailReceivers - it's an array of objects and you can define more than one emailReceivers
6. webhookReceivers - it's an array of objects and you can define more than one webhookReceivers

Then it's necessary to define the Alerts.
The Alerts parameter file has one parameter names alerts, this parameter is an array of objects and it's possible to define in the array more than one Alert to create in that Subscription/ResourceGroup.
The alerts parameter has fortheen fields:
1. alertLocation - Region which create the alert in
2. alertName - name of the alert
3. alertDescription - alertDescription
4. alertStatus - status of alert (Can be true for enabled or false for disabled)
5. alertSourceQuery - kusto query
6. alertSourceSourceId - ID of the log analitycs workspace to query
7. alertScheduleFrequency - Frequency
8. alertScheduleTime - Time
9. alertActionsSeverityLevel - Severity alert
10. alertTriggerOperator - Trigger operator for query result
11. alertTriggerThreshold - Trigger threshold 
12. actionGrpName - Action Group name (eventually defined in the Action Group parameter file), the Action Group must be already created otherwise you get error, you don't need to provide the Action Group ID but only the name, the ARM template will automatically search for it in the subscription.
13. actionGrpSubject - Email Subject
14. actionGrpWebhook - Additional field to the webhook Payload, if yu don't want enable this, you can leave the field empty. Don't remove the field because the template will generate an error.

# Limitation
These templates are not be able to deploy alerts and action group in different subscription using the same parameters file.
To deploy them in different subscription it's necessary a parameter file for each subscription to deploy resource with.