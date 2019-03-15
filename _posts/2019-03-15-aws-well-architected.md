---
layout: post
title: Well Architected and Me
date: 2019-03-15 12:00:00
---

The [Well Architected Framework][0] is an guide to good infrastructure practices within AWS.
It's also about 80 pages long and incredibly dry.
So here it is in like 3 pages.

# Overview

The framework has been designed and developed to assist in the development of cloud solutions.
It's the official AWS guide to not being inefficient.
The white-papers are available for free for anyone who want's to study them.
They are a large part of the AWS Solutions Architect Certification

__Warning:__ The following article is my take on the more important parts of the Well Architected framework for the 90%. If you don't fit into the 90% understand, you're the weird one.

## The 5 Pillars

The well architected framework is based on 5 pillars -- operational excellence, security
reliability, performance efficiency, cost optimization -- each building off of the previous.

__Operation excellence:__ running systems that deliver value quickly and consistently
__Security:__ protecting information, systems, and customers
__Reliability:__ preventing and quick recovery from application or infrastructure failures
__Performance efficiency:__ using resources efficiently
__Cost optimization:__ minimizing cost without sacrificing performance

## Operation Excellence

> The Operational Excellence pillar includes the ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures.

__Perform Operations as Code.__
If you're deploying it, do it through code.
Use the [AWS CLI][1] and [Jenkins][2] to automate infrastructure creation.

__Make frequent, small, reversible changes:__
Non-reviersible (one-way) changes are terrifying.
If something goes wrong, there's no going back.
Develop a workflow that allows for changes to be reverted.

__Anticipate Failure:__
Everything fails all the time.
Expect things to break and infrastructure to go down.
Use things like autoscaling to recover from failures.

__Learn from all operational failures:__
If you make a mistake once, that's fine, just don't do it again.
Write up the post-mortem and make the changes to never let it happen again.

__Summary:__
Use the [AWS CLI][1] and [CloudFormation][3] to deploy infrastructure.
Use it with [Jenkins][2] or another build server solution for more consistency.

## Security

> The Security pillar includes the ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies.

__Implement a strong identity foundation:__
Use the principle of least privilege to prevent boundaries from being over-stepped.
Enforce segregation of duties with approriate authorization for resource interations.
Don't use long term credentials unless you need to.
[IAM][6] will be your go to for this.

__Enable traceability:__
When your servers go down because someone deleted all of your CloudFormation stacks, you're going to want to know who did it.
[CloudTrail][7] will allow you to process any and all logs from your systems.

__Protect data in transit and at rest:__
Classify data into levels of sensitivity and implement proper access controls at all levels.
[KMS][4] is your friend.

__Prepare for security events:__
Make sure every possible drop of data is monitored and logged.
If someone is able to get into your system, you're going to need to know who, what, when, where, why, and how.
[CloudWatch][5] can help you there.

## Reliability

> The Reliability pillar includes the ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.

__Test recovery procedures:__
Knock out a region and see what happens.
A properly designed system should not come down if the

__Automatically recover from failures:__
Autoscaling.
I cannot say this enough.
All of your resources on AWS (or other clouds) should utilize automatic scaling for increases in loads.
Get yourself some [load balancers][8].

__Scale horizontally:__
By running applications on multiple, small instances as opposed to one large instance, reduces impact from failures.
Losing 10% of your resources is much less devestating than losing 100%.
Distribute requests according to available memory and CPU capacity.
[ECS] is a great way to build toward horizonal scaling.

__Stop guessing capacity:__
Guessing capacity is a great way to immediately give yourself a chance to make a mistake.
Rather than guessing for production, run performance and load tests to determine scaling policies and appropriate instance types for your application.

## Performance Efficiency

__Democratize advanced technologies:__
If you don't have an expert in it, don't do it yourself.
AWS provides just about everything you'll ever need as a service.
Gone are the days when tech teams need to learn how to roll their own solutions all the time.

__Utilize multi-region deployments:__
Deploying to multiple regions requires almost no effort at all.
Take advantage of that fact in order to provide minimal latency for a wide-spread customer base.

__Use serverless patterns:__
Static sites can be hosted on S3.
Lambda can execute quick compute tasks without the need to manage an underlying server.
This also plays into minimizing costs, but that's not important until the next pillar.
[Serverless][10] can take your computation practices and quickly get them out the door.

__Experiment Often:__
With infrastructure being so easy to spin up and down, try new things.
No need to keep running the same tech stack due the the fear of, "What if it doesn't workout?"
If a new solution turns out to not be the right approach, nuke it and be done with it.

__Mechanical Sympathy:__
Utilize the best technology based on your needs.
If you have a flat storage need, use S3.
If you need to run a batch process, use AWS Batch.
If you have a fluid data model, use DynamoDB for easy schema updates.

__Analyze Tradeoffs:__
Managing ECS Clusters can lead to maximizing speed and performance, but [Fargate][11] is much less work for the develpoers to work with.
RDS can allow you to scale certain schema IO but DynamoDB is much more resilient with lower cost for changing schema.

## Cost Optimization

__Adopt a consumption model:__
Only pay for what you need.
You might need 15 instances in an ECS cluster to support maximum work load.
If that workload only exists for 2 hours per day though, then scale up when you need it and down when you don't.

__Use managed and application level services to reduce cost of ownership:__
No need to roll your own email service, just use [Simple Email Service][12].
Instead of deploying and hosting database servers, use [RDS][13] or [DynamoDB][14].
If there is an offering from AWS already, chances are, it'll be cheaper to use theirs in the long run.

__Maximize Resource Density:__
ECS Clusters are almost always grossly underutilized.
Running 10 Services all on their own clusters usually means you are doing something inefficiently.
Deploying a mem heavy and CPU heavy service on the same cluster can result in equal performance for decreased cost.

---
[0]: https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf
[1]: https://aws.amazon.com/cli/
[2]: https://jenkins.io/
[3]: https://aws.amazon.com/cloudformation/
[4]: https://aws.amazon.com/kms/
[5]: https://aws.amazon.com/cloudwatch/
[6]: https://aws.amazon.com/iam/
[7]: https://aws.amazon.com/cloudtrail/
[8]: https://aws.amazon.com/elasticloadbalancing/
[9]: https://aws.amazon.com/ecs/
[10]: https://d1.awsstatic.com/whitepapers/architecture/AWS-Serverless-Applications-Lens.pdf
[11]: https://aws.amazon.com/fargate/
[12]: https://aws.amazon.com/ses/
[13]: https://aws.amazon.com/rds/
[14]: https://aws.amazon.com/dynamodb/
