# AAS Connect Backend Service

This service provides a publicly available REST and GraphQL APIs for managing Asset Administration Shells (AAS) and AAS Submodel Templates and AAS Submodel Instances, stored in a Neo4j Graph Database. 

The provided REST endpoints fully comply with the [AAS OpenAPI Specification](https://github.com/admin-shell-io/aas-specs-api) published by the IDTA.


## Prerequisites

1. An installed and running recent version of the [Docker Engine](https://docs.docker.com/get-docker/).
2. *Optional:* A valid, personal GraphQL Frontend **License Key**, if you wish to use the **GraphQL Frontend** for accessing the GraphQL interface. <br/>Please refer to [https://graphqlfrontend.com/](https://graphqlfrontend.com/) on how to obtain one.

## Getting Started

1. Clone this repository to your local machine.
2. *Optional:* Open the file `.env.sample` with a text editor and replace the placeholder `{YOUR_FRONTEND_LICENSE_KEY}` with your personal GraphQL Frontend **License Key** and save the file as `.env`.
3. Run the following command in the repository root directory to download all dependencies and start the AAS Connect Backend Service 

 ```bash
 docker compose up -d
 ```

When the start-up completed, you will be able to access the following services in the browser:

- http://localhost:4000/graphql: The **GraphQL endpoint** for the AAS Connect Backend Service.
- http://localhost:4000/docs: The SwaggerUI listing and allowing access to all available **REST endpoints** for the AAS Connect Backend Service.
- http://localhost:4000/gui - The integrated **[BaSyx Web UI](https://github.com/eclipse-basyx/basyx-applications/tree/main/aas-gui)** for managing AAS and AAS Submodel Data.
- http://localhost:4000/: The integrated **GraphQL Frontend** instance. Use the credentials *aas/connect* to sign in (as defined in the `DEFAULT_USERNAME` and `DEFAULT_PASSWORD` variables in the `docker-compose.yml`). <br/>The frontend will only be functional if a valid GraphQL Frontend **License Key** is provided in the `.env` file.

To stop the services, run the following command in the repository root directory:

```bash
docker compose down
```

All persistent data is stored outside the container in the `neo4j/data` and the `neo4j/logs` directory. 

## First Steps

For submitting your first **Asset Administration Shell (AAS)** you follow this procedure (which might be automatized by your client application):

1. Create **Submodel Templates** for each submodel your AAS uses, using the [POST /api/v1/submodel](http://localhost:4000/docs/#/Submodel%20Repository%20API/PostSubmodel) endpoint.<br/>The `Submodel` object provided in the body must have the `kind` property set to `Template` and the `id` should be the unique global identifier of the submodel template(!) used for creating this submodel and must **differ** from the `id` used in the submodel in the AAS.
<br/>Templates need only to be created once and can be reused for multiple Submodel instances.
2.  Create **Submodel Instances** for each submodel your AAS uses, using the [POST /api/v1/submodel](http://localhost:4000/docs/#/Submodel%20Repository%20API/PostSubmodel) endpoint. <br/>The `Submodel` object provided in the body must have the `kind` property set to `Instance`. The `id` should be the same as the one used in the submodel in the AAS.
3. Create the **Asset Administration Shell** using the [POST /api/v1/shells](http://localhost:4000/docs/#/Asset%20Administration%20Shell%20Repository%20API/PostAssetAdministrationShell) endpoint. <br/>The id's of the submodels referenced in the `submodels` object property of the `AssetAdministrationShell` object in the request body must be the ones of the Submodel Instances created in step 2.
   understanding how all components work together.

## Copyright

Copyright (c) 2024 [FoP Consult GmbH](https://fop-consult.de/). All rights reserved.