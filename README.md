# Sample To Do List web application using Spring Boot and DSE

A simple Todo list application using Spring Boot with the following options:

- Spring Data Cassandra for persistence.
- Thymeleaf templae for the rendering.
- REST API for simple add/list operations.

To build and run the sample from a fresh clone of this repo:

## Configure DSE

1. Create a keyspace and table in DSE via `cqlsh -f src/main/resources/schema.ddl`



## Build and run the sample

1. `mvn clean package`
2. `java -jar target/TodoDemo-0.0.1-SNAPSHOT.2ar`
3. Open a web browser to http://localhost:8222

As you add and update tasks in the app you can verify the changes in the database through cqlsh using simple statements like 
`select * from demo_sdc.todo`.






# Notes
This repo is a simple ToDo application with 2 controllers: one is pretty (Thymeleaf) and one is REST

This repo demonstrates a few things:
1. Spring Boot
2. Spring Data Cassandra
3. Pivotal Cloud Foundry (PCF) and connecting to a Custom User-Provided Service (CUPS)

## Configuration
The DSE configuration is contained in `src/main/resources/application-dev.properties`.  The other 
configs are contained in `src/main/resources/applicaiton.properties`.

The idea is that you would set up the properties from `application-dev.properties` in a user-provided 
service in PCF (`cf cups mycups ...`).  If you do this, comment out line 8 in DseTodoProperties.java, specifically
this: `@PropertySource("classpath:application-dev.properties")`.  To run with properties defined in 
`application-dev.properties`, uncomment that line 8.

To set the properties correctly, we need to set the following:
* `vcap.services.dsecups.credentials.hostname=<DSE contact point(s)>`
* `vcap.services.dsecups.credentials.port=<DSE port (9042)>`
* `vcap.services.dsecups.credentials.keyspace=<DSE keyspace (demo_sdc)>`
* `vcap.services.dsecups.credentials.usepwd=<Whether to use user/pass login (true)>`
* `vcap.services.dsecups.credentials.cass_username=<DSE username (cassandra)>`
* `vcap.services.dsecups.credentials.cass_password=<DSE password (datastax)>`
* `vcap.services.dsecups.credentials.usessl=<Whether to use SSL (true)>`
* `vcap.services.dsecups.credentials.truststore=<BASE64 encoded truststore>`
* `vcap.services.dsecups.credentials.trust_password=<truststore password>`
* `vcap.services.dsecups.credentials.keystore=<BASE64 encoded keystore - first 4000 bytes>`
* `vcap.services.dsecups.credentials.keystore2=<BASE64 encoded keystore continued - remainder of keystore>`
* `vcap.services.dsecups.credentials.key_password=<keystore password>`

To BASE64-encode the keystore/truststore files, you can use a tool like `https://www.browserling.com/tools/file-to-base64`

### Running without PCF
* Compile and build with `mvn clean package`.
* Run with `java -jar target/TodoDemo-0.0.1-SNAPSHOT.jar`
* Navigate to `http://localhost/` or `http://localhost/rest/hello`

### Running with PCF
1. Login to PCF: `cf login -a https://api.run.pivotal.io`
2. Setup CUPS: 
```cf cups dsecups -p '{\"hostname\":\"18.216.190.63\",\"port\":\"9042\",\"keyspace\":\"demo_sdc\",\"usepwd\":\"true\",\"cass_username\":\"cassandra\",\"cass_password\":\"datastax\",\"usessl\":\"true\",\"truststore\":\"/u3+7QAAAAIAAAABAAAAAgAJY2x1c3RlcmNhAAABZ3TIkLoABVguNTA5AAADlTCCA5EwggJ5oAMCAQICCQCKvTBLBEnaLTANBgkqhkiG9w0BAQsFADBeMQ8wDQYDVQQDDAY0QnJpYW4xETAPBgNVBAoMCERhdGFTdGF4MQ0wCwYDVQQLDARgRFNFMQswCQYDVQQGEwJVUzELMAkGA1UECAwCVFgxDzANBgNVBAcMBkF1c3RpbjAgFw0xODEyMDMxNTQ1MDVaGA8yMTE4MTEwOTE1NDUwNVowXjEPMA0GA1UEAwwGNEJyaWFuMREwDwYDVQQKDAhEYXRhU3RheDENMAsGA1UECwwEYERTRTELMAkGA1UEBhMCVVMxCzAJBgNVBAgMAlRYMQ8wDQYDVQQHDAZBdXN0aW4wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC7EckcY61QDHwB0FSOdpfT2efjfgm/2fpQi38je2QAw3/ce1v7wTGmAQKiPsR8v0IUiDgq9AWIpLDW5JkHTzlPg5u7AxpNaSCeN+QT4AhA718+7gk7p31SzHHmd2hxMXwjV9ZnjLgi7P8tX9I4L3AgjJrtVlOUYog21IkoMevLnz8EX6Ywxk3ZBog/xKY+wWNvXyV9t1SXqt2SVbk3fmAZ2OH8p6eMeMvtQFr+NNO6tB6PT/LpmPYNYwf5sD6hbqnxCJlJBwPeWmlljGONXsoUBp0EKV1PnvBzfUR2HUHzwmH+lSQO7QIVSkLYVVyv39RU625uhhWaXuo6riYZ/z1BAgMBAAGjUDBOMB0GA1UdDgQWBBRQ5XN0emDsrkpZg344tRf5vJg2pjAfBgNVHSMEGDAWgBRQ5XN0emDsrkpZg344tRf5vJg2pjAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBCwUAA4IBAQC01tFp+/5vaHTfXLjyBILrOZtdMepElih3iu/WKU44oEYwC+aefUEwwV8II18zACQmYk5abJCL11Xcscev1jTVWNJHuhfjzQuVWjUiU/i5bgHVvYaAh92vkA598HH40b9MqndGzVqsJ+Bqzm0LiXJ23I26K1QZTp3XKcOa58JEnNZtkIJADeEnFoCzpG95Ve3nai/DeUdAvD6iKfwcnm8AHr1dwZ+jq7Nb8R+WPkvYpMXdjiIu7AkVuX8JA5+VUdLqscbN+z3EZIkI9lrXGyzZKbzioqmXkpIIs7ys8JEwdUiY9bDL0ofJyye+Pbuav1ThUUfs2zxeWVDfg3wDWPkqFh7C++RpdCLbWQqEgX2yvu2NV0I=\",\"trust_password\":\"lifecyclemanager\",\"keystore\":\"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\",\"keystore2\":\"uAdW9hoCH3a+QDn3wcfjRv0yqd0bNWqwn4GrObQuJcnbcjborVBlOndcpw5rnwkSc1m2QgkAN4ScWgLOkb3lV7edqL8N5R0C8PqIp/ByebwAevV3Bn6Ors1vxH5Y+S9ikxd2OIi7sCRW5fwkDn5VR0uqxxs37PcRkiQj2WtcbLNkpvOKiqZeSkgizvKzwkTB1SJj1sMvSh8nLJ749u5q/VOFRR+zbPF5ZUN+DfANY+SoAAAACAAljbHVzdGVyY2EAAAFndMiR9gAFWC41MDkAAAOVMIIDkTCCAnmgAwIBAgIJAIq9MEsESdotMA0GCSqGSIb3DQEBCwUAMF4xDzANBgNVBAMMBjRCcmlhbjERMA8GA1UECgwIRGF0YVN0YXgxDTALBgNVBAsMBGBEU0UxCzAJBgNVBAYTAlVTMQswCQYDVQQIDAJUWDEPMA0GA1UEBwwGQXVzdGluMCAXDTE4MTIwMzE1NDUwNVoYDzIxMTgxMTA5MTU0NTA1WjBeMQ8wDQYDVQQDDAY0QnJpYW4xETAPBgNVBAoMCERhdGFTdGF4MQ0wCwYDVQQLDARgRFNFMQswCQYDVQQGEwJVUzELMAkGA1UECAwCVFgxDzANBgNVBAcMBkF1c3RpbjCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALsRyRxjrVAMfAHQVI52l9PZ5+N+Cb/Z+lCLfyN7ZADDf9x7W/vBMaYBAqI+xHy/QhSIOCr0BYiksNbkmQdPOU+Dm7sDGk1pIJ435BPgCEDvXz7uCTunfVLMceZ3aHExfCNX1meMuCLs/y1f0jgvcCCMmu1WU5RiiDbUiSgx68ufPwRfpjDGTdkGiD/Epj7BY29fJX23VJeq3ZJVuTd+YBnY4fynp4x4y+1AWv4007q0Ho9P8umY9g1jB/mwPqFuqfEImUkHA95aaWWMY41eyhQGnQQpXU+e8HN9RHYdQfPCYf6VJA7tAhVKQthVXK/f1FTrbm6GFZpe6jquJhn/PUECAwEAAaNQME4wHQYDVR0OBBYEFFDlc3R6YOyuSlmDfji1F/m8mDamMB8GA1UdIwQYMBaAFFDlc3R6YOyuSlmDfji1F/m8mDamMAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQELBQADggEBALTW0Wn7/m9odN9cuPIEgus5m10x6kSWKHeK79YpTjigRjAL5p59QTDBXwgjXzMAJCZiTlpskIvXVdyxx6/WNNVY0ke6F+PNC5VaNSJT+LluAdW9hoCH3a+QDn3wcfjRv0yqd0bNWqwn4GrObQuJcnbcjborVBlOndcpw5rnwkSc1m2QgkAN4ScWgLOkb3lV7edqL8N5R0C8PqIp/ByebwAevV3Bn6Ors1vxH5Y+S9ikxd2OIi7sCRW5fwkDn5VR0uqxxs37PcRkiQj2WtcbLNkpvOKiqZeSkgizvKzwkTB1SJj1sMvSh8nLJ749u5q/VOFRR+zbPF5ZUN+DfANY+Sob2eK8CopNH4aEWF/9sbcI05X8RA==\",\"key_password\":\"lifecyclemanager\"}'```
3. Push the app: `cf push dsetodo -p .\target\TodoDemo-0.0.1-SNAPSHOT.jar`
4. Bind the service: `cf bind-service dsetodo dsecups`
5. Restage the app: `cf restage dsetodo`
6. Navigate to webpage: `https://dsetodo.cfapps.io` or `https://dsetodo.cfapps.io/rest/hello`.

