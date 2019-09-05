---
title: How to Review A Spec (as an Engineer)
description: Guidlines of how to provide good feedback in spec review meetings
author: misampso
ms.author: misampso
---

# How to Review A Spec (as an Engineer)

This guide will serve as a living document to ensure that the Engineering team can provide robust feedback to PMs as they write specs. Our goal here is to ensure that we have a reference for common questions we need to ask as we consider work that may be picked up. It is the job of all engineers to ensure that before we accept a spec as "complete" we ensure that it is truly ready to start coding

> [!NOTE]
> This document covers just Spec documents and not Vision documents. They are high level and far reaching such that this level of detail is not needed yet. Some of these may apply there as well but most will be too specific.

## Updating this document

As we continue to review specs, we will find lenses that we need to add to this list as well as new questions under existing lenses. If you find something that you want to add here, open a pull request and tag your manager and the author of this document for review. At any given time, this should serve as the best reference for ensuring that we get the details in specs we need.

## Spec Review Lenses

This document divides each set of questions/concerns that we need to cover into different categories that we call "lenses". Each lens focuses on a specific area that each spec needs to cover in order to be considered complete. In spec review meetings, use these lenses to ensure that the spec covers everything we need to begin work on the item. All of these are considered requirements for specs to be accepted so if you don't see something covered, bring up the lens and ask for it to be added!

### Compliance

Questions in this lens cover various requirements that all feature we develop must have. Not all of these apply to everything we build but we need to ensure that we don't forget to cover these items just to get something done or shipped.

* Is this feature accessible?
* Does this feature have implications for our GDPR compliance?
* Is this feature designed to be secure and follow Microsoft SDL guidlines?

### Division of Labor

This lens works to ensure that the right people on the team tackle the right problems. A spec is foremost concerned with "what" a feature is supposed to be and should avoid diving too deep into "how" it is built or what it looks like without input from engineering or design.

* Does this spec use images or wireframes that speculate about final UI rather than user flow diagrams?
* Does the spec include architecture or service design that hasn't been coordinated with Engineering?

### User Cases

With this lens we work to ensure that the spec covers the totality of user flow and not simply the core functionality or "happy path." When evaluating through this lens, don't go too deep on error cases that are general problems of web sites but rather things an average user may run into when working with this feature.

* What does the feature do if the user makes an invalid request?
* What happens when the feature returns zero results?
* What error messages do we communicate to the user?
* What if an operation takes a long time? How do we communicate progress?
* What if a user makes an unconventional choice?
* What if a user cancels an action?
* How does a user correct a mistake?

### Web Basics

Web Basics is all about including all of the things we need in a spec that is describing a web site. These are small things that are often forgotten when talking about the big picture of a feature.

* If this spec covers new pages, what are their URLs?
* Does this feature require something that breaks our design guide like a popup?
* Are any basic rules of the web broken here?
* How does this feature interact with our headers, footers, and sidebar?
* How does this interact with dark theme?

### Ownership and Responsibility

Features may require some continuous admin and maintenance even after they are shipped and this lens ensures that we have a plan for that. It also covers ensuring that features that impact a lot of users or authors have a plan to move into production that is not disruptive.

* Who manages configuration data for this feature such as white lists, black lists, or global settings?
* Who has the responsibility to control if this feature is turned on or off and how it applies to different docsets?
* Do we need an admin UI that allows those in control to make changes without involving Engineering?
* What is the rollout plan for this feature to ensure minimal disruption of authors and users?
* Does this require an CI/CD jobs to be created and monitored?
* How to people "onboard" to this feature?

### Content Team Impact

Most features we build are targeted at end users but have an impact on the authors that create the content to fill out the experience. This lens covers ensuring that when we build something for our external users we don't forget about our critical internal ones.

* How does this impact authors working on their content today?
* Where is the content plan for new content described in this feature? (ex. modules for Triple Crown)
* Where will the final documentation for authors live?
* Do we need to update or create author tooling for this feature?