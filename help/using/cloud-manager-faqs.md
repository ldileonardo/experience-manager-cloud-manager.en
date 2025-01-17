---
title: Cloud Manager FAQs
seo-title: Cloud Manager FAQs
description: Refer to Cloud Manager FAQs to get some troubleshooting tips
seo-description: Follow this page to get answers on Cloud Manager FAQs
feature: Getting Started
exl-id: 52c1ca23-5b42-4eae-b63a-4b22ef1a5aee
---
# Cloud Manager FAQs {#cloud-manager-faqs}

The following section provides answers to frequently asked questions related to Cloud Manager.

## Is it possible to use Java 11 with Cloud Manager builds? {#java-11-cloud-manager}

AEM Cloud Manager build fails when attempting to switch the build from Java 8 to 11. The problem can have many causes and most common ones are documented below:

* Add the maven-toolchains-plugin with the correct settings for Java 11 as documented [here](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/create-application-project/using-the-wizard.html?lang=en#getting-started).  For example, see the [wknd sample project code](https://github.com/adobe/aem-guides-wknd/commit/6cb5238cb6b932735dcf91b21b0d835ae3a7fe75).

* If you encounter the error below then you need to remove use of `maven-scr-plugin` and convert all OSGi annotations to OSGi R6 annotations. For instructions, see [here](https://cqdump.wordpress.com/2019/01/03/from-scr-annotations-to-osgi-annotations/).

   `[main] [ERROR] Failed to execute goal org.apache.felix:maven-scr-plugin:1.26.4:scr (generate-scr-scrdescriptor) on project helloworld.core: /build_root/build/testsite/src/main/java/com/adobe/HelloWorldServiceImpl.java : Unable to load compiled class: com.adobe.HelloWorldServiceImpl: com/adobe/HelloWorldServiceImpl has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0 -> [Help 1]`

* For Cloud Manager builds, the maven enforcer plugin fails with error `"[main] [WARNING] Rule 1: org.apache.maven.plugins.enforcer.RequireJavaVersion"`. This is a known issue due to Cloud Manager using a different version of Java to run the maven command versus compiling code. For now, omit `requireJavaVersion` from your maven-enforcer-plugin configurations.

## Our deployment is stuck because the Code Quality check failed. Is there a way to bypass this check? {#deployment-stuck}

All Code Quality failures except for *Security Rating* are non-critical metrics, so they can be bypassed by expanding the items in the results UI.  

A user with [Deployment Manager, Project Manager, or Business Owner](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html?lang=en#requirements) role can override the issues, in which case the pipeline proceeds or they can accept the issues, in which case the pipeline stops with a failure.  See [Three-Tier Gates while Running a Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html?lang=en#how-to-use) for more details.

## Cloud Manager deployments fail at the performance test step in Adobe Managed Services environments. How do we debug this to pass the critical metrics? {#debug-critical-metrics}

See [Understand your Test Results](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html?lang=en#how-to-use) to understand the results.

Some notes about the Performance Test step:

* The *Performance Step* is a web performance step, that is, the time to load the page using a Web browser.
* The URLs listed in the result *CSV* file are loaded in a Chrome browser in the Cloud Manager infrastructure during the test.
* A common metric that fails is the *error rate*. In order for a URL to pass, the main URL must load with `200` status and in less than `20` seconds. Page loads that exceed `20` seconds are marked as `504` errors.
* If your site requires User Authentication, see the document [Understand Your Test Results](understand-your-test-results.md#authenticated-performance-testing) for configuring the test to authenticate to your Site.

## Are we allowed to use SNAPSHOT in the version of the Maven project? How does versioning of the packages and bundle jar files work for Stage and Production deployments? {#snapshot-version}

Refer to the following scenarios to learn about versioning of the packages and bundle jar files for Stage and Production deployments:

1. For developer deployments, the Git branch `pom.xml` files must contain `-SNAPSHOT` at the end of the `<version>` value. This allows subsequent deployment where the version does not change to still get installed. In developer deployments, no automatic version is added or generated for the maven build.

1. In Stage and Production deployment, an automatic version is generated as documented [here](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/activating-maven-project.html?lang=en#managing-code).

1. For custom versioning in Stage and Production deployments, set a 3 part proper maven version like `1.0.0`. Increase the version each time you have to do another deploy to production.

1. Cloud Manager automatically adds its version to Stage and Production builds and even creates a Git branch. No special configuration is required. If step 3 above is skipped, the deployment would still work fine and a version would automatically be set.

1. There are no issues, if you leave the version with `-SNAPSHOT` for Stage and Production builds or deployments. Cloud Manager automatically sets a proper version number and creates a tag for you in Git. This tag can be referred to later, if required.

1. If you want to try out some experimental code on Development environment, you can create a new Git branch and set the pipeline to use that different branch. This is useful when deployments start failing and you would like to test with older versions of the code to see when it broke.

   The Git command below creates a remote branch named *testbranch1* against a specific pre-existing commit `485548e4fbafbc83b11c3cb12b035c9d26b6532b`.  This special branch could be used in Cloud Manager without affecting any other branches:

   `git push origin 485548e4fbafbc83b11c3cb12b035c9d26b6532b:refs/heads/testbranch1`

   See the [Git documentation](https://git-scm.com/book/en/v2/Git-Internals-Git-References) for more details.

   If you want to delete the test branch later, then use the delete command:

   `git push origin --delete testbranch1`

## Maven build fails in Cloud Manager deploys but builds locally without errors. How to debug? {#maven-build-fail}

See [Git Resource](https://github.com/cqsupport/cloud-manager/blob/main/cm-build-step-fails.md) for more details.

## Unable to set a variable via aio cloud manager set pipeline variables. How to debug these issues? {#set-variable} 

If you are getting a `403` error when attempting to list or set pipeline variables via commands similar to the ones below, then you need to be added as a *Deployment Manager* Cloud Manager product role in the Admin Console.  
See [API Permissions](https://www.adobe.io/apis/experiencecloud/cloud-manager/docs.html#!AdobeDocs/cloudmanager-api-docs/master/permissions.md) for more details.

Related commands and errors:

`$ aio cloudmanager:list-pipeline-variables 222`

*Error*: `Cannot get variables: https://cloudmanager.adobe.io/api/program/111/pipeline/222/variables (403 Forbidden)`

`$ aio cloudmanager:set-pipeline-variables 222 --variable TEST 1`

*Error*: `Cannot get variables: https://cloudmanager.adobe.io/api/program/111/pipeline/222/variables (403 Forbidden)`
