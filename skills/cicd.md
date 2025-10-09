---
layout: default
---


# Building and Deploying Software (CI/CD)

Much like "DevOps", "CI/CD" means a lot of different things to a lot of different folks. At some places, I've seen for-real continuous integration and continuous deployment - ArgoCD running in a Kubernetes cluster constantly testing and applying the latest changes from git. At other places, "CI/CD" was a manually-triggered Jenkins pipeline that released to production once a month. "CI/CD" has become a convenient shorthand for "the process by which we build, test, and release our software". 

## Source Control

I've worked with a variety of source control systems. In rough order of preference:
 - GitHub (which is where this website's code is hosted!)
 - Bitbucket Cloud
 - Azure DevOps
 - Serena PVCS (a locking version control system - not great, but better than nothing)
 - Loose files on a file server

With the exception of Serena PVCS and loose files, these are based on git, the industry standard for source control. 

## Build

 - cloud or self-hosted runners
 - important - should produce a build artifact that you can release to all of your environments
	- do not build a unique artifact for each environment
	- externalize any config

## Test

## Deploy

 - likely using the same runners you use to build the softare

# Unorganized Notes

What else do I want to put here? Getting started kit for building a process around this? Links to "documenting a process" page. 

Discuss Jenkins, Bamboo CI/CD, custom build systems, one person with a collection of PowerShell scripts, Drone, 
