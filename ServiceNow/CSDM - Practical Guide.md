# DRAFT DRAFT DRAFT

# CSDM in Practice
A practical guide implementing ServiceNow CSDM

Version 20240825 

# Safe Harbor Statement

This guide provides one approach to implementing the Common Service Data Model (CSDM) on the ServiceNow platform. However, I'm not bold enough to claim that it's the definitive way. The CSDM framework continually evolves through the efforts of engineers at ServiceNow, and if anyone can state the exact way to implement CSDM, it's them. Therefore, your mileage may vary in terms of takeaways from this guide. Your best path forward lies in listening, understanding, and collaborating with others.

# About

As someone working at a moderately large manufacturing multi-conglomerate, I've had the opportunity to collaborate with eight distinct business units (which I refer to as clients). Each client operates independently within the broader context of the business, with its own IT department. My role involves ensuring that all eight clients use the same instance of ServiceNow.

In this guide, I'll share real-world examples of how to implement the Configuration and Service Data Model (CSDM) effectively. Whether you're part of a large organization or a smaller team, these insights can help streamline your processes and enhance collaboration. 

__TL;DR:__ In this guide, I share practical insights on implementing the Configuration and Service Data Model (CSDM) within your organization. Drawing from real-world examples, I discuss how to streamline processes and enhance collaboration across distinct business units. Whether you're part of a large enterprise or a smaller team, these insights can help you effectively use ServiceNow.

# Introduction

One of my favorite post-movie ending scenes comes from the Walt Disney film _Finding Nemo_. In this clip, a band of fish escapes from their aquarium and returns to the ocean, only to discover that they are still trapped in their plastic baggies floating ontop of the water. The scene ends with the poignant question, 'Now what?' The reason why this scene resonates is we have all been there. Whether you're a customer or a partner, we've all been there. After the flashy presentations, dazzling demos, and once the ink has dried on the digital contracts, it's time to actually implement what you've purchased.

Anytime a solution is purchased, the number 1 question on the minds of leadership is "when will the solution be up and running?" Ultimately they want to see return on value as soon as possible. Now of course the answer varies on many factors like leadership involvement, staff knowledge and utilization of external ServiceNow partners. Now if you are reading this you are either in the position of pre-implementation or post implementation of ServiceNow. The later is a bit more effort to make changes, but it is possible and I will lay out strategies where it makes sense.  

The architects and engineers at ServiceNow have developed an excellent model called the Common Service Data Model (CSDM). This model helps organize and enhance our understanding of the various services offered by our organization and the best place to start from when implementing the ITSM module of ServiceNow. The CSDM does integrate with other various modules of the ServiceNow platform as well, which I am still exploring and implementing.

## Baseline

If you are unfamilar with ServiceNow's CSDM, I recommend you first search for it online and read what the authors’ have to say, before you read what this guide has to offer. I cannot provide a link because the way ServiceNow keeps their documentation evergreen and any link will be outdated without notice. A simple “ServiceNow CSDM” search will suffice. From there, you can get all the formal definitions and explanations directly from the source. No reason for me to copy-paste what is already publicly and freely provided.

## Boiled Down Baseline

The Common Service Data Model (CSDM) serves as an organizational framework for business operations. Analogous to personnel org charts that depict connections between individuals, the CSDM provides a model to illustrate relationships within business processes. From the smallest assets—such as a shop floor thermostat—to high-level business operations at the C-suite, the CSDM establishes links across the entire spectrum.

In essence, the CSDM offers a holistic view of an organization's assets, services, and their interdependencies, facilitating efficient management and strategic alignment. If you'd like further details, I recommend referring to authoritative resources on the topic.

## Before We Begin

- The Common Service Data Model (CSDM) is an evolving framework. Currently, the CSDM stands at version 4, with version 5 in draft. The crucial lesson here is to remain adaptable—neither fixated on completion nor frustrated by new versions announced by ServiceNow.
   
- Individuals rarely approach a project without preconceptions. Life experiences shape our perspectives. Expect discussions comparing CSDM to other product models. The key lies in active listening to requirements and avoiding direct emulation of other products within the ServiceNow ecosystem. Let ServiceNow maintain its unique identity.
   
- Leadership may underestimate the complexity of CSDM setup. Exercise patience and maintain a calm, matter-of-fact demeanor. Educate them on the process while emphasizing realistic expectations.

Remember, successful CSDM implementation involves collaboration, adaptability, and clear communication.

# The Core of CSDM: Understanding Services

At the heart of the Common Service Data Model (CSDM) lies the concept of services. While the Configuration Management Database (CMDB) plays a crucial role, it isn't the initial starting point for grounding the CSDM.

__Sidebar:__ If you receive a paycheck from your employer, you are compensated for providing a service to the organization. Whether it's a solution invoiced periodically or a task requiring time and effort, it falls under the umbrella of services.

**TL;DR:** A service encompasses anything within the organization that consumes time or financial resources.

Rather than attempting an exhaustive definition, let's focus on practical implementation. From top-level executives to office custodians, everyone contributes a distinct service. Our task is to precisely define and document these services within the ServiceNow framework.

Remember, clarity and alignment are key as we map out services in ServiceNow.

__Sidebar:__ In the 1999 movie _Office Space_, two consultants—Bob Slydell (played by John C. McGinley) and Bob Porter (played by Paul Wilson)—interview an office worker named Tom Symkowski (played by Richard Riehle). Their goal is to better understand everyone's role within the company. The scene is hilariously depicted as the two Bobs try to grasp what Tom actually does. After a back-and-forth banter, Bob Porter finally asks, 'What is it that you actually do here?' Tom then launches into an unhinged tailspin rant. If you haven't seen the movie, consider watching it before continuing with this guide."

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

__TL;DR:__ services are the buckets and Service Offerings are the marbles in the buckets. 

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
    __TL;DR:__ you are smart, but your predecessor might not be. So keep it simple.
- __Simple lookups__ - There is a balance between easy to search and easy to find. They are two completely different definitions in the context of user experience (UX). On one hand, having the scroll through volumes of records can be very tedious. And then also having to hunt for what you are looking for with multiple mouse-clicks can also be tedious as well. By having a two-tier relationship between Services and Service Offerings you are addressing both the search and find aspect of UX. Searches can easily be narrowed down to two searches; one for the right Service and one for the right Service Offering. Instead of complex tree searches. Finding through the act of mouse-clicks is narrowed down to two mouse-clicks.
    __TL;DR:__ save yourself from headaches later on when developing solutions that rely on the CSDM records.
- __Easier reporting__ - An area that constantly gets overlooked is reporting. You want reports to represent information equally and consistently. By having a two-tier relationship between Services and Service Offerings, reports will be far easier to understand without having to understand embedded sub-services; which in the long run, we have found really does not result in better outcomes or decisions.
    __TL;DR:__ complex CSDM models equals complex reporting.

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

__TL;DR:__ service definitions should only contain nouns and catalog items should be the verbs.

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
- you have multiple teams in different locales?

Though service records contain various fields, the services cannot cover all the various scenarios that make your company unique. The go to for some is to create custom fields and there are cases where this would be applicable. If you can forsee duplicate service names, consider using additional attributes to differentiate services by their names. 

I prefer a _”pill”_ or _”block”_ approach for naming standards. A _”pill”_ uses parentheses and a _”block”_ uses square brackets to append tags to service names to make them unique. I have not run into a case where I would use both a pill and block style for naming convensions on the same platform; either will suffice in ServiceNow. Within the pill or block the standard of tags can be defined and set apart.  

The standards below are examples based on needing to support 8 different clients on the same instance of ServiceNow. Your mileage will vary based on your organization’s structures. Take this an idea and implement your own version that best fits your organization's style.

##### Scenario
I will be using the following scenario as an example for defining various services.

A hypothetical organization is comprised of 3 companies (COM1, COM2, and COM3) with each having their own IT department. Each of these companies have an Identity and Access Management (IAM) team. All three companies use a multi-service provider (MSP1) to handle all tier 1 service desk calls for IAM requests and incidents. The COM1 company has outsourced tier 2 support to external vendor EXT1. And then all 3 companies provide final support.  

#### Technical Service

The syntax for technical services:
        
`<name> (<company stock symbol>|<department code>)`

where:
- __\<name\>__ is the name of the service
- __\<company stock symbol\>__ is the company stock symbol
- __\<department code\>__ is the department code or symbol

Department code is needed for similar technical service names within a company as many departments have simalar service names. As an example “Security” could be used for both physical security at facilities and cybersecurity for IT. “Engineering” could be used for manufacturing, R&D, and PLM for IT. 

##### Scenario

In this scenario, notice how there are no definitions at the Technical Service level for external suppliers MSP01 and EX01. The reason is rarely are external suppliers accountable at the business level. Even though they provide a service for the organization, they are replaceable. Technical Service[HERE I AM]

| Technical Service                         | Support Group |
|-----------------------------—-------------|---------------|
| Identity and Access Management (ABA\|ITN) | SN_C01_IAM    |
| Identity and Access Management (CAC\|ITN) | SN_C02_IAM    |
| Identity and Access Management (DEL\|ITN) | SN_C03_IAM    |

#### Technical Service Offering 

The syntax for technical service offerings:
        
`<name> (<company stock symbol>|<T#>)`

where:
- __\<name\>__ is the name of the service
- __\<company stock symbol\>__ is the company stock symbol
- __\<T#\>__ is the support tier number

Regarding tier number, tiers are integers that start at 1 and goes to N. When defining services with tiers, make sure the tiers are consecutive with no gaps; gaps add confusion and doubts. Also, as an advanced tip, tier 0 should be reserved for automation. When designing workflows/flows that perform automation, make sure that tickets that have service fields use tier 0 services to denote automation operations.

##### Scenario

| Technical Service Offering                | Support Group |
|------------------------------------------|---------------|
| Active Directory - Accounts (ENT\|T1)    | Zeta          |
| Active Directory - Accounts  (ALP\|T2)   | Delta         |
| Active Directory - Accounts  (ALP\|T3)   | Alpha         |
| Identity and Access Management (BET\|T2) | Beta          |
| Identity and Access Management (GAM\|T2) | Gamma         |

__NOTE__: See section _Support Groups_ regarding proper naming conventions.
