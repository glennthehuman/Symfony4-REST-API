* Enter database credentials in env file
* Run 'php bin/console doctrine:schema:create'
* Run 'php bin/console doctrine:migrations:migrate'

Suggested reading: https://www.bennadel.com/blog/2385-application-services-vs-infrastructure-services-vs-domain-services.htm

Excerpt:

**Application Services**

Sometimes referred to as "Workflow Services" or "User Cases", these services orchestrate the steps required to fulfill the commands imposed by the client. While these services shouldn't have "business logic" in them, they can certainly carry out a number of steps required to fulfill an application need. Typically, the Application Service layer will make calls to the Infrastructure Services, Domain Services, and Domain Entities in order to get the job done.

I believe that the Application Service objects are "command" objects. That is, in relation to the command/query separation, you wouldn't talk to an Application Service simply to retrieve information. Instead, you'd go directly to the Domain Services for such information.

But, I am not 100 percent sure on that; if your "query" request involved additional actions such as logging or billing, then I'd hazard a guess that it would be part of the Application Service. Furthermore, if you didn't want to return domain entities to the Controller, I supposed you'd have to go through the Application Service layer for data requests such that it could transform the data.

The Application Service can only be called by the Controller. Since the Application Service orchestrates application workflow, it would make no sense for them to be called by the Domain Service (or even by themselves) - a workflow has only one single starting point; if an Application Service could be called by other entities within the domain model, it would imply that a workflow has an indeterminate number of starting points.

**Infrastructure Services**

These are services that typically talk to external resources and are not part of the primary problem domain. The common examples that I see for this are emailing and logging. When trying to categorize an Infrastructure service, I think you can ask yourself the following questions:

If I remove this service, will it affect the execution of my domain model?
If I remove this service, will it affect the execution of my application?
If it will affect your domain model, then it's probably a "Domain Service." If, however, it will simply affect your application, then it is probably an Infrastructure Service. So for example, if I removed Email Notifications from an application, it probably wouldn't affect the core domain model of the application; it would, however, completely break the usability of the application - hence it is an Infrastructure service (and not a Domain Service).

In my diagram, the Infrastructure service can only be invoked by the Application Service. Given the above questions, this makes sense. If the Domain Services or the Domain Entities could invoke the Infrastructure Services (think Logging, think Email confirmations), then you'd probably have application logic leaking into your domain model. Plus, since logging and emailing is typically part of some greater "workflow," it makes sense that they'd be called from the "Workflow Services" (ie. Application Services).

NOTE: I have seen a number of people categorize Database access as an Infrastructure concern. In my diagram, I treat persistence encapsulation as a separate concept - Gateways. I am not sure how to reconcile this. If I moved Gateways into Infrastructure, then it would mean the Domain Services could invoke the Infrastructure services. I'm not quite sure how I feel about that just yet - it might open up a whole can of worms. Data persistence is probably the biggest monkey wrench in my understanding of all of this!

**Domain Services**

These are services that coordinate domain-level actions that are application-agnostic. From what I have read, they provide a means to ensure the integrity of the domain model by encapsulating CRUD (Create, Read, Update, Delete) operations and data access. Plus, they provide a place to put behaviors that don't really fit into any single Domain Entity.

Right now, this is a lot of theory and thinking and almost no code. I am sure that as I start to try and put some of this thought into practice, my understanding will continue to evolve. Heck, you can see how much my thinking has already evolved since my application architecture post yesterday. Wild stuff!
