![gbs-bulpros-company.png](https://github.com/rexhinvorpsi/gbs/blob/master/images/gbs-bulpros-company.png)
# Quick Reference
- Maintained by: [GBS Workflow Manager Team](https://www.gbs.com/en/workflowmanagement)
- Where to get help: [our Support](https://www.gbs.com/en/contactus)

# What is GBS Workflow Manager
Managing business processes efficiently is one of the biggest challenges facing companies today. Greater individualisation, more diversified departments, varying structures and varying systems contribute to the ever-growing complexity of business management.The GBS Workflow Manager is an intuitive solution for workflow management, making it easy to shape and automate business processes. A sure way of tackling even the biggest challenges successfully.
Find out more [https://www.gbs.com/en/workflowmanagement](https://www.gbs.com/en/workflowmanagement)  

# Running the image 
Workflow Manager runs on a jboss wildfly application server.  
To run the latest released image: 
```sh
 docker pull gbseuropagmbh/workflowmanager:latest

 docker run -d --name workflowmanger -p 8080:8080 -p 9990:9990 gbseuropagmbh/workflowmanager:latest
```
Access the UI at http://localhost:8080. Access the management console of jboss at http://localhost:9990/

### Run the full application
Workflow Manager can be used with one of these databases (Microsoft SQL Server, Postgres SQl, Apache Derby)
The recommended way to run the workflow manager is via **docker-compose**. 
Follow the link to see a few examples: [docker-compose examples](https://github.com/rexhinvorpsi/gbs/tree/master/examples)

**Using Docker Compose**
This configuration uses postgres as its main database

```
version: '3.4'

services:
  mongodb:
    image: mongo
    container_name: wfm-mongodb
    ports:
      - "27017:27017"
    networks:
      - postgres

  postgres:
    container_name: wfm-postgres
    image: postgres
    environment:
      POSTGRES_USER: root
      POSTGRES_PASSWORD: pavone
      POSTGRES_DB: postgres
      PGDATA: /data/postgres
    volumes:
      - postgres:/data/postgres
      - ./postgres-dbs/init.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - "5432:5432"
    networks:
      - postgres
    restart: unless-stopped

  workflowmanager:
    image: ${DOCKER_REGISTRY:-}/workflowmanager:${TAG:-latest}
    container_name: wfm-wildfly
    environment:
      - Datasource_MongoDB_Connectionstring=mongodb://wfm-mongodb:27017
      - Datasource_ProcessEngine_Connectionstring=jdbc:postgresql://wfm-postgres:5432/ProcessEngine
      - Datasource_ProcessEngine_Username=root
      - Datasource_ProcessEngine_Password=pavone
      - Datasource_OD_Connectionstring=jdbc:postgresql://wfm-postgres:5432/OD
      - Datasource_OD_Username=root
      - Datasource_OD_Password=pavone
      - Datasource_AgentManager_Connectionstring=jdbc:postgresql://wfm-postgres:5432/agman
      - Datasource_AgentManager_Username=root
      - Datasource_AgentManager_Password=pavone
      - Datasource_Driver_Class=org.postgresql.Driver
      - Datasource_Driver_Module=org.postgres
      - Datasource_XA_Datasource_Class=org.postgresql.xa.PGXADataSource
      - System_Database_Dialect=org.hibernate.dialect.PostgreSQL82Dialect
      - System_Security_Transport_Guarantee=NONE
      - System_Resteasy_PreferJacksonOverJsonB=true
    ports: 
      - 8080:8080
      - 9990:9990
    command: bash -c "/opt/jboss/wildfly/bin/add-user.sh admin pavone --silent && /opt/jboss/wildfly/bin/standalone.sh -b 0.0.0.0 -bmanagement 0.0.0.0"
    networks:
      - postgres
    depends_on:
      - postgres
      - mongodb

networks:
  postgres:
    driver: bridge

volumes:
  postgres:
```
`
 docker-compose up -d 
`

Navigate to http://localhost:8080
You should see the inital login page.
![workflowmanager.png](https://github.com/rexhinvorpsi/gbs/blob/master/images/workflowmanager.PNG)