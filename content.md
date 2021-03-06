## What are Runbooks?

**Runbooks** can be thought of as manuals or playbooks used to guide you through a structured set of tasks. Each runbook has one or more tasks, each task building on the previous task. While runbooks will not replace manuals or other documentation, they allow you to share knowledge across your team when it is needed most. 

Runbooks can be used for incident response, data processing and verification, customer account management, and many other processes. The importance of using runbooks to standardize and verify internal processes make them an essential part of documentation for any organization.  

Runbook automation helps standardize workflows across teams, provides structure to regular maintenance tasks and incident resolution, and share information from a single source. 

In this article you will learn how to use runbook automation to extend your documentation to make it actionable, accessible, accurate, authoritative, and adaptable. 

## Why Use Runbook Automation

Runbook automation helps share knowledge. Experiential knowledge is often difficult to share, leaving your team mates confused or frustrated when the information they need is not readily available. Runbook automation provides integration with external services and access to your runbooks through the CLI or by using Python or NodeJs, keeping your information accessible and correct.

Runbook automation provides security. A marketing associate probably does not need direct access to a production database, however they may need to transfer data from a CRM to begin an onboarding process. Access controls can eliminate the need to navigate sensitive information. 

In short, automation lets you create documentation that is actionable, accessible, accurate, authoritative and adaptable. 

### Actionable

Traditional runbooks are not executable, which means that any action listed in a runbook must be done manually under most circumstances. Automation lets each step in a runbook become executable documentation, providing a simple interface that includes information and the ability to act on it. 

As a simple example, let's say you are tasked with sending customer information from a CRM to a Slack channel on a weekly basis. In a traditional runbook, this process involves five steps:

- Log in to CRM
- Locate the customer using their id
- Export the customer information
- Format the information for Slack
- Send slack message

The five steps can be converted to a runbook containing two tasks. 

The first task will run the `SQL` query against the selected database (this example uses a Demo database):

```sql
SELECT id, company_name, country, signup_date, total_dollars
FROM accounts
WHERE id = :user_id
```

Using Slack integration, the second task will format the data and post the message in your selected channel. 

### Accessible

Departments may require access to certain data, but also would like to restrict access to other data. Groups allow fine grained permissions that allow you to access only the information you need. For example, the marketing developers will not need `admin` status for a particular runbook so their permissions should reflect their access level.

The access permissions will additionally prevent viewers from seeing and interacting with portions of the runbook they are restricted from. This creates a more secure environment for running tasks against production databases. 

### Accurate

Updating a traditional document based runbook requires time intensive collaboration and verification to ensure accurate information added to the document. As processes change over time the runbook unfortunately becomes out of date. Without a regular schedule updates can easily be forgotten. Luckily, automation can do the scheduling for you. 

As you update the runbook itself you can take advantage of testing to ensure all tasks in the runbook complete successfully. This gives you and your team the knowledge that the process will run successfully each time. If a runbook does not run successfully, look to the audit log as the source of truth by drilling down into the metadata of the execution to pinpoint where and when the error occurred. 

To ensure accuracy you might also want to use a command line tool to programmatically create new tasks. This allows tasks to be templated and the task description and code can be checked into a version control tool, like `git`.

Using the CRM example from above, you can set up a yaml file with the relevant details. The example below uses a database called `[Demo DB]`

```yaml
# file: query_user.task.yaml

slug: query_user
name: query_user
description: "Query the DB for user info"
parameters:
  - slug: name
    name: Name
    type: integer
    description: "The user's ID."
    default: 1
# Configuration for a SQL task.
sql:
  # The name of a database resource.
  resource: "[Demo DB]"
  entrypoint: query_user.sql
  queryArgs:
     user_id: "{{params.user_id}}"
```

The entrypoint `query_user.sql` file contains the following code

```sql
???  file: query_user.sql

SELECT id, company_name, country, signup_date, total_dollars
FROM accounts
WHERE id = :user_id
```

When deployed the information in `query_user.task.yaml` will be used to create a new task, if one doesn't already exist with the same name. This task can now be run by itself or add it as a step in a playbook. 

### Authoritative

In an ideal world there should only be one official copy of the documentation. However, in practice there may be multiple drafts of the same document floating around and locating the correct draft becomes a chore. 

You may not have time to ask around for the most recent copy, and decide instead to randomly select a document that seems like it has the most recent information.  You still can't be sure that the steps in the document are correct. At this point you probably have lost a significant amount of time and gained a bit of frustration. 

Finding inaccurate data is a frustrating experience, especially when resolving an incident. Regular review cycles keep documentation updated and storing documentation alongside a runbook as notes makes the revision a lot less time consuming. During a review cycle both documentation and runbook content will be reviewed and updated. After deploying an updated runbook the tasks become immediately available to anyone who has the correct access permissions. 

### Adaptable

Runbooks are frequently used as a part of incident response, but automation can allow runbooks to adapt to almost any purpose. For example, say the marketing development team would like a new process for onboarding prospective clients. An example runbook could instruct developers to enter and verify information, provide relevant access to company services, and automatically send a welcome email. 

Additionally as your organization grows the processes must be modified to fit any new requirements. Runbook automation simplifies this process by providing a standard method of creating and executing runbooks, including relevant team members via group permissions, and automates the review process of the newly created runbook to ensure that all new information is accounted for. This serves to make runbooks and runbook automation a key part of the documentation process. 

## Closing

In this article you have learned about runbook automation and how automation helps create actionable, accessible, accurate, authoritative, and adaptable documentation. 

You also read about how runbooks can be secured with groups and role based permissions, how runbook actions can be reviewed with audit trails, and how you can verify accuracy testing each step of the runbook. 

Airplane.dev is a fully featured solution that offers runbook automation and more, including Github and Slack integration, scheduled execution, user groups with specific access permissions, and task creation on the command line or by using NodeJS, Python, or Docker. 
Create tasks, add resources like PostgreSQL and SendGrid, and use these tasks to create fully actionable, extendable documentation.

