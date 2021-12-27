# gcp-infra-and-microservices

### Containerization

- "While Container Registry is still available and will continue to be supported as a Google Enterprise API, going forward new features will only be available in Artifact Registry, and Container Registry will only receive critical security fixes." - https://cloud.google.com/blog/products/application-development/understanding-artifact-registry-vs-container-registry
  - https://cloud.google.com/artifact-registry/docs/transition/transition-from-gcr#compare
  - https://cloud.google.com/artifact-registry/docs/transition/changes-gcp#artifact-registry_2

- If container images are to be nested in a repo you have to pay particular attention to the container source code's `Dockerfile` and `cloudbuild.yaml`, you can't just copy paste code that goes into these two files from docs because they assume you have a flat project/repo structure and the container image source code is not nested , look at the 2 files above for more guidance...and Merry Xmas!🎄🎅🏽🎁

### API Gateway

- "Using API Gateway, app developers consume your REST APIs to implement apps. Because all APIs are hosted on API Gateway, app developers see a consistent interface across all backend services. By deploying your APIs on API Gateway, you can update the backend service, or even move the service from one architecture to another, without having to change the API. As long as the API to your service stays consistent, app developers will not have to modify deployed apps because of underlying changes on your backend."

  - https://cloud.google.com/api-gateway/docs/about-api-gateway#api-gateway
  - "OpenAPI Specification (formerly Swagger Specification) is an API description format for REST APIs. An OpenAPI file allows you to describe your entire API" - https://swagger.io/docs/specification/about/

- "In addition, the gateway service account requires the permissions necessary to access your backend service. For example, if your backend is implemented as a Cloud Function, then the service account should at least be assigned the role of Cloud Functions Invoker. For a Cloud Run backend, the role is Cloud Run Invoker. By limiting the permissions associated with the API config, you can better secure your backend systems. After you create the service account, use the --backend-auth-service-account option to specify the email address of that service account when creating an API config:" (https://cloud.google.com/api-gateway/docs/configure-dev-env?&_ga=2.177696806.-2072560867.1640626239#configuring_a_service_account)

- `resource "google_api_gateway_api_config" "name"` is immutable, if you change the openapi spec file or any arguments in this resource and run `terraform apply` you will get the "resource already exists" error - destroy the resource (`terraform destroy -target RESOURCE_TYPE.NAME`) and run `terraform apply` again

### `gcloud`

- gcloud run services list
  - gives _region, endpoint, associated service account_

### Creating new projects

1. Create new `gcloud` configuration - `gcloud init` then select option 2 (https://cloud.google.com/sdk/docs/initializing)
2. Link github repo in cloud build
3. Manually enable resource manager API

### Things to do...

- `basic-express-microservice` should have it's own SA
- every time tf code runs, the container image is set to the gcp helloWorld one, instead of the basic-express image cloud build is deploying - there has to be a better way manage microservice infra AND be able to upload the latest image using cloud build
