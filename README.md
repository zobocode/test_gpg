# Module Flask GCR Template

 ## Purpose

 This document aims to give information on the current module. This module is dedicated to be used as a template for building a Google Cloud Run application. This solution uses the Python Framework Flask.

 ## Content

 ## How to test this module template

 ### Requirements for local testing
 1. service account credentials

Put the credentials json  file for a service account in the module directory. It is mandatory to inject the permissions in the Docker image. The credentials file must be named 'creds.json'.

See [here](https://cloud.google.com/docs/authentication) the Google documentation about credentials and authentication.
  
 2. environment variables
  ```
  export SANDBOX_PROJECT=<your-sandbox-project-name>
  ```

 ### Run the test
 1. build the docker image

Go into the module directory and run this command:
```
BACK_PROJECT=btdp-sbx-g-wadriako make -f ../../Makefile.module gcr
```
The Docker image will be built and stored at in GCP Container Registry.

2. Run the application locally

From the module directory, run the following command: 
  ```
  BACK_PROJECT=$SANDBOX_PROJECT SANDBOX_DB_HOST=host.docker.internal make -f ../../Makefile.module local-test
  ```
*where ```host.docker.internal``` is your local Docker host's IP address (set up is only done for local testing purpose).*

At this step, the Flask application is running at http://0.0.0.0:8080/.

3. Test the application

To test the application, use a curl request GET, for example:

```
$ curl -X GET -H "Authorization: Bearer $(gcloud auth print-identity-token)" http://0.0.0.0:8080
```
where ```$(gcloud auth print-identity-token)``` is the token that allows the user to request the application.

If the curl request is successful, here an example of output:
```
{"caller_info":{"at_hash":"-hhFB5nEB6gZHaDJyJmUaw","aud":"32555940559.apps.googleusercontent.com","azp":"32555940559.apps.googleusercontent.com","email":"gilles.wadriako@loreal.com","email_verified":true,"exp":1615476538,"hd":"loreal.com","iat":1615472938,"iss":"https://accounts.google.com","sub":"115844571050672950277"},"headers":{"Accept":"*/*","Authorization":"Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6Ijg0NjJhNzFkYTRmNmQ2MTFmYzBmZWNmMGZjNGJhOWMzN2Q2NWU2Y2QiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiIzMjU1NTk0MDU1OS5hcHBzLmdvb2dsZXVzZXJjb250ZW50LmNvbSIsImF1ZCI6IjMyNTU1OTQwNTU5LmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwic3ViIjoiMTE1ODQ0NTcxMDUwNjcyOTUwMjc3IiwiaGQiOiJsb3JlYWwuY29tIiwiZW1haWwiOiJnaWxsZXMud2Fkcmlha29AbG9yZWFsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJhdF9oYXNoIjoiLWhoRkI1bkVCNmdaSGFESnlKbVVhdyIsImlhdCI6MTYxNTQ3MjkzOCwiZXhwIjoxNjE1NDc2NTM4fQ.dGvkpbOp9SeAOkneDNYGXL9vYFiTW9ys6oU84CGIOZQkr_vhm4JGIBFibKvJdqYpJYcWLejFjCGH10wFbNlqmgpF8yDmpmd8TwO_YOYLn9tVi2VMiR9DorK_iyJo6R_6ZTYxF3T_S-uC0lJSDLSO7-T7RpNVmcZHz1pNqmF9iXRr6_bYGJP5UE31FlP6cFNSOdJjEvxvjbcWoj2-d5JQVyx892yY5brqLCL0DXp5-eNiEERCaK9nRdZoNaXbLY5K-4vCucyHN9vGCDpMQjS4lPynDwvtwxrrYmd8pEARTtcPGjajQc6pPTWQkdVGvby2Tz4vbDNdvufZf5UEIhleUQ","Host":"0.0.0.0:8080","User-Agent":"curl/7.68.0"},"result":"Hello gilles.wadriako@loreal.com"}
```

### Components

The main components are:

 Component name     | Comment
--------------------|---------------------------------------
 application        | the application creation using IoC
 blueprint_factory  | the flask blueprint decorator definition
 controller         | the routing implementation
