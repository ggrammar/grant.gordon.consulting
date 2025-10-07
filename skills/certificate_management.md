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

## Types of Certificates

I've been running webservices for the better part of a decade, so I think of certificates from the point-of-view of a webserver. I think of certificates as either "inbound" or "outbound" certificates. 

Inbound certificates:
 - Are used for client-initiated traffic that is inbound to our server. 
 - Are simple to monitor - check the URL and you're good to go. 
 - Are typically installed on a webserver (like nginx, apache, or a Kubernetes ingress controller)

Outbound certificates:
 - Are used for traffic that we initiate, outbound to 3rd parties. 
 - Are more challenging to monitor - we need to know about every certificate store for every app on every server. 
 - Are typically installed by default on your operating system (although we may install additional certificates, depending on who we need to talk to)

Both types of certificates are used to encrypt traffic between two parties, but we need different approaches to manage them. 

## Monitoring Inbound Certificates

Inbound certificates are typically installed by an administrator:
 - For nginx, we have the `ssl_` [family of directives](https://nginx.org/en/docs/http/configuring_https_servers.html). 
 - For apache, we have the `mod_ssl.so` [module](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html).
 - Kubernetes supports a number of [ingress controllers] like [nginx](https://kubernetes.github.io/ingress-nginx/), [Traefik](https://doc.traefik.io/traefik/reference/install-configuration/providers/kubernetes/kubernetes-ingress/), and [HAProxy](https://www.haproxy.com/documentation/kubernetes-ingress/overview/).
 - [AWS](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/https-listener-certificates.html), [GCP](https://cloud.google.com/load-balancing/docs/ssl-certificates), and [Azure](https://learn.microsoft.com/en-us/azure/app-service/configure-ssl-certificate?tabs=apex%2Crbac%2Cazure-cli) offer load balancers with managed certificates. 

If you have outages caused by expired certificates, monitoring the expiration date of your inbound certificates is a great way to improve reliability. This monitoring is simple to implement, even if you don't already have a purpose-built tool. If you later decide to automate certificate rotation, this monitoring can let you know if the automated process failed.

The first thing you'll need is a list of all of your inbound certificates. Check every environment, every virtual host, every load balancer, every FTPS server, every service. Let everyone involved in certificate management know that _this is the list_. You won't get everything on the first go-around, so it's important that folks know to update this list.

Then, use a tool that can consume the list to check each certificate. Some monitoring tools (PRTG, ServiceNow, Nagios, Zabbix, etc) have built-in certificate monitoring capabilities. If you don't have such a tool, it is simple to write a script.

Be sure that your tool checks all the certificates in the chain. It's possible for the end-entity cert to be unexpired, only for an intermediate or root certificate to be expired, which will cause a similar outage.

## Monitoring Outbound Certificates

Outbound certificates are typically installed by default on your operating system:
 - For Windows, Microsoft operates the [Microsoft Trusted Root Program](https://learn.microsoft.com/en-us/security/trusted-root/program-requirements).
 - For Apple operating systems, Apple operates [Root Stores](https://support.apple.com/en-us/103272).
 - For some Linux distributions, Mozilla operates the [CA Certificate Program](https://wiki.mozilla.org/CA) (you may recognize the ca-certificates package for Alpine Linux, which Mozilla maintains). 

 If you're staying on top of operating system patching, you probably don't need to worry about outbound certificates. 

 If you are installing additional certificates xyz (like a private CA for your organization, or to communicate with a vendor), it might be worth the effort to monitor these. 

## Unorganized notes. 

I like to use something like https://www.ssllabs.com/ssltest/ 

Cipher suites are a common requirement for audits like PCI or HIPAA, not directly related to certificates. 

Discuss automation, especially with a tool like Let's Encrypt - where that is (and is not) appropriate.

