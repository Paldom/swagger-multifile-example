# swagger-multifile-example

[![Build Status](https://travis-ci.org/Paldom/swagger-multifile-example.svg?branch=master)](https://travis-ci.org/Paldom/swagger-multifile-example)

Example multi-file OpenAPI 3.0 (OAS 3.0) specs for mobile and web applications. Specifications are spitted into smaller YAML files for better reusability, maintainability and separation.

This API specification is describing a sample blog service.

## Project structure

- `/components` - Basic OAS 3.0 components (schemas, parameters, securitySchemes, requestBodies, responses, headers, examples, links and callbacks), merged in `index.yaml` file.
- `/components/schemas` - Schemas (previously Swagger definitions) separated into different files.
- `/defaults` - Other defaults for all paths: `info.yaml`, `security.yaml` `servers.yaml` and `tags.yaml`.
- `/paths` - Paths for each service. Each definition can be used independently.
- `swagger.yaml` - All paths merged into one.

[More about OpenAPI Specification](https://swagger.io/specification/)

## Development

Install `swagger-ui-watcher` to render changes in your swagger files and observe merged specification in Swagger UI.

- Use for entire API:`swagger-ui-watcher ./swagger.yaml`
- Or for a specific service: `swagger-ui-watcher ./paths/<service>.yaml`

**Debugging:** use [swagger-editor](https://github.com/swagger-api/swagger-editor) and paste bundled yaml file (generated with `swagger-cli bundle ...` or `swagger-ui-watcher --bundle=...`) for more detailed error messages.

## Build and validate

Use `swagger-cli` (`npm install -g @apidevtools/swagger-cli`) to validate and distribute.

- bundle: `swagger-cli bundle ./swagger.yaml -t yaml -o swagger.bundle.yaml`
- validate: `swagger-cli validate ./swagger.yaml`

## Checklist before production

Consider following topics before taking this project as base of your production API:

- [ ] This project contains dummy values for demo purposes (e.g. callbacks, links), which shall be removed.
- [ ] Application architecture. This project is an example for a **monolith** API, but it can be easily separated into **microservices**. All `<path>.yaml` can be considered as a service, but health info, versioning and logging may need to be refined according to microservices architecture.
- [ ] Versioning in this project is managed in base path (e.g. `/api/v1`), but there are alternatives, like header (e.g. `Accept: application/vnd.example.v1+json`) custom header (e.g. `Api-Version: 1` or `Accept-Version: 1`) or query parameter (e.g. `?v=1`)
- [ ] Authentication endpoints are also dummy, refine with your flow or with your SSO service.
- [ ] Refine other security, e.g. based on [OWASP REST recommendations](https://www.owasp.org/index.php/REST_Security_Cheat_Sheet)
- [ ] Error handling is based on standard HTTP error codes and furthermore on custom error codes defined as examples in status 400 response. Find more in `parameters.yaml`.
- [ ] Refine naming conventions (URIs, custom headers, parameters).
- [ ] Check response formatting like ISO 8601 for date-time, type and structure of unique ids etc.
- [ ] Collection handling, pagination, filtering. Using named URIs for frequent queries.
- [ ] Details of localization handling (currently `Accept-Language` header is set to every endpoint).
- [ ] Request/response logging (e.g. headers for session, correlation).
- [ ] Caching with **expiration model** (`Cache-control` and/or `Expires` headers) or **time based validation model** (`Last-Modified` header on server and `If-Modified-Since` header on client side) or **content based validation model** (`ETag` header from server and `If-None-Match` in client request).
- [ ] Media management, proxy service etc.
- [ ] Of course add the endpoints you need, and don't forget to remove.

Feedback is welcomed, if you use this base project.

