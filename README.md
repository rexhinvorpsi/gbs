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
