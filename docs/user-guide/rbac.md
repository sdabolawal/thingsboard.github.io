---
layout: docwithnav
title: Advanced role based access control (RBAC) for IoT devices and applications
description:  

---

{% assign feature = "Advanced RBAC for IoT devices and applications" %}{% include templates/pe-feature-banner.md %}

* TOC
{:toc}


## ThingsBoard CE vs PE security features comparison

ThingsBoard Community Edition (TB CE) supports straight-forward security model with three main roles: System administrator, Tenant administrator and Customer user. 
System administrator is able to manage tenants, while tenant administrator manages devices, dashboards, customers and other entities that belong to particular tenant.
Customer user is able to view dashboards and control devices that are assigned to specific customer.
TB CE functionality is sufficient for a lot of simple use cases, especially building real-time [end-user dashboards](/docs/user-guide/ui/dashboards/).
 
ThingsBoard Professional Edition (TB PE) brings much more flexibility in terms of user, customer and role management. 
It is designed to cover use cases for businesses and enterprises with multiple user groups that have different permissions but may interact with the same devices and assets. 

TB PE security model was significantly improved in v2.3 to enable new security features and to support advanced RBAC for IoT applications. For example:

  - ability to create hierarchy of customers with multiple levels of sub-customers, independent users and devices; 
  - ability to create roles with flexible set of permissions;
  - ability to assign roles to specific user groups;
  - ability to grant specific permissions to specific user groups over specific device groups;
   
This document covers features that are exclusive to TB PE. We will start with a glossary and will provide step-by-step examples how to configure most popular use cases.

## Glossary
 
**Tenant**

You can treat tenant as a separate business-entity: individual or organization who owns or produce devices and assets. Tenant may have multiple tenant administrator users and millions of customers.

**Entity**

Any entity managed by ThingsBoard. For example: device, asset, user, dashboard, entity view, etc. See [entities and relations](/docs/user-guide/entities-and-relations/) guide for more details.

**Entity Group (EG)** 

Entity Groups are groups of entities of the same type, for example: Device Group or Asset Group. Single entity may belong to multiple entity groups simultaneously. 
For example, thermostat device may belong to group "Thermostats" that contains all devices and more specific group like "Thermostats with FW v1.2.3". 

**Customer**

Customer may be a separate business-entity: individual or organization who purchase or uses tenant devices and/or assets. 
Customer may also be a division within Tenant organization. 
Customer may have multiple users, inner customers and millions of devices and/or assets.

**Customer Group (CG)**

Customer group is also an EG. It has the same features as regular EG, but we have a separate term for CG to be able to easily distinct CGs and all other EGs.

**User**

Users are able to login to ThingsBoard web interface, execute REST API calls, access devices and assets if allowed. User is also an Entity in ThingsBoard.

**User Group (UG)**

User group is also an EG. It has the same features as regular EG, but we have a separate term for UG to be able to easily distinct UGs and all other EGs.

**Owner**

Each EG belongs to one owner. This may be either Tenant or Customer. 
Also, each Customer has also only one owner. If the Customer Owner is Tenant, it means that this is a top-level Customer.
If the Customer owner is another Customer, it means that this is a sub-customer. There might be multiple levels of Customers in ThingsBoard.

**Resource**

Anything that has secure APIs or represents a ThingsBoard Entity is a resource. 
Examples of Entities are listed in the Entity definition above. Groups of entities are also resources, for example: Device Group, Asset Group, Dashboard Group. 
Additional resources are white-labeling, audit logs and admin settings.

**Operation**

Operations represent actions that you might perform over Resources. 
There are generic actions like "create", "read", "write", "delete", "add to group", "remove from group". There are also specific actions like "read/write credentials".

**Role**
 
Role contains a list of Resources and a list of allowed Operations for each of those resources. There are two Role types: Generic and Group.
There is a special resource "All" that is a shortcut to all available resource types. 
There is also a special operation "All" that is a shortcut to all possible operations.  
We will explain the differences between them later in this article.   
 
**Group Permissions Entity (GPE)**

GPE is basically a mapping between UG, Role and optional EG. See "Generic roles" and "Group roles" for more details.

## Customer hierarchy

ThingsBoard supports "recursive" customer hierarchy with unlimited number of sub-customers. 
The root level Owner is Tenant. Each Owner may have multiple Entity Groups (EGs), User Groups (UGs) and Customer Groups (CGs).

**Note:** Each Entity has exactly one owner. However, Entities may belong to multiple EGs that belong to the same owner.

Since CGs may contain multiple Customers, each Customer may also own his EGs, UGs and CGs (i.e sub-customer groups). 
See diagram below for visual representation of relations between those entities. 
 
TODO: paste diagram from https://docs.google.com/document/d/1soB_1tBFtWNieCuShO6c5N1CJmy6jtxcCku82WspHfY/edit#heading=h.dsgfhlasq7wk

## Generic roles

Each Role is related to one or more User Group. Each User Group has only one Owner. 
With Generic Role you grant UG with the same permissions over all entities that belong to the same Owner and all it's sub-customers recursively.
We use special "connection" object called Group Permission Entity to make a connection between User Group and Generic Role.  

Let's review the diagram below. 

User Bob will be able to perform any operations over any entity that belongs to either his Tenant A or Customer B as long as any other customers and sub-customers for the same Tenant.
However, User Alice will be able to perform any operations over any entity that belongs to only her Customer B and all it's sub-customers.
So, Alice and Bob are able to access Device B1, but only Bob is able to access Device A1.        

![image](/images/user-guide/security/generic-role-diagram.png)

## Group roles

Group Role allows you to map set of Permissions for specific User Group to specific Entity Group.
We use special "connection" object called Group Permission Entity to make a connection between User Group, Entity Group and Group Role.

**Note:** Since Entity Group has exactly one Owner, you can assign Group Role to any User Group that belongs to the same Owner or any parents of the Owner.  

![image](/images/user-guide/security/group-role-diagram.svg)      


## Examples and How-Tos 
    
## Next steps

{% assign currentGuide = "AdvancedFeatures" %}{% include templates/guides-banner.md %}

