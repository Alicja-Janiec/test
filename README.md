# test
# SOL documentation 
SOL (Safety Operating Limits) is an application that supports monitoring of operating limits during production activities on offshore platforms. The application allows to see sensors list, timeseries and excursions. It also allows you to create events based on excursions. It has reporing some reporting functionality too.
## Development environment
Requirements

- OpenJDK 11
- Docker

After cloning the project you need to update your .env file

```
DB_USERNAME=
DB_PASSWORD=
SOL_PWSALT=
PIWEBAPI_PASSWORD=
OIDC_CLIENT_SECRET=
COGNITE_CLIENT_SECRET=
```

To run applicaiton on docker you need 

### Source code

Running the application from source code

    ./gradlew bootRun

## Backup
Azure SQL Server has daily backup enabled together with weekly and monthly.

## Application Architecture

The application is a Grails / Spring Boot Java application using an embedded Tomcat web server. It runs behind an Apache proxy and is only accessible via the proxy front-end.

Four-tiered architecture

· The web layer is the public-facing layer. It is responsible for processing user input and returning the correct response back to the user. The web layer must also handle the exceptions thrown by the other layers. Currently there is no authentication for unauthorized users.

· The web layer is the public-facing layer. It is responsible for processing user input and returning the correct response back to the user. The web layer must also handle the exceptions thrown by the other layers. Currently there is no authentication for unauthorized users.

· The service layer resides below the web layer. It acts as a transaction boundary and contains both application and infrastructure services. The application services provide the API of the service layer for use by the web layer.

· The repository layer is the lowest layer of the application stack. It is responsible for communicating with the database(MS SQL Server) used by the application and presents one or more APIs to the service layer to enable this.

## Data flow overview

The application communicates with several resources:

· Block Events Database from ABB control system log

· Azure SQL Server

· Cognite CDF

· Power BI connects to the SOL database and to the frontend of the application.

![dataflow-picture](documentation/images/solapp.png)

## Deployment Architecture

### Hosting

The application is currently hosted on AkerBP’s Azure Virtual Machine running Red Hat 7. The application is only available inside AkerBP’s level 4 zone.

### Database

We are using multiple databases:

The main application database is a AkerBP Azure SQL Server instance. Hostname is:

dlcdb01.database.windows.net;databaseName=PO_PowerBI

The second database is an on premise AkerBP SQL Server database for block log data, the hostname for this is:

A21-D1-SQL005;databaseName=Valhall_BlockLog

### Application software

The application is built on Grails 4 (Spring Boot 2.1) running on OpenJDK 11.0

### Domain name

The application have the following domain name http://sol.eureka.akerbp.com

### Networking and communications

The application is communicating with the database servers we have mentioned earlier. There are firewall rules added by Sopra Steria for communication between the Azure virtual machine and Azure SQL Server and AkerBP SQL Server.

The Blog Log Database has only a read only connection.

![network-picture](documentation/images/network.png)

### Security

The application is secured/encrypted with HTTPS and uses Azure Active Directory to give AkerBP users access to the application. Users need to belong to the xxx group to have access to the application . It is also only accessable through AkerBP’s level 4 network zone. Third parties have to use citrix to access the application.

### Observations

As the application is still in development, the needs for deployment and test infrastructure is not in place. Neither is handling scaling nor failover.

# Notes

## RHEL / CentOS

### Libargon2  
The project use the Argon2 hashing library, this library may not be available in RHEL yet, you can checkout the https://github.com/p-h-c/phc-winner-argon2 repository and `m̀ake && make install`
