---
layout: default
---

## Cloud Cost Optimization

Software-as-a-Service, Platform-as-a-Service, Infrastructure-as-a-Service - anything as-a-service can get expensive! If you haven't audited your cloud spend recently, it's likely you can save tens or hundreds of thousands of dollars a year with a few days of work. 

The basic playbook is simple:
 - Get access to the billing console for the service you'd like to audit.
 - Sort by cost, determine what's historically the most expensive. 
 - Reduce cost by making changes. 
 - Repeat until you're satisfied with the cost. 

This can be easier said than done! Some services make it challenging to determine where exactly your money is going. At larger organizations, cloud compute platforms like AWS, GCP, and Azure are complex enough to warrant a full-time FinOps team. 

I've gathered some guidance below for services that I have specific experience working with. If you have a cloud bill that you'd like me to look at, please reach out! I really enjoy cloud cost optimization - you can get a very clear return-on-investment in a matter of days or weeks, and it's particularly satisfying work. 

## Specific Guidance

### Dynatrace

You can access the Dynatrace "Account Management" page at https://myaccount.dynatrace.com/. If you have the correct permissions, you should be able to see the "Subscription" drop-down, which is where your billing information is stored. 

 - https://www.dynatrace.com/pricing/rate-card/
	- Infrastructure Monitoring is usually cheaper than Full-Stack Monitoring, but merits some tuning. 
 - RUM (Real-User Monitoring) can get expensive. Only turn it on where you need it. 
 - some of the list prices are per-hour. divide by 24 to get a sense of how much of that thing you're monitoring

### AWS

 - General cloud compute guidance - pay ahead rather than pay-as-you-go, right-sizing, delete old infrastructure
 - Aggregate billing in one account
 - Don't forget to look at all regions

### GCP

 - General cloud compute guidance - pay ahead rather than pay-as-you-go, right-sizing, delete old infrastructure

### Azure

 - General cloud compute guidance - pay ahead rather than pay-as-you-go, right-sizing, delete old infrastructure

## General Guidance

Right-sizing - can be more effort than it's worth
