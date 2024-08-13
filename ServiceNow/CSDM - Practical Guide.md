# DRAFT DRAFT DRAFT

# CSDM in Practice
A practical guide implementing ServiceNow CSDM

Version 20240813 

# Safe Harbor Statement

This guide is <ins>a way</ins> to implement the Common Service Data Model (CSDM) on the ServiceNow platform. I am not bold enough to say this guide is <ins>the way</ins>. The CSDM framework is an ever evolving design modeling structure by engineers at ServiceNow, if anyone can state the exact way to implement the CSDM it is them. So, your mileage may vary on what you can take away from this guide. Your best path forward is listening, understanding, and collaborating with others.

# About

This guide comes from experience gained from my employer; which will stay unsaid. The examples are real world everyday practical implementation examples that anyone can use. What you take away from this guide is hopefully ideas how to implement the CSDM in your organization.

I have the fortune to work at a moderately large manufacturing multi-conglomerate that has multiple (8 at this time) distinct business units (which I call clients as I am in corporate IT) covering a wide range of brands and products. What is unique about the business arrangement is each client is federated entity within the broader context of the business, with each client possessing their own independent IT department. I work at the corporate office and my job to to ensure that the enterprise (aka 8 clients) collaborate on the same instance of ServiceNow. 

__TLDR;__ the experience of this guide is built on getting 8 different clients, that see the world differently, have opposing agendas, and barely aligned priorities into using a single instance of ServiceNow.

# Introduction

One of my favorite post movie ending clips comes from the Walt Disney movie Finding Nemo. In the clip a band of fish escape from their aquarium back to the ocean, only to find they are still trapped in their plastic baggies floating on the ocean and the scene ends with the question “now what?” Whether you are a customer or partner, we have all been there. After the flashy presentations, dazzling demos, and the perversity digital ink has dried, the time comes to actually implement what you bought.

The architects and engineers of ServiceNow has helped put together a really great model called the Common Service Data Model (CSDM) to help organize and better understand the various services your organization has to offer. The CSDM ties into many various aspects of the ServiceNow platform that I am still either implementing or discovering myself.

## Baseline

If you do not know anything about the CSDM, I recommend you first search for it online and read what the authors’ have to say. I cannot provide a link because the way ServiceNow keeps their documentation evergreen and any link will be outdated without notice. A simple “ServiceNow CSDM” search will suffice. From there, you can get all the formal definitions and explanations directly from the source. No reason for me to copy-paste what is already publicly and freely provided.

## Boiled Down Baseline

The CSDM is basically the org chart for business operations. Just like many organizations have a personnel org chart that shows the connections between people, the CSDM is a model to show the relationships between business operations. The CSDM can link the tiniest asset, like a thermostat on the wall of a shop floor all the way to business operations at the C-level.

## Before We Begin

There are a couple of hurdles one has to reconcile or overcome when it comes time to implement CSDM within your ServiceNow instances and here is what I have found to be true.

- The CSDM is constantly maturing. At the time of this sentence, the CSDM is at version 4 with version 5 in draft. (I expect version 5 is have AI sprinkled in like sprinkles on cupcakes.) The key takeaway from this is to never get caught up in either being finished or frustrated when ServiceNow announces a new CSDM version.
- Very rarely do people come into a body of work with no preconceived ideas. The baggage of life experiences is always present. You will have conversations around how other products have similar CSDM model and how other products do it differently. The key takeaway is hear them out by listening to their requirements and do not emulate another product in ServiceNow. Let ServiceNow, be ServiceNow.
- Leadership will not understand or leadership will have preconceived idea how easy this will be to setup. The key takeaway is patience and keep a calm matter of fact attitude.

# Services

Where our journey begins is what I call the heart of the CSDM and that is services. (Note: The CMDB comes in later. The CMDB is important, but it not the starting point to root the CSDM from.)

I am going to forgo any attempt to define what a service is or is not. There are a great many smart noodlers out there, far smarter than I, that can provide an eloquent definition what a service is. I will provide the implementer’s version.

If you draw a paycheck from your employer, then your employer is paying you to perform a service for the organization. If a solution has an invoice (one-time or re-occurring) or requires time and effort, that solution is providing a service. 

__TLDR;__ a service is anything in the organization that takes time or money.

Everyone provides a service, from the highest ranking C-level suite to the office cleaning staff, each provides a defined service. It is our job to quantify what that service is and create a definition of the service in ServiceNow.

__Sidebar:__ There is a scene in the 1999 movie _Office Space_, where two auditors, played by John C. McGinley and Paul Wilson, are interviewing an office worker, played by Richard Riehle, where they are trying to better understand everyone’s role within the company. It is a hilarious scene where John and Paul’s characters are trying to understand what Richard’s character actually does for the company. After a back and forth banter, John’s character finally asks _“what is it that you actually do here?”_ And Richard’s character goes into a tailspin rant. If you have not seen the movie, then your homework is to watch that movie before continuing with this guide.

## Service Types

There are three primary service types in ServiceNow; application, business, and technical. These three form the broader groupings of services you will define and all are extended from the Configuration Item [cmdb_ci] table.

### Technical Services
Before any other CSDM services are defined, I like to define these types of services first. I really do not like ServiceNow’s scant definition of a technical service. When you read the word “technical”, do not think “technology”, think “technically” as in “technically, this is what I know” or “technically, this is what do ” The key is to focus on what do people know or do when defining technical service offerings.

For example, let us say you need to open a ticket requesting help setting up a cubicle for a visiting guest. That is not actually a technology, but a service that is probably provided by either Human Resources, an office/floor manager or a secretary. So you can and should create a technical service called Office Management or Office Resources. Do not jump ahead and think of these as business services, that’s completely different.

### Application Services
Application Services have nothing to do with software applications. (I will give you a moment to let that sink in.) Recall how the CSDM is an ever maturing model put forth by ServiceNow? Well, mistakes were made and what is done is done. The name Application Service is not accurately describe the intent by name alone, but the definition and meaning behind it is correct. When ServiceNow choose the word “application”, they meant it in a pure academic sense of the definition “to apply.” I have read one of the early authors of the CSDM lamented they wished they named it System Services, but I do not feel that would have faired better. 

Instead of Application Services, think of them as Deployed Services (though I will not be calling these types of services by that name, this is just to help you see where I am going). Application Services are the deployments (aka instances) of a solution for the organization. Another definition or view is actualizations, as in what is real in practice. 

Using the same example above, you need a temporary cubicle for a visitor, but you have three offices; Burbank, London, and Madrid. You can have three Application Services for each office as they are realizations of the organization in three distinct locations. So when a ticket is created the Application Service can be used to help route the ticket to the proper assignment group for each location. 

The same can be said for digital solutions as well. Let us say at each of the three office locations, IT has deployed an instance of the timekeeping solution Kronos. Also, the IT department maintains a development (DEV) and user acceptance testing (UAT) instance of Kronos as well. Therefore, there will be 5 Application Service definitions defined in ServiceNow; one for each deployment of Kronos. 

### Business Services
Business Services answers the broader question _“what can the organization accomplish?”_ Business Services bundle up all the Technical and Application Services into groupings that answer broader questions. As I like to see them, these are services for director and above to understand. Business Services would be defined as Financial Services, Marketing, Sales, Manufacturing, Legal Services, etc. etc..

While Business Services are important to define to get the larger landscape of the organization, they are typically the last to be defined and the least important in priority. The reason for this is that Technical Services and Application Services are the foundations that make up Business Services. So unless you are starting an organization from scratch, it is better to focus on Technical and Application Services first.

## Service & Service Offerings

There are 2 different service definitions per service type listed above; service and service offering for each. Go read ServiceNow’s definition of what a Service and a Service Offering is. (I’ll wait, no rush.)

So let’s simplify what a Service and Service Offering are. A Service is a grouping of Service Offerings. A Service Offering is the lowest form of service for its type. Nothing complicated, but you can make it as complicated if you so desire.

__TLDR;__ services are the buckets and Service Offerings are the marbles in the buckets. 

It can get complicated because technically you chain together Services and Service Offerings as they share a base table with a parent field. On one hand, this offers flexibility to model complex models of services. On the other hand developing, maintaining, supporting, and reporting can become quite tedious very quickly.

My recommendation is to keep it simple with these basic rules. 

- A single Service shall have 1 or more Service Offerings.
- A Service shall not have a parent Service of the same service type.
- A Service could have no parent.
- A Service Offering shall have a Service as a parent.

This keeps a flat set of records where Services are groupings of Service Offerings.

Examples:
- Network (Service)
    - WAN (Service Offering)
    - LAN (ServiceOffering)
    - MPLS (Service Offering)
    - WIFI (Service Offering)
- Identity and Access Management (Service)
    - Active Directory (Service Offering)
    - Active Directory Accounts (Service Offering)
    - Active Directory Groups (Service Offering)
    - LDAP (Service Offering)
    - SSO (Service Offering)
- Field Offices (Service)
    - Burbank (Service Offering)
    - London (Service Offering)
    - Madrid (Service Offering)

There are a couple of reasons we set this up this way.
- __Easy to understand standard__ - A simple relationship between all Services and Service Offerings is straightforward. Once you get into multiple levels the maintenance of the organization of the records can get confusing. ServiceNow allows for a complex tree-structure when it comes to all services, the problem becomes obvious overtime, as roles and organizations change. 
    __TLDR;__ you are smart, but your predecessor might not be. So keep it simple.
- __Simple lookups__ - There is a balance between easy to search and easy to find. They are two completely different definitions in the context of user experience (UX). On one hand, having the scroll through volumes of records can be very tedious. And then also having to hunt for what you are looking for with multiple mouse-clicks can also be tedious as well. By having a two-tier relationship between Services and Service Offerings you are addressing both the search and find aspect of UX. Searches can easily be narrowed down to two searches; one for the right Service and one for the right Service Offering. Instead of complex tree searches. Finding through the act of mouse-clicks is narrowed down to two mouse-clicks.
    __TLDR;__ save yourself from headaches later on when developing solutions that rely on the CSDM records.
- __Easier reporting__ - An area that constantly gets overlooked is reporting. You want reports to represent information equally and consistently. By having a two-tier relationship between Services and Service Offerings, reports will be far easier to understand without having to understand embedded sub-services; which in the long run, we have found really does not result in better outcomes or decisions.
    __TLDR;__ complex CSDM models equals complex reporting.

__Key Point:__ Keep Service and Service Offerings simple. The CMDB is already complex enough, do not make more work for yourself by making the the CSDM complex too.

## Nouns & Verbs

The CSDM defines various types of services as defined above; technical services, technical service offerings, and application services that you will need to setup. When setting up a service definition/record, all services definitions should be nouns with no verbs.

Service definitions can be either generalized nouns or proper nouns with no verbs in their definition. The reason is many times a service can have multiple actions (verbs) and that is better saved for catalog items (requests). 

Here are a few examples of common mistakes.

| Incorrect Service Name | Correct Service Name      |
|------------------------|---------------------------|
| Password Reset         | Active Directory Account  |
| Windows Update         | Microsoft Windows Desktop |
| Provision SAP Account  | SAP Account               |
| Security Remediation   | Attack Surface Management |

__TLDR;__ service definitions should only contain nouns and catalog items should be the verbs.

__Leadership Alert:__ When setting up Services and Service Offerings, the whole noun concept is a hard concept for leadership to grasp. Leadership would want to create services with verbs in the definition. For example adding “grant”, “revoke“, “reset”, etc. etc. into the service definition name. The problem is using verbs would create an avalanche of service offerings. Because then there is “provision”, “reclaim”, “add”, “delete”, “security remediation”, “patch”, “update”, “upgrade”, and the list continues on and on. Then trying to stay consistent becomes something to manage as well, when do you use “add”, “provision”, or “create”? Ultimately, leadership wants to ensure they can see what actions (verbs) their teams were performing. Explain how catalog items capture the verbs and how they would generate reports based on request items. 

__Real World:__ One of the challenges I ran into was a scenario where a single service offering was supported by 3 different support groups. One support group supported provisioning virtual machines, one support group supported patching virtual machines, and a third support group supported backing up and restoring virtual machines. Leadership wanted the following Service Offering definitions:

- Virtual Machine - Provision
- Virtual Machine - Patching
- Virtual Machine - Backup & Restore

I knew there were more operations (verbs) for virtual machines. I worked on understanding what the teams did and came up with the following Service Offering definitions. I still had to create 3 different service offerings for virtual machines, because a Service Offering can only have 1 support group. But, none of the Service Offerings have verbs in their definitions.

- Virtual Machine - Services
- Virtual Machine - Operations
- Virtual Machine - Disaster Recovery

__Virtual Machine - Services__ would take care of provisioning, reclamation, resizing, and moving of virtual machines. 
__Virtual Machine - Operations__ would take care of updates, upgrades, and security remediation of virtual machines.
__Virtual Machine - Disaster Recovery__ would take care of backups, recovery, and planning. 

## Advice

Here are some tips to consider when setting up service standards in your organization.

### Unique Names 

Typically, unique names are not a problem when defining records. But, what happens when:

- you have multiple teams or sub-teams that provide tiered support?
- you have multiple IT departments within multiple companies within the same organization?

To address these issues, devise a standard which is appended to service definition names to make them unique. Consider using additional attributes to differentiate service names. For example, you could include a unique identifier, such as a department code or location, in the service name.

I prefer a _”pill”_ or _”block”_ approach for naming standards. A _”pill”_ uses parentheses and a _”block”_ uses square brackets to append tags to service names to make them unique. Within the pill or block the standard of tags can be defined and set apart.  

The standards below are examples based on needing to support 8 different clients on the same instance of ServiceNow. Your mileage will vary based on your organization’s structures.

##### Scenario
The following scenario is used when showing examples of the various services to be defined.

A hypothetical organization is comprised of 3 companies (ABA, CAC, and DEL) with each having their own IT department. Each of these companies have an Identity and Access Management (IAM) team. All three companies use a multi-service provider (MSP) named ZET to handle all tier 1 service desk calls for IAM requests and incidents. The ABA company has outsourced tier 2 support to external vendor QED. And then all 3 companies provide final support.  

#### Technical Service

The syntax for technical services:
        
`<name> (<company stock symbol>|<department code>)`

where:
- __\<name\>__ is the name of the service
- __\<company stock symbol\>__ is the company stock symbol
- __\<department code\>__ is the department code or symbol

Department code is needed for similar technical service names within a company. As an example “Security” could be used for both physical security at facilities and cybersecurity for IT. “Engineering” could be used for manufacturing, research and design, and PLM for IT. 

##### Scenario

In this scenario, notice how there are no definitions at the Technical Service level for external suppliers ZET and QED. The reason is rarely are external suppliers accountable at the business level. Even though they provide a service for the organization, they are replaceable.

| Technical Service                         | Support Group |
|-----------------------------—-------------|---------------|
| Identity and Access Management (ABA\|ITN) | SN_ABA_IAM    |
| Identity and Access Management (CAC\|ITN) | SN_CAC_IAM    |
| Identity and Access Management (DEL\|ITN) | SN_DEL_IAM    |

#### Technical Service Offering 

The syntax for technical service offerings:
        
`<name> (<company stock symbol>|<T#>)`

where:
- __\<name\>__ is the name of the service
- __\<company stock symbol\>__ is the company stock symbol
- __\<T#\>__ is the support tier number

Regarding tier number, tiers are integers that start at 1 and goes to N. When defining services with tiers, make sure the tiers are consecutive with no gaps; gaps add confusion and doubts. Also, as an advanced tip, tier 0 should be reserved for automation. When designing workflows/flows that perform automation, make sure that tickets that have service fields use tier 0 services to denote automation operations.

##### Scenario

| Technical Service Offering               | Support Group |
|------------------------------------------|---------------|
| Active Directory - Accounts (ENT\|T1)    | Zeta          |
| Active Directory - Accounts  (ALP\|T2) | Delta         |
| Active Directory - Accounts  (ALP\|T3) | Alpha         |
| Identity and Access Management (BET\|T2) | Beta          |
| Identity and Access Management (GAM\|T2) | Gamma         |

__NOTE__: See section _Support Groups_ regarding proper naming conventions.
