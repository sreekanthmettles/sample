High level implementation plan
User Profiles
* CMS user
* VA user
* Admin

Functional Requirements
Create the API for consumption at VA to import beneficiary details into CMS
Create the API for consumption at CMS to view the demographics and coverage information of beneficiaries to identify dual beneficiaries.

Technical requirements/constraints from the prospective product owner
Use FHIR Bulk data IGs
The data formats provided should be maintained as much as possible.
Should deploy into AWS US-east environment

Key Technical Decision at the Beginning of the project
1. Use Hapi FHIR starter kit - Our organization has been using this for several of its projects and also in its product offerings. It provides a comprehensive FHIR environment and contains most of the latest enhancements based on FHIR implementation guides.
2. Bulk data Import for importing data - The implementation guide has not yet been balloted. However many vendors have taken the route of using the export format for importing the data. Hapi FHIR also implemented it the same way. However the data files provided as a part of the technical implementation would require significant changes. Our HCD team also recommended using standard FHIR data resources instead of the exported format based on the feedback they received. Hence we implemented the bulk data import in two formats. One using the NDJSON format which is used by Bulk data export and also in json format which takes in a collection bundle of resources. This enables VA users to use either bulk export or an other collection formats to extract data from their server and import into CMS system.
3. A few decisions were taken based on the data provided which are documented separately here.

