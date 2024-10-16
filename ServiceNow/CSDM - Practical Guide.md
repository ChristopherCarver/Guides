# DRAFT DRAFT DRAFT

# CSDM in Practice
A practical guide implementing ServiceNow CSDM

Version 20240825 

# Safe Harbor Statement

This guide provides one approach to implementing the Common Service Data Model (CSDM) on the ServiceNow platform. However, I am not bold enough to claim that it is the definitive way. The CSDM framework continually evolves through the efforts of engineers at ServiceNow, and if anyone can state the exact way to implement CSDM, it is them. Therefore, your mileage may vary in terms of takeaways from this guide. Your best path forward lies in listening, understanding, and collaborating with others.

# About

As someone working at a moderately large manufacturing multi-conglomerate, I have the opportunity to collaborate with eight distinct business units (which I refer to as clients). Each client operates independently within the broader context of the business, with its own IT department. My role involves ensuring that all eight clients leverage the same instance of ServiceNow with no domain seperation.

In this guide, I will share real-world examples of how to implement the Configuration and Service Data Model (CSDM) effectively within the ITSM module. Whether you are part of a large organization or a smaller team, these insights could help streamline your processes and enhance collaboration. 

> __TL;DR:__ In this guide, I share practical insights on implementing the Configuration and Service Data Model (CSDM) within the ITSM module for your organization. Drawing from real-world examples, I discuss how to streamline processes and enhance collaboration across distinct business units. Whether you are part of a large enterprise or a smaller team, these insights can help you effectively use ServiceNow.

# Introduction

One of my favorite post-movie ending scenes comes from the Walt Disney film _Finding Nemo_. In this clip, a band of fishes escapes from their aquarium and returns to the ocean, only to discover that they are still trapped in their plastic baggies floating ontop of the water. The scene ends with the poignant question, _'Now what?'_ The reason why this scene resonates is we have all been there. Whether you are a customer or a partner, we have all been there. After the flashy presentations, dazzling demos, and once the ink has dried on the digital contracts, it is time to actually implement what has been purchased.

Anytime a solution is purchased, the number 1 question on the minds of leadership is _"when will the solution be up and running?"_ Ultimately they want to see return on investment as soon as possible. Now of course the answer varies on many factors like leadership involvement, staff knowledge, and utilization of ServiceNow partners. If you are reading this, you are either in the position of pre-implementation or post implementation of ServiceNow. The later takes a bit more effort to make changes, but it is possible and I will lay out strategies where it makes sense when converting over.

The easiest and flashy way to show leadership immediate and actionable outcomes is doling out fulfiller licenses and creating catalog items for the service portal. And this is the typical immediate reaction implementation partners take as it gets things started by puting a _Now Open_ sign on the newly minted purchase of ServiceNow. But, there is more to it than just the front end and without understanding the Common Service Data Model (CSDM), you will be setting yourself up for future failure. By understanding the how the CSDM is laid out and works, this will help cement a solid foundation to build from.

The CSDM was designed and developed by architects and engineers at ServiceNow. This is not some third-party industry defined standard where you have to figure out how to adjust processes to meet "industry best standards", but rather practicial implementation how an organization defines itself and operates. By investing into the CSDM, the CSDM becomes the foundation and fuel to create the neccessary components that drive better outcomes from the ServiceNow platform.

## Why

Before we begin, it is important to address the why question: _"Why go through the effort of setting up the Common Service Data Model (CSDM) in ServiceNow?"_ The best way to answer this is through my own experience.

- Prior to adopting ServiceNow, my employer faced challenges with basic IT practices that negatively impacted customer satisfaction and caused long-term internal friction between the clients. One of the most noticeable issues consistently brought up was ticket misrouting, which led to long delays in delivering services to customers. Customers became frustrated driving down IT customer satisfaction. 

- I had an incredible mentor, and upon his retirement, I asked him, _"What is the biggest hurdle IT has yet to overcome during your tenure at the organization?"_ His response was, _"The CFO has yet to fully understand the value IT brings to the organization, because IT has yet to fully understand all the services it provides."_ As a cost center, IT departments are often seen as only consuming money rather than generating it. In a large and diverse organization, this perception is exacerbated as IT costs continue to rise and technology becomes more common place across all things.

I started with a simple two-part problem statement: _"How do we prevent ticket misrouting?"_ and _"How do we demonstrate IT's value?"_ The CSDM provided all the necessary components and definitions, but lacked specific instructions on what to do and how to do it. I filled in the gaps based on personal experience, intuition, and a dedicated team focused on solving these fundamental issues. As a result, our ticket misrouting is now at an all-time low, and we have developed reports that clearly show the value IT brings to the organization.

For me, it was clear that we could not carry the _"sins of the past"_ into the future with ServiceNow.

> __TL;DR__ The CSDM can feel a bit like a complex board game with a thick manual that talks about all of the pieces and the board, but there are no instructions on how to play. So, let us roll up our sleeves and I will show you how to play the game.

## Baseline

If you are unfamilar with ServiceNow's CSDM, I recommend you first search for it online and read what the authors’ have to say, before you read what this guide has to offer. I cannot provide a link because the way ServiceNow keeps their documentation evergreen and any link today will be outdated tomorrow without notice. A simple “ServiceNow CSDM” Internet search will suffice. From there, you can get all the formal definitions and explanations directly from the source. No reason for me to copy-paste what is already publicly and freely provided. 

My take is the CSDM serves as an organizational framework for business operations that shows the holistic view of an organization's services. Which highlights their interdependencies, facilitating efficient management and strategic alignment. Analogous to personnel org charts that depict connections between individuals, the CSDM provides a model to illustrate service relationships within an organization. From the smallest assets—such as a shop floor thermostat—to high-level business operations at the C-suite level, the CSDM establishes links across the entire spectrum.

## Before We Begin

- The CSDM is an evolving framework. Currently, the CSDM stands at version 4, with version 5 in draft. The crucial lesson here is to remain adaptable—neither fixated on completion nor frustrated by new versions announced by ServiceNow. This does not mean scrap and rebuild each time. Rather take the time to better understand the various components that make up the CSDM and then stick with a model that best fits your organization.
   
- Individuals rarely approach a project without preconceptions and ideas. Life experiences shape our perspectives which in-turn shapes how one goes about future enhancements. Expect discussions comparing CSDM to other product models and solutions. The key lies in active listening for requirements and avoiding emulating other products within the ServiceNow ecosystem. Let ServiceNow maintain its unique identity and move away from the other platform.
   
- Leadership may underestimate the complexity of configuring and setting up CSDM. Exercise patience and maintain a calm, matter-of-fact demeanor. Educate them on the process while emphasizing realistic expectations. Normally the expectations are around how long this will take. The mileage will vary from organization to organization, but it is common for this to take years to build out from scratch. They key is knowing where to start.

# Services

At the core of every organization are the services it provides, as these services are the primary way organizations do business. Whether it is a product-based manufacturing organization, a consultancy providing expert advice, or a tech firm delivering software solutions, the services offered are essential to the business's value proposition. This concept is central to the CSDM, which emphasizes that every position, regardless of role, provides a service whether that be internal or external to the organization. 

> __Sidebar:__ When defining services at your organization, you are going to receive push-back. Remember, everyone that receives a paycheck is being compensated for providing a service to the organization. In the 1999 movie _Office Space_, two consultants—Bob Slydell (played by John C. McGinley) and Bob Porter (played by Paul Wilson)—interview an office worker named Tom Symkowski (played by Richard Riehle). Their goal is to better understand everyone's role within the company to then help with efficiency. The scene is hilariously depicted as the two Bobs try to grasp what Tom actually does. After a back-and-forth banter, Bob Porter finally asks, _'What is it that you actually do here?'_ Tom then launches into an unhinged tailspin rant. If you haven't seen the movie, consider watching it before continuing with this guide. It is a good representation of what might come when having these discussions.

## Service Types

There are three primary service types in ServiceNow; application, business, and technical. These three form the broader groupings of services you will define and all are extended from the Configuration Item [cmdb_ci] table. Each service type has their own definition and role within the CSDM to help map out business operations.

> __Sidebar:__ Configuration Items (CIs) are typically misunderstood. Most see CIs as workstations, servers, printers, and various misc. equipment. A CI is just a undefined widget within the Configuration Management Database (CMDB). Not to get too confusing, but a CI is a placeholder for whatever you want it to be. Both a workstation and a service are CIs. Hence, in ServiceNow all service tables derive from the [cmdb_ci] base table. So when someone says _"can you send me a copy of your CIs?"_ You need to clarify what CIs in particular they are looking for.

### Technical Services

__Technical Services__ defines the "what" is being provided as a service. _(Notice, I did not include "how", this is very important and will come into play later on.)_ Technical Services should not be interpreted solely as "technology", but rather as "technically", in the sense of _"technically, this is what I know"_ or _"technically, this is what I provide."_ The focus should be on what teams and individuals know or provide when defining technical service offerings. This is not to say Technical Service definitions cannot be technology; they can. For example Microsoft Active Directory can be defined as a Technical Service just as Office Management can be as well. It is important not to confuse these with business services, as they are entirely different.

> __TL;DR__ Technical Services is more about what you know or what you provide; not how you do it. 

### Application Services

__Application Services__ are not related to software applications. _(Take a moment to let that sink in.)_ Remember that the CSDM is an evolving and ever maturing framework introduced by ServiceNow. The name of this service type did not accurrately convey its intended meaning. When ServiceNow chose the word Application, the means was more inline to the academic sense of "to apply." _(One of the early authors of the CSDM expressed regret over not naming it "System Services," but I believe that would not have been a better choice.)_

Instead of thinking of Application Services as software applications, consider Application Services as Deployed Services (though I will not use this term officially, it helps word illustrate the concept more concisely). Application Services represent the deployments (or instances) of a solution within the organization. Another way to view Application Services is as actualizations, meaning what is real in practice. This is not to say Application Service definitions cannot be applications or software; they can. For example Domain Controllers can be defined as a Application Service just as Frankfurt Office can be as well. Domain Controllers are the actualization and deployment of the Microsoft Active Directory solution. Frankfurt Office is an actual office location for the organization.

#### Technical Services and Application Services Overlap

Confusion can arise when Technical Services and Application Services overlap. For instance, Microsoft Active Directory can be classified as both. As a Technical Service, it represents the general expertise professionals have about Microsoft Active Directory regardless if the solution is installed or not. In contrast, as an Application Service, it pertains to the deployment of Microsoft Active Directory on domain controllers within the organization. So it it okay if you have dual Technical and Application Service definitions. See __Unique Names__ below when addressing overlapping services.

### Business Services

__Business Services__ address the broader question, _“What can the organization accomplish?”_ These services consolidate all Technical and Application Services into groupings that provide comprehensive answers to overarching organizational questions. Essentially, Business Services are designed for understanding at the director level and above. Examples of Business Services include Financial Services, Marketing, Sales, Manufacturing, Legal Services, among others.

> __Sidebar:__ While defining Business Services is crucial for understanding the larger landscape of the organization, they are typically the last to be defined and are considered lower in priority. This is because Technical Services and Application Services form the foundational elements that constitute Business Services. Therefore, it is more effective to focus initially on defining Technical and Application Services.

## Service & Service Offerings

There are two distinct definition types for both Technical and Business Services: __Service__ and __Service Offering__ definitions. This means there are records for Technical Services and Technical Service Offerings, as well as Business Services and Business Service Offerings. For detailed definitions, please refer to ServiceNow’s documentation on what constitutes a Service and a Service Offering.

To simplify the formal ServiceNow definitions:

- A __Service__ is a collection of Service Offerings.
- A __Service Offering__ represents the most granular level of service within its Service.

__TL;DR:__ Services are the containers, and Service Offerings are the individual items within those containers.

While the concept of Service and Service Offering is straightforward, practical implementation can be complex. These complications often arise from differing leadership perspectives on their respective towers or domains. Depending on who you ask, you may receive varying answers. Additionally, a change in leadership can bring a new perspective on services. The complexity increases because, in ServiceNow, Services and Service Offerings are linked through a base table with a parent reference field. This structure offers flexibility to model complex service relationships but can make development, maintenance, support, and reporting quite tedious.

The goal is to select a model and advocate for consistency. Here are a few key points to keep in mind:

1. Can you document the model in a way that will make sense for the next ServiceNow administrator?
1. Can your customers find the service offerings quickly and easily?
1. Can you generate a report dashboard that equally weighs and measures each service and service offering?

## Professional Advice

Here are some points for consideration. These are not meant as a _go and do_, but rather _for your consideration_. And as always, your mileage may vary.

### Nouns & Verbs

When setting up service definitions, it is crucial to use nouns only, avoiding verbs in the naming conventions. Service definitions should be either generalized nouns or proper nouns, with no verbs included.

- Adopting this policy helps prevent duplication of verbs across different services. For instance, the verb "add user" could apply to multiple services such as Active Directory, SAP, PeopleSoft, Microsoft Windows Desktop, and ServiceNow. Similarly, "grant access" could be relevant for Adobe, CAD/CAM systems, OracleDB, and Fidelity.

- Verbs should be reserved for creating catalog items, which are the actionable representations of services. Catalog items use verbs to describe specific actions that can be performed, such as "add," "provision," "reclaim," "delete," "security remediation," "patch," "update," and "upgrade."

__Leadership Alert:__ When setting up services and service offerings, it is important to communicate the concept of using nouns for service definitions to leadership. Leadership may instinctively want to include verbs in service definitions, such as "grant," "revoke," "reset," etc. However, using verbs in service definitions can lead to an overwhelming number of service offerings. This creates challenges in maintaining consistency, as it becomes difficult to decide when to use terms like "add," "provision," or "create." Leadership often wants visibility into the actions (verbs) their teams are performing. It is essential to explain that catalog items capture these verbs and that reports can be generated based on request items and their relationships with catalog items. This approach ensures clarity and consistency in service definitions while providing the necessary insights into team activities.

### Unique Names 

A potential problem your organization might run into is duplication of service names; which ServiceNow does allow. What happens when:

- you have multiple teams or sub-teams that provide tiered support for the same service?
- you have multiple IT departments within multiple companies within the same organization?
- you have multiple teams in different locales supporting the same service?

This introduces confusion when selecting services when services have the same name. (See _Ticket Routing_ below.) A drop-down list showing the same service names could have the user selecting the wrong service that could impact automation and ticket handling.

Though service records contain various fields, the services cannot cover all the various scenarios that make your organization unique. The go to for some is to create custom fields and there are cases where this would be applicable. If you forsee duplicate service names, consider using additional attributes to the name field to differentiate services and setting up standards in your organization. 

I prefer a _pill_ or _block_ approach for naming standards. A _pill_ uses parentheses () and a _block_ uses square brackets [] to append tags to service names to make them unique; either will suffice in ServiceNow. Within the pill or block, you set the standard of tags to help uniquely define the services.  

The naming standard below is just an example based on needing to support multiple companies inside a single organization on the same instance of ServiceNow. Your mileage will vary based on your organization’s structures. Take this an idea and implement your own version that best fits your organization's style.

Below gives examples for an naming standard. It is possible to use custom fields on the service records that represent various aspects of uniqueness and then create a __Business Rule__ that updates the name of the service. (See _Service Naming Standard Implementation_ below.)  

#### Technical Service Naming Standard

The syntax for technical services:
        
```<name> (<company stock symbol>|<department code>)```

where:
- __\<name\>__ is the name of the service
- __\<company stock symbol\>__ is the company 3-letter designation; see _Stock_ field in the Company [core_company] table
- __\<department id\>__ is the department id; see _ID_ field in the Department [cmn_department] table

Notice the standard uses fields from both the Company and Department table, but not directly referncing the records. Avoid refencing records that could change overtime for services; such as referencing vendors that could change overtime.

Using department id is helpful when multiple departments have simalar service names. As an example “Security” could be used for both physical security at facilities and cybersecurity for IT. “Engineering” could be used for manufacturing, R&D, and PLM for IT. 

#### Technical Service Offering Naming Standard

The syntax for technical service offerings:
        
```<name> (<company stock symbol>|<department code>|<T#>)```

where:
- __\<name\>__ is the name of the service offering
- __\<company stock symbol\>__ is the company 3-letter designation; see _Stock_ field in the Company [core_company] table
- __\<department id\>__ is the department id; see _ID_ field in the Department [cmn_department] table
- __\<T#\>__ is the support tier number

Regarding tier number, tiers are integers that start at 1 and goes to N. When defining services with tiers, make sure the tiers are consecutive with no gaps; gaps add confusion and doubts. Also, as an advanced tip, tier 0 should be reserved for automation. When designing workflows/flows that perform automation, make sure that tickets that have service fields use tier 0 services to denote automation operations.

#### Service Naming Standard Implementation

### Users vs. Groups

Maintaining a long term maintenance and support strategy should include how to keep ServiceNow current as organization changes take place. When setting up services, have a strategy to establish ownership pointing to groups instead of individual users. The reason is employee role changes and turnover would require constant upkeep on what records to update. Wether you pull in user and group information from an identity provider or use local ServiceNow records, this is a wise rule to consider. This shifts the maintenance and support away from the ServiceNow team to the group managers to maintain membership.
 
The base Service [cmdb_ci_service] table contains 10 user reference fields. A better strategy is to create custom fields that reference the Group [sys_user_group] table instead. The only field suggested to leave as a reference to an individual user is the _Attested By_ field. Attestment typically requires a specific account for audit purposes. Check with your business audit process before making changes.

### Available For & Not Available For

Just as not all catalog items are available to everyone, services have targeted consumers/customers as well. Then is makes sense that services should be scoped for targeted available for accounts.
 
ServiceNow has a field on the base service table [cmdb_ci_service] called _Users supported_ that references the table [sys_user_table]. This requires the organization to maintain a user group for the service. Consider leveraging the _User Criteria_ table [user_criteria] for greater level of flexibility and long term support with less managerial overhead in comparison to a standard group. 

__One way__ (easier) is to create two custom fields to the base service table [cmdb_ci_service] called _Available for_ and _Not Available for_ that is a List collector referencing _User Criteria_ table [user_criteria]. __Another way__ (harder) is to create two custom tables _Service Available for_ [u_cmdb_ci_service_user_criteria_mtom] and _Service Not Available for_ [u_cmdb_ci_service_user_critera_no_mtom].  

#### Available For & Not Available For Setup Instructions

__FILL ME IN__

### Ticket Routing

To address the issue of ticket misrouting, consider leveraging services instead of relying on assignment groups when assigning tickets. Relying on assignment groups is prone to difficulties when organizations change, when service responsibilities change between teams, and when external partnerships change. It is common for tribal knowledge to form where tickets are assigned to teams, but what happens when that service is no longer handled by that team? 

As a hypothetical example, an internal facilities team has handled parking lot security for years addressing reported incidents and fulfilling requests. As the number of facilities has increased, so has the ticket overhead, streching the team to seek alternative solutions. To keep costs down the organization has hired an external contracting firm to take over parking lot security and the internal facilities team can field all other services. Then in ServiceNow two new assignment groups are created, one for the internal facilities team and one for the external security parking team. If the organization relies on assigning tickets based on assignment groups, many people will continue to open parking lot security tickets against the wrong team. Also, all of the catalog items, workflows, flows, business rules, etc. etc.. will have to be updated as well that point to the internal facilities team.

The best course of action is to force all tickets and automation to use services as a means to perform ticket assignment. This ensures tickets are routed to the appropriate team that handles the service requested. In order for this to work there has to be enough services pre-defined for ticket routing to be successful.

#### Ticket Routing Setup Instructions

__FILL ME IN__

### Service Tiers

A __service tier__ typically refers to the level of support provided, often categorized by the complexity and urgency of the issues being addressed. Here’s a breakdown of common support tiers:

1. __Tier 1 (Level 1)__: This is the first line of support, often handling basic customer inquiries and issues. Support staff at this level provide general assistance, troubleshoot common problems, and escalate more complex issues to higher tiers if necessary.

2. __Tier 2 (Level 2)__: This level deals with more complex issues that Tier 1 cannot resolve. Support staff here have more technical expertise and can handle in-depth troubleshooting, configuration issues, and more detailed problem-solving.

3. __Tier 3 (Level 3)__: This tier involves highly specialized support, often provided by experts or engineers. They handle the most complex and critical issues, including those that require deep technical knowledge or development skills. They may also work on bug fixes and system improvements.

4. __Tier 4 (Level 4)__: Sometimes, there is a fourth tier, which involves external support from vendors or partners. This tier is engaged when the issue requires specialized knowledge or access to proprietary systems and tools that the internal support team does not have.

These tiers help ensure that customer issues are resolved efficiently and effectively, with the right level of expertise applied to each problem.


## Example Service & Service Offerings

In these examples we will model a generic organizations Technical Services and Technical Service Offerings. This organization decided on 1:2 model where there would be two levels of Technical Service Offerings per Technical Service.

For exammple:

- Technical Service (1st level)
	- Technical Service Offering (2nd level)
		- Technical Service Offering (3rd level)

I will call out the 1st thru 3rd level to help designate at which level we will define the services.  

### Identity & Access Management

Most organizations have one or more teams dedicated to Identity and Access Management. The Identity and Access Management (IAM) service is responsible for managing user identities, authentication, and access control across the organization. This service ensures that only authorized users have access to the necessary resources, enhancing security and compliance.

The Technical Service (1st level) would be defined as _Identity & Access Management_.

Within _Identity & Access Management_, the organization has broken down their Technical Service Offering's 2nd and 3rd level and they are:

- Active Directory Management
	- User Account Management
	- Group Policy Management
	- Directory Services
	- Access Control
- Lightwieght Directory Access Protocol
	- Directory Services
   - Directory Management
- Authentication
	- Single-Sign On
	- Kerberos
	- SAML
   - Radius
	- Priviledge Access Management

 

- __Easy to understand standard__: A simple relationship between all Services and Service Offerings is straightforward. Once you get into multiple levels the maintenance of the organization of the records can get confusing. ServiceNow allows for a complex tree-structure when it comes to all services, the problem becomes obvious overtime, as roles and organizations change. In short, you are smart, but your predecessor might not be. So keep it simple.
- __Simple lookups__: There is a balance between easy to search and easy to find. They are two completely different definitions in the context of user experience (UX). On one hand, having the scroll through volumes of records can be very tedious. And then also having to hunt for what you are looking for with multiple mouse-clicks can also be tedious as well. By having a two-tier relationship between Services and Service Offerings you are addressing both the search and find aspect of UX. Searches can easily be narrowed down to two searches; one for the right Service and one for the right Service Offering. Instead of complex tree searches. Finding through the act of mouse-clicks is narrowed down to two mouse-clicks.
- __Easier reporting__: An area that constantly gets overlooked is reporting. You want reports to represent information equally and consistently. By having a two-tier relationship between Services and Service Offerings, reports will be far easier to understand without having to understand embedded sub-services; which in the long run, we have found really does not result in better outcomes or decisions.

__Key Point:__ Keep Service and Service Offerings simple. The CMDB is already complex enough, do not make more work for yourself by making the the CSDM complex too.


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
