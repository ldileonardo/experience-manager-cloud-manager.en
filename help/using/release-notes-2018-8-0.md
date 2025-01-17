---
title: Release Notes for 2018.8.0
seo-title: AEM Cloud Manager Release Notes for 2018.8.0
description: Follow this page to get information for Cloud Manager Release 2018.8.0.
seo-description: Follow this page to get information for AEM Cloud Manager Release 2018.8.0.
uuid: e8aaba32-89b4-4bc5-b295-09b753252612
contentOwner: jsyal
products: SG_EXPERIENCEMANAGER/CLOUDMANAGER
topic-tags: release-notes
discoiquuid: 9222ac3b-525e-47c1-b481-ac9d22e3d559
feature: Release Information
exl-id: 20f87048-30f7-4869-aad0-13ca383a404b
---
# Release Notes for 2018.8.0 {#release-notes-for}

The [!UICONTROL Cloud Manager] 2018.8.0 Release adds support for triggering the CI/CD pipeline automatically upon git commits and a new wizard for creating application projects in git based on the AEM Project Archetype.

## Release Date {#release-date}

The Release Date for [!UICONTROL Cloud Manager] Version 2018.8.0 is October 04, 2018.

## What's New {#what-s-new}

* **Program Setup** - New wizard to create an application project in git using the AEM Project Archetype 

* **CI/CD Pipeline** - The following changes are added to CI/CD Pipeline. Please refer to the document [Configure Production Pipelines](configuring-production-pipelines.md) to learn more.

  * On Git Changes trigger, which starts the CI/CD pipeline whenever there are commits added to the configured git branch.  
  * Cards on home screen now deep link into specific sections of pipeline execution page.
  * Activity page now lists the specific branch used for each execution.
  * Activity page now indicates the duration in hours and minutes.
  * Pipeline execution page now displays the version/tag name created for the execution.
  * Apache Maven version updated to 3.5.3.

* **Navigation** - The following changes are added to the [!UICONTROL Cloud Manager].

  * Resources link in global navigation will navigate to the Runbook in Sharepoint.
  * Help menu has been reorganized to include more [!UICONTROL Cloud Manager]-specific content.

## Bug Fixes {#bug-fixes}

* Some details in the Performance Testing dialog were not visible in Firefox.
* Timeouts between internal systems would occasionally cause deployment failures to be reported.
* The icon color on the Activity page was not correct for some successful pipeline executions.
* In some cases, the Performance Testing dialog listed the same metric multiple times.
* If a new instance was added after the CI/CD pipeline had started, the deployment would not be executed on the newly created instance.

## Known Issues {#known-issues}

* Branches created using the Application Project Wizard cannot contain dashes.
* The [!UICONTROL Experience Cloud] notification sidebar may not load notifications consistently. Notifications, however, are visible in the [!UICONTROL Experience Cloud] and, if configured, will still be sent via email.
