# Installing as an OpenShift application

The ID that you use must be able to make changes to the host environment. The tools required by all installation scenarios include the following items:

-   Docker. The commands used during installation retrieve files and container images from remote locations. If you are unable to access the internet during installation, the install images will need to have been previously downloaded and placed in a Docker repository that the installation commands can access.
-   IBM UrbanCode™ Deploy Version 6.2.3 and later. Although not strictly required, many UrbanCode™ Velocity features assume integration with UrbanCode Deploy. It doesn't matter which product you install first.

    If you are using an UrbanCode Deploy version prior to V6.2.5, you must install the patch located at the following website: [http://public.dhe.ibm.com/software/products/UrbanCode/plugins/ucsync/patches/ibmucd/](http://public.dhe.ibm.com/software/products/UrbanCode/plugins/ucsync/patches/ibmucd/). Select from the index the appropriate version that is installed on your computer.

    UrbanCode Velocity can connect to an UrbanCode Deploy server on the same network. If you install UrbanCode Velocity with Kubernetes, the Kubernetes cluster must be on the same network as the UrbanCode Deploy server.

-   Git and a GitHub account.

In addition to the requirements for all installation scenarios, you need the following items:

-   Configured OpenShift client instance. This topic uses Minishift for local instance installations.
-   OpenShift command line tool, OC, installed on the client.
-   Access to the PEM-format certificate and private key that you can use to configure an OpenShift route.
-   Master node: 2 vCPU, 8 GB RAM, 30 GB storage.
-   Other nodes: 1vCPU, 8 GB RAM, 15 GB storage.

You will install UrbanCode Velocity into a OpenShift project. During installation, you configure a MongoDB for your project, and add the UrbanCode Velocity service to your OpenShift project catalog.

1.   Get an access key. 

    The access key enables you to complete installation. [Visit the UrbanCode Velocity web portal to obtain your key](https://uc-velocity.com/). After completing the form, you can copy the access key. Store the key in a readily-available location; you use it during installation.

    **Note:** Make sure that you select a key for the product version that you want to install. Keys for the Standard Edition do not work with the Community Edition and vice-versa.

2.   Configure the containers to be run by root user. Several service containers require the admin user. From the OpenShift client command line, run the following commands:

    ```
    oc login -u system:admin -n default
    oc adm policy add-scc-to-group anyuid system:authenticated
    ```

3.   From the OpenShift dashboard, create a project for UrbanCode Velocity. When the project is created, in the **My Projects**area, select the project, and then click **Browse Catalog**.
4.   Install the UrbanCode Velocity service in the project catalog by completing the following steps:. 
    1.   [Download the UrbanCode Velocity template for OpenShift](https://github.com/IBM/velocity-se/releases). The file is named velocity-se-1-1-0-openshift.yaml 
    2.   From OpenShift project catalog, use the **Add to Project** \> **Import YAML/JSON** command to import the service into the catalog 
    3.   Select **Create**, than check **Save Template** and uncheck **Process the Template**. After refreshing the browser, you will see the UrbanCode Velocity service in the OpenShift project catalog.
5.   Create a MongoDB service for the project by completing the following steps: 
    1.   From the OpenShift project catalog, select **MongoDB**. 
    2.   On the MongoDB configuration page, in the **MongoDB Admin Password** field, enter a password for admin user. You can accept the default values for the other parameters.

        **Note:** The user must be the admin user.

    3.   Click **Create**. 
    4.   On the MogoDB has been created page, copy the `Connection URL` string and store it in a readily-accessible location, then close configuration window. You use the connection string to configure your UrbanCode Velocity project.
6.   Create UrbanCode Velocity service by completing these steps. 
    1.   From project catalog, select **Velocity** ![OpenShift Catalog](../images/OpenShiftCatalog.PNG). The Velocity configuration window is displayed.
    2.   Click **Next**, and then, in the **Name** field, enter a service name. 
    3.   In the **Hostname** field, enter the service host name. 

        The host name must resolve to a name on your DNS server, or in the server's hosts file. On Linux, the file location is etc/hosts; on Windows, the location is C:\\Windows\\System32\\drivers\\etc\\hosts.

    4.   In the **Access Key** field, paste the UrbanCode Velocity access key received earlier. 
    1.   In the **Mongo URL** field, paste MongoDB connection string for the MongoDB service. Do not use the default user or database name. Use the admin user and password you created for the admin. For example:

        ```
        mongo://admin:password@mongodb
        ```

        If you do not want to use the admin user, log on to the MongoDB and use a command similar to this one to grant admin permissions to the user:

        ```
        
        db.grantRolesToUser( "mongo", ["readWriteAnyDatabase", "dbAdminAnyDatabase", "clusterAdmin"]  )
        ```

    1.   Accept the default values for the other parameters and click **Create**. 
7.   From project dashboard, click **Applications** \> **Routes** \> **Create Route**, and then create a route by completing the following steps: 

    1.   In the **Name** field, provide a name for the route. 
    2.   In the **Hostname** field, enter the host name you specified earlier for UrbanCode Velocity. 
    3.   In the **Service** field, select the `velocity-router` service. 
    4.   Check **Secure route** and then upload your PEM-format certificate and key into the fields provided for them. 
    5.   Click **Create**. 
    The new route is listed on the Routes page.


After the installation is complete, access UrbanCode Velocity by clicking the route name link. The default user name is admin and the default password is admin.

**Parent topic:** [Installation roadmap](../topics/c_install_se_roadmap.md)

