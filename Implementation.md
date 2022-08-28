# High level implementation plan
## User Profiles
* CMS user
* VA user
* Admin

## Functional Requirements
* Create the API for consumption at VA to import beneficiary details into CMS
* Create the API for consumption at CMS to view the demographics and coverage information of beneficiaries to identify dual beneficiaries.

## Technical requirements/constraints from the prospective product owner
* Use FHIR Bulk data IGs
* The data formats provided should be maintained as much as possible.
* Should deploy into AWS US-east environment

## Key Technical Decision at the Beginning of the project
1. Use Hapi FHIR starter kit - Our organization has been using this for several of its projects and also in its product offerings. It provides a comprehensive FHIR environment and contains most of the latest enhancements based on FHIR implementation guides.
2. Bulk data Import for importing data - The implementation guide has not yet been balloted. However many vendors have taken the route of using the export format for importing the data. Hapi FHIR also implemented it the same way. However the data files provided as a part of the technical implementation would require significant changes. Our HCD team also recommended using standard FHIR data resources instead of the exported format based on the feedback they received. Hence we implemented the bulk data import in two formats. One using the NDJSON format which is used by Bulk data export and also in json format which takes in a collection bundle of resources. This enables VA users to use either bulk export or an other collection formats to extract data from their server and import into CMS system.
3. A few decisions were taken based on the data provided which are documented separately here.
4. We typically use Jenkins as our Devpops process management tool. However due to the time constraints, we decided to use simple AWS script which would install the cloud formations and the security groups. It will be in a "Infrastructure as Code" model. However there will not be any continous integration mechanism.
5. As in other short span projects, we decided not to use the pull request feature in github. Instead we add the developers as colloborators. 

## Key decisions during the execution of the project
1. FHIR profile changes were discouraged strongly. In the past, it was very difficult to make the custom profiles interoperables. So we implemented only minor changes to the standard implementation guides. It involved additional work on our side but based on past experience, it will open doors for standard applications to use this API easily. 
2. A new operation $member-match was introduced by a FHIR IG. We wanted to use that IG and explored the usage of that. It requires both the payers to agree on a standard format which is different from the data files given. Hence we decided bot to use it.
3. This APi may not be accessible by the patients or external users currently. However we constratined ourselves to use standards as much as possible to aid in future expansion.

## Implementation key points
1. The FHIR server will be running in a persistant mode with a relational database to back it. The table design etc is left to the HAPI FHIR JPA server which we have extended for our use.
2. An API gateway is placed as a layer in front of the FHIR server to enable load balancing, authentication, addition of optional filters. 
3. AWS cognito is being used as authentication and authorization service.
4. A mysql service from AWS RDS is being used for database management. 
5. As per the requirements there will be 4 endpoints. We have left the other FHIR endpoints also available for future expansion. Due to time constraints we did not block them temporarily. 
  * An endpoint to import Coverage and Patient data from VA asynchronously ($import operation)
  * An endpoint to check the status of the asynchronous data import ($import-poll-status operation)
  * An endpoint to synchronously get the Coverages based on patient (standard FHIR search on Patient Resource)
  * An endpoint to synchronously check the demographics of a patient (standard FHIR search on Coverage Resource)
6. Used agiled development model where a working server was created on day 1 of the implementation and gradual increments were made from then on so that continous feedback can be taken from both technical and non technical stakeholders.

Lessons learned
1. Several tasks were done in parallel which resuulted in longer feature deliveries. Could have reduced then number of parallel developments.
2. Team composition had dedicated personnel for cloud and infrastructure. For short spanned projects, we could have ustilized genaralists.
3. Could have used predefined teamplates for devops processes.
4. using GIT for documentation was little difficult for many personnel which resulted in schedule delays and too many last minute documentation changes.

What worked
1. Our experience with FHIR profiles and knowledge of various IGs.
2. Experience in setting up systems quickly.
3. availability of strong agile project management tools in house.
4. Human centric design based on our experience working with healthcare data.

