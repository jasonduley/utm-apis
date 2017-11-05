# UTM APIs

This repository contains the collection of OpenAPI specification APIs within the NASA's research version of the UTM System.

Each directory contains files related to a single API.  Within each directory there is an additional README that provides further insight into that API.

## References

- [UTM Home Page](https://utm.arc.nasa.gov/)
- [UTM Publications](https://utm.arc.nasa.gov/documents.shtml)
- [UTM Swaggerhub](https://app.swaggerhub.com/organizations/utm)


## Codegen 

You can generate code from OpenAPI Specifications (swagger).  

Swagger Hub has a feature where the site will generate code into a zip file which is downloaded. 
This is a great first checkout, however, it uses the default language-specific configurations. 

One option (which we have used efficiently) is to generate all the data models into a library.
In Swagger code gen, code is be generated only from API Specifications, not directly from Swagger Domains.  
An approach to creating a model-only library is to generate against all the APIs whereby the generation 
output is filtered for model-only.
The argument to the input spec can be a local file or internet

````````
GENERATE="java  -Dmodels -DmodelDocs=false -DapiDocs=false -jar $CODEGEN generate  -l spring --config config.json"

$GENERATE -i ${SWREPO}/fims-uss-api/swagger.yaml   #localfile input
or
$GENERATE -i https://raw.githubusercontent.com/nasa/utm-apis/master/uss-api/swagger.yaml

````````

Your language-specific configurations can also generate model-specific data validations.
For example for swagger-codegen's 'Spring server' language you can codegen bean validations
and java8's OffsetDateTime class for date-time using this language-specific config. 

````````

{
  "library": "spring-boot",
  "java8": "true",
  "dateLibrary":"java8",
  "modelPackage": "gov.nasa.utm.utmcommons.model",
  "useBeanValidation":"true"
}

````````
## Sandbox vs. Swaggerhub versions

Swaggerhub's master branch is generally ahead of our sandbox.

The swaggerspec's github is branched by version.

For example the v17.11 sandbox was generated from github branch tcl3_v17.11 which has base 0afb8139643610b0743d9d436b1feb341b935e73.

If you checkout this branch locally you can point to tcl3_v17.11 for codegen or browsing.
The github top level README is now updated with notes for codegen'ing from a local
clone by release tag, as well as options for viewing local swagger specs.


### Local Viewing

You have choices to edit/view local swagger files. IMO option 1 is better for viewing all files.  A local swagger editor is good if you want rendering. 

1. Use a text editor that supports YAML such as Sublime Text 2.

2. Install local [Swagger Editor](https://swagger.io/swagger-editor/)


3. Bring up an online swagger editor and copy entire file contents into your browser


## Codegen by verison

To codegen from local swagger specs, the local files need to be "resolved"  whereby
all domain references are resolved.  (Because the refs point to master/HEAD. This will result in mixed versions. 

`````````
CODEGEN=./lib/swagger-codegen-cli-2.2.3.jar
GENERATE="java  -Dmodels -DmodelDocs=false -DapiDocs=false -jar $CODEGEN generate  -l spring --config config.json"

$GENERATE -i ./uss-api/swagger-resolved.yaml  #where input is a local file
```````