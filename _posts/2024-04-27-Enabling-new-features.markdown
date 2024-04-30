---
layout: post
title:  "Smooth Deployments in mature orgs!"
date:   2024-04-27 01:25:15 +0530
categories: Salesforce
---
At some point in our lives we work in projects or orgainizations which have mature orgs. 

Mature Orgs?

Yes, orgs that have been in production for many years, have multiple features, packages deployed. Multiple teams have worked on these over multiple years, so long ago that people who worked on these features have moved organisations. Such orgs have large user base, agents, and customers. Most of business processes are already set in system, Case Management, Omni, Sales, Marketing, Service, Experience, Lighnting migration, deployments processes. 
Most of the development work is either a feature request, enhancements or production bugs from users or agents.
Occasionaly the business might decide to try some new Salesforce offering to improve agent productivity, digital initiateves, new integrations for better reporting and data warehousing, moving to different vendors etc.

So What?

Well the impact these orgs play in business running smooth in ineffable. It is literally the companies infracture running. While in such the code base is immense, virtually unlimited funtionalities and automations, zero to little documenatation, and the most worrysome no one knows how is all works in symphony, what the lecagy requirements were and why were they in first place, it is nightmare to having to modify anything in such orgs even as small as removing an if statement which you suspect could be the reason for an edge happeing in production instance for single user that you havent been able to replicate in other environment. Yes, we have all been there. The pressure is huge and consequences unfathomnable. How do you tackle this all?


Creating a gaurd rails

Once the deployment is done it might be hard to revert the changes if something breaks. This will involve involvement of Release Management, CICD Admins, Salesforce admins, coordination with other teams who had a release with you. It can be all nightmare not just for you but for the other teams, company and most important the customers and their business. This can be not just challenging and stressful but could potentially be risky as well.

Now you know it your code that has caused an issue in production, potentially slowing down org, hitting governer limits or things not working as expected. 
What can you do to avoid all this and fulling the system from your changes. That is where the system of gaurd rails come it. 

Gaurd Rails

This system will involve two deployments, don't panic it will make sense as you read along.
Deployment 1
This is release when you deploy or introduce your changes to Production environment alony with a gaurd Rail. This is the normal planned deployment that was expected on the release date

Deployment 2
This is next release when you are sure the feature or fix is working as expected in the Production after validations from users and monitoring the system for any anamolies. In this deployment you remove the gaurd rails deployed in the last release to keep the code clean.

Gaurd Rails are nothing but a way to enable or disable a feature in production on demand without any deployment. This is simple as a manual step which can be the backup plan in case of fallout. 
But do you achieve this?  Custom Labels my friends.

You write the code in such a way to check for the value in a custom label which maybe as simple as true to false. Based on this value the feature can be enabled or disabled.
Here are a few examples:

Example 1: Send email notification to customers when they haven't responded to agent after rep changes the case status. Suppose the company doesnt want to send these notifications and leave the case open now.


{% highlight ruby %}

public class CasesTriggerHandler extens TriggerHandler{

public handleAfterUpdate(List<Case> Trigger.New){
  sendCustomerNotification(Trigger.New);
  ...
}

private static void  sendCustomerNotification(List<Case> CaseList){
  if($Label.sendCustomerNotification == 'True'){
    return;
  }
  //create notifications and send email
  ...
  }
}



{% endhighlight %}

