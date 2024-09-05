# DRAFT DRAFT DRAFT

# CSDM in Practice
A practical guide implementing ServiceNow CSDM

Version 20240825 

# Safe Harbor Statement

This guide provides one approach to implementing the Common Service Data Model (CSDM) on the ServiceNow platform. However, I'm not bold enough to claim that it's the definitive way. The CSDM framework continually evolves through the efforts of engineers at ServiceNow, and if anyone can state the exact way to implement CSDM, it's them. Therefore, your mileage may vary in terms of takeaways from this guide. Your best path forward lies in listening, understanding, and collaborating with others.

# About

As someone working at a moderately large manufacturing multi-conglomerate, I've had the opportunity to collaborate with eight distinct business units (which I refer to as clients). Each client operates independently within the broader context of the business, with its own IT department. My role involves ensuring that all eight clients use the same instance of ServiceNow.

In this guide, I'll share real-world examples of how to implement the Configuration and Service Data Model (CSDM) effectively. Whether you're part of a large organization or a smaller team, these insights can help streamline your processes and enhance collaboration. 

> __TL;DR:__ In this guide, I share practical insights on implementing the Configuration and Service Data Model (CSDM) within your organization. Drawing from real-world examples, I discuss how to streamline processes and enhance collaboration across distinct business units. Whether you're part of a large enterprise or a smaller team, these insights can help you effectively use ServiceNow.

# Introduction

One of my favorite post-movie ending scenes comes from the Walt Disney film _Finding Nemo_. In this clip, a band of fish escapes from their aquarium and returns to the ocean, only to discover that they are still trapped in their plastic baggies floating ontop of the water. The scene ends with the poignant question, _'Now what?'_ The reason why this scene resonates is we have all been there. Whether you're a customer or a partner, we've all been there. After the flashy presentations, dazzling demos, and once the ink has dried on the digital contracts, it's time to actually implement what has been purchased.

Anytime a solution is purchased, the number 1 question on the minds of leadership is _"when will the solution be up and running?"_ Ultimately they want to see return on investment as soon as possible. Now of course the answer varies on many factors like leadership involvement, staff knowledge and utilization of ServiceNow partners. If you are reading this you are either in the position of pre-implementation or post implementation of ServiceNow. The later is a bit more effort to make changes, but it is possible and I will lay out strategies where it makes sense when converting over.  

The architects and engineers at ServiceNow have developed an excellent model called the Common Service Data Model (CSDM). This model helps organize and enhance our understanding of the various services offered by our organization and the best place to start from when implementing the ITSM module of ServiceNow. The CSDM does integrate with other various modules of the ServiceNow platform as well, which I am still exploring and implementing.

## Why

Before we begin, it's important to address the why question: _"Why go through the effort of setting up the Common Service Data Model (CSDM) in ServiceNow?"_ The best way to answer this is through my own experience.

- Prior to adopting ServiceNow, my employer faced challenges with basic IT practices that negatively impacted customer satisfaction and caused long-term internal friction. One of the most noticeable issues consistently brought up was ticket misrouting, which led to long delays in delivering services to customers. This frustrated customers driving down IT customer satisfaction. For me, tt was clear that we couldn't carry the "sins of the past" into the future with ServiceNow.

- I had an incredible mentor, and upon his retirement, I asked him, _"What is the biggest hurdle IT has yet to overcome during your tenure at the organization?"_ His response was, _"The CFO has yet to fully understand the value IT brings to the organization, because IT has yet to fully understand all the services it provides."_ As a cost center, IT departments are often seen as only consuming money rather than generating it. In a large and diverse organization, this perception is exacerbated as IT costs continue to rise and technology becomes more common place across all things.

I started with a simple two-part problem statement: _"How do we prevent ticket misrouting?"_ and _"How do we demonstrate IT's value?"_ The CSDM provided all the necessary components and definitions, but lacked specific instructions. I filled in the gaps based on personal experience, intuition, and a dedicated team focused on solving these fundamental issues. As a result, our ticket misrouting is now at an all-time low, and we have developed reports that clearly show the value IT brings to the organization.

> __TL;DR__ The CSDM can feel a bit like a complex board game with a thick manual that talks about all of the pieces and the board, but there are no instructions on how to play. So, let's roll up our sleeves and I will show you how to play the game.

## Baseline

If you are unfamilar with ServiceNow's CSDM, I recommend you first search for it online and read what the authors’ have to say, before you read what this guide has to offer. I cannot provide a link because the way ServiceNow keeps their documentation evergreen and any link today will be outdated tomorrow without notice. A simple “ServiceNow CSDM” Internet search will suffice. From there, you can get all the formal definitions and explanations directly from the source. No reason for me to copy-paste what is already publicly and freely provided.

My take is the CSDM serves as an organizational framework for business operations that shows the holistic view of an organization's services. Which highlights their interdependencies, facilitating efficient management and strategic alignment. Analogous to personnel org charts that depict connections between individuals, the CSDM provides a model to illustrate service relationships within an organization. From the smallest assets—such as a shop floor thermostat—to high-level business operations at the C-suite, the CSDM establishes links across the entire spectrum.

## Before We Begin

- The CSDM is an evolving framework. Currently, the CSDM stands at version 4, with version 5 in draft. The crucial lesson here is to remain adaptable—neither fixated on completion nor frustrated by new versions announced by ServiceNow. This does not mean scrap and rebuild each time. Rather take the time to better understand the various components that make up the CSDM and then stick with a model that best fits your organization.
   
- Individuals rarely approach a project without preconceptions and ideas. Life experiences shape our perspectives and then them to future enhancements. Expect discussions comparing CSDM to other product models. The key lies in active listening to requirements and avoiding emulating other products within the ServiceNow ecosystem. Let ServiceNow maintain its unique identity and move off the other platform.
   
- Leadership may underestimate the complexity of CSDM setup. Exercise patience and maintain a calm, matter-of-fact demeanor. Educate them on the process while emphasizing realistic expectations. Normally the expectations are around how long this will take. The mileage will vary from organization to organization, but it is common for this to take years to build out from scratch. They key is knowing where to start.

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

There are two distinct definitions for each service type mentioned above: __Service__ and __Service Offering__. For detailed definitions, please refer to ServiceNow’s documentation on what constitutes a Service and a Service Offering.

To simplify the formal ServiceNow definitions, a __Service__ is a collection of Service Offerings. A __Service Offering__ represents the most granular level of service within its Service. 

> __TL;DR:__ Services are the buckets and Service Offerings are the marbles in the buckets. 

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

- __Easy to understand standard__: A simple relationship between all Services and Service Offerings is straightforward. Once you get into multiple levels the maintenance of the organization of the records can get confusing. ServiceNow allows for a complex tree-structure when it comes to all services, the problem becomes obvious overtime, as roles and organizations change. In short, you are smart, but your predecessor might not be. So keep it simple.
- __Simple lookups__: There is a balance between easy to search and easy to find. They are two completely different definitions in the context of user experience (UX). On one hand, having the scroll through volumes of records can be very tedious. And then also having to hunt for what you are looking for with multiple mouse-clicks can also be tedious as well. By having a two-tier relationship between Services and Service Offerings you are addressing both the search and find aspect of UX. Searches can easily be narrowed down to two searches; one for the right Service and one for the right Service Offering. Instead of complex tree searches. Finding through the act of mouse-clicks is narrowed down to two mouse-clicks.
- __Easier reporting__: An area that constantly gets overlooked is reporting. You want reports to represent information equally and consistently. By having a two-tier relationship between Services and Service Offerings, reports will be far easier to understand without having to understand embedded sub-services; which in the long run, we have found really does not result in better outcomes or decisions.

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
