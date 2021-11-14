# Data Access playground

## Summary

The goal of this repository is to provide an isolated sandbox for the testing and development of new features for the iPC's data access framework. The services are dockerized and their deployment is orchestrated by docker-compose. Besides, these services are bootstrapped with test data, with no additional configuration required, and therefore, easing the burden of setting up such a complex environment. In this way, dev-teams only have to care about the development process itself. Importantly, the addition of the different services as git submodules allows them to have an independent development process, so that their inclusion, updates, and deployment [into the main project's repository](https://github.com/inab/iPC-Platform-Deployment.git), is greatly improved.

## Services, volumes and networks:

### <ins>Services</ins>

#### Data Access Committee portal (DAC-portal):

This portal allows the creation of new Data Access Committees and data policies (DAC-admin), the validation/rejection of incoming Data Access Requests by authorized users (DAC-members), and the inspection of the status of the different requests (users).

For technical details you can go to the [DAC-portal repository](https://github.com/acavalls/DAC-Portal.git)

#### Permissions-API:

This service enables the creation/inspection/deletion of user permissions at the level of files/datasets by DAC members. The API follows the GA4GH specification, which enhances interoperability.

For technical details you can go to the [Permissions-API repository](https://github.com/inab/Permissions-API)

#### Helpdesk portal:

Here, system administrators will be able to assign roles to the different users (i.e: DAC-admin), and validate the creation of new DACs. Importantly, this service should take care of the assignment of different files/datasets to the specific DACs.

This service has not been implemented yet.

#### Keycloak:

Keycloak controls authentication and authorization through all the platform components. The configuration steps related with clients (Permissions-API, DAC-portal), roles definitions (DAC-admin, DAC-member, User), and test users, have been setup in advance, and therefore, they are automatically applied during the service's startup. 

#### MongoDB:

Database that manages metadata both from the Permissions-API and the DAC-Portal. Testing data is provided, and it is automatically injected during the container startup, so that users can start playing around with the DAC-Portal interfaces and the rest of the system quickly.

#### Postgres:

The Keycloak service DB.

### <ins>Volumes</ins>

Both the Permissions-API and DAC-Portal source code is mounted in their respective container, which facilitates the development process. Moreover, MongoDB and Keycloak also expose their data to the host.

### <ins>Networks</ins>

The docker-compose.yml creates a private subnetwork (172.21.0.0/24) that assigns static IPs for the different services (ipam). This design has proven to be particularly useful when dealing with Keycloak redirections (OAuth2) in a system that combines public (browser) and confidential clients (APIs).


## How to deploy?

- Clone the [project's repository](https://github.com/acavalls/data-access-playground.git):

    ```
    git clone https://github.com/acavalls/data-access-playground.git
    ```

- Initialise git submodules (Permissions-API, DAC-Portal) 

    Execute the following commands in the root folder:

    ```
    git submodule init
    git submodule update
    ```

    As a result, the different git repositories will be cloned as dependencies of the main project.


- Launch the stack (main project root folder):

    ```
    docker-compose up
    ```

- Access to the DAC-Portal service and login:

    - Go to your browser and access to http://localhost:3000

    - Once redirected to the Keycloak's login page, introduce your credentials (check the 'user.txt' file in the root project's folder)


That's it! Easy, right?


 

