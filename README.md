# ola
ola microservice using Spring Boot

The detailed instructions to run *Red Hat Helloworld MSA* demo, can be found at the following repository: <https://github.com/redhat-helloworld-msa/helloworld-msa>


Build and Deploy ola locally
----------------------------

1. Open a command prompt and navigate to the root directory of this microservice.
2. Type this command to build and execute the microservice:

        mvn clean compile spring-boot:run

3. The application will be running at the following URL: <http://localhost:8080/api/ola>


### Deploy ola (Spring Boot) microservice

#### (Option 1) Deploy project via oc CLI

##### Basic project creation

----
$ git clone https://github.com/redhat-helloworld-msa/ola
$ cd ola/
$ oc new-build --binary --name=ola -l app=ola
$ mvn package; oc start-build ola --from-dir=. --follow
$ oc new-app ola -l app=ola,hystrix.enabled=true
$ oc expose service ola
----

##### Enable Jolokia and Readiness probe

----
$ oc set env dc/ola AB_ENABLED=jolokia; oc patch dc/ola -p '{"spec":{"template":{"spec":{"containers":[{"name":"ola","ports":[{"containerPort": 8778,"name":"jolokia"}]}]}}}}'
$ oc set probe dc/ola --readiness --get-url=http://:8080/api/health
----

#### (Option 2) Deploy project via Fabric8 Maven Plugin

----
$ mvn package fabric8:deploy
----
