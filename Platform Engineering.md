### Platform Engineering

- the platform engineering changes lota established rules we knew about Devops, SRE, and cloud engineering.

# what is platform engineering ? 

- the problems that lead to platform engineering.
    - initially the developer and the ops works in separate teams
    - but this was inflexible and slow process as the developers has to wait for the ops when they need any changes in the IS or the resources or the pipelines.
    - or the ops ve to wait for the developers to fix something in their app, or app runtime etc 
    - when devOps was introduced it fix the inflexible and slow process by uniting both teams.
    - so this was the huge improvement of the traditional approach.
    - but as the developers team grows the devop has tremendous cognitive work loads (monitorin, maintenance, CI/CD, runtime, Security, IS etc).
    - too many things/ responsibility for the small devops team
    with the complex s/ms like k8s sometimes the pod of the managing IS becomes more effort than writing the app code and providing business logic itself.
    - so we ve teams that are too busy to build platform and less time to build the app.
    - plus the devop has to be expertise in so many things (means many engs have to be need leads to higher human resource costs).
    - and finally this becomes very hard to scale, they ve to set up the IS before they releas the final product.
    - or lack of missing knowledge/ expertise.
    - and when each team uses diff tech configuration, this leads to inconsistency across the organization.
    - and the SRE concept of you build it and you run it doesn't work.

## thats where the platform engineering comes into play.

    - but only when it implemented correctly.
    - the platform engineers take the tools that are needed for the deploying and running the application, and then standardize the usage across the teams.
    - so basically the platform engineers "standardize" the everything that is part of the application's "non functional requirement".
    - the non-functional requirements are the things that are not business logic but are necessary to deliver the application to the end users 
    - basicall the app needs VCS, ci/cd, runtime, provisioning, observe, secure, connect, logging etc. are all the non-functional requirements
    - in order to standardize these tools platform engineers need to takes over the admin/operations role of these tools 
    - which means selecting the best tools and selecting the best security practices.
    - so we basically extract the need of expertise to administer these tools from the application teams.
    - so now they ve more expertise and more time to focus on the operations.
    - so now they ve consistent operations.

# how to standardize w/o creating silos ?

- "self Service Platform" - the platform team all those tools that the app team needed and applies their expertise and configures them properly.
- then they create the abstraction layer on the top, with a user friendly interface. UI
- so now the app teams come and choose/ self service the tools that are needed for them.
- so these provisioned and the interaction ui to use for the application is the platform
- and this a PssS for the internal developers or also called Internal Developer Platform "(IDP)".
- so the platform engineers are building the IDP, hiding away or Abstracting the complexity of the services that are needed for the developers.

# how it works in real life ? 

- freedom of choice 
- even if any of the tool is not available in the abstract platform, but they will integrate the tool in the platform (with the best practise) that is required for the application team.
- and offered to the other teams as well.
- so it keeps the platform flexibility, But add the guardrails and the pre-configured (to ensuer security, consistency and so on)
- so its a step forward to the Devops.
- Avoiding strict rules, how can they integrate those guardrailes for using tools correctly into IDP and part of the offering
- and that where the IaC or the configuration as a code Templates comes in
- again they will be having various pipeline templates ex if the app wants to use python for their app they can use the py pipeline specifically and so on.
- 
# Implementation

- this leads to the question of how to impl this sucessfully in the company.
- how to approach implementingg the IDP.
- The masterplan(entire sketch) for the IDP won't work and they can impl that 
- instead we can follow the "Agile" approach - starts with the small steps and that can immediately adds val to the one team.
- for starter the migration of the teams to the more modern tech stack will be so difficult for the app team, bcoz they will ve to put more effort and work to migrate.
- so the platform team has to consider the product team's status quo and helps them to slowly migrate/ move to the platform.

1. so the first thing is they ve to treat the IDP as a Product,
    - means they should not treat that as a one time project 
    - instead its a PaaS that needs to be developed over the time, and then continuously improved over time.
    - so it should be considered as a product with own development and release life cycles.
    - so they ve to work closely with developement teams to impl the IDP in a phase manner.. v1, v2, v3.. just like any other product.

# Platform vs DevOps

- now the platform team takes over the operations.
- now the application team don't need to set up and operate the cluster, but they still need to know how to deploy their applications into the cluster.
- this means we still ve to ve the devops in the product teams but now they will share their cognitive work loads with the platform team. and don't ve to ve deep expertise
- now as we know the platform is an application. as it needed lotta phsse manner etc,
- so these processess reqqurired the devops.. "Devops for the Platform."
- so we ve tons of devops process needed in the platform development.

# Platform vs Cloud Engineer 

- the platform engineering the enhancement of the Devops, SRE, and the cloud.
- the cloude eng needs to be specialized in any one the cloud service.
- but platform engineering has the wider range of knwledge outside the cloud alone.
- so basically they take the IasS to customize the platform as a sevice for the internal team.
- so essentially they build the layer on top of the cloud with the bunch of clouc services and other services/tools.


# TERRAFORM TF

- terraform is a tool for building, changing and versioning the IS safely and efficiently.
- it falls into the category of IasS, which allow us to define the entire IS.




