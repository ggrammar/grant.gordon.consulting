---
layout: default
---

## Certificates

A certificate helps secure communication between two computers. For example, this communication might be:
 - Between a prospective customer and your website
 - Between a vendor and your file transfer server
 - Between different pieces of your internal infrastructure

Certificates expire over time, so it's important that they're periodically rotated. Expired certificates can be a recurrent source of outages, especially if the certificates are short-lived. Certificates used to be valid for years or decades, but we are moving towards certificates that will [only be valid for a month or so](https://www.digicert.com/blog/tls-certificate-lifetimes-will-officially-reduce-to-47-days). 

In my experience, even people who are not very technical are aware of certificate expiration as a source of outages. Outages caused by expired certificates can be a black eye for an engineering organization - a certificate has a clear expiration date, so you'd think it would be easy to prevent that sort of outage. 





## Playbook

If you often have outages caused by expired certificates, monitoring the expiration date of your certificates is a great way to improve reliability. This monitoring is simple to implement, even if you don't already have a purpose-built tool. If you later decide to automate certificate rotation, this monitoring can let you know if the automated process failed. 

The first thing you'll need is a list of all of your certificates. Let everyone involved in certificate management know that _this is the list_. You won't get everything on the first go-around, so it's important that folks know to update this list.

Then, use a tool that can consume the list to check each certificate. Some monitoring tools (PRTG, ServiceNow, Nagios, Zabbix, etc) have built-in certificate monitoring capabilities. If you don't have such a tool, it is simple to write a script. 

Be sure that your tool checks all the certificates in the chain. It's possible for the end-entity cert to be unexpired, only for an intermediate or root certificate to be expired, which will cause a similar outage. 

## Unorganized notes. 

I like to use something like https://www.ssllabs.com/ssltest/ 

Cipher suites are a common requirement for audits like PCI or HIPAA, not directly related to certificates. 

Discuss automation, especially with a tool like Let's Encrypt - where that is (and is not) appropriate.

