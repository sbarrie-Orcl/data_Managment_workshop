1.  Database Development
    ====================

    1.  ### Introduction

In this lab you will deploy an APEX application to the Alpha Clone PDB
and adjust the firewall rules to support access to the application from
the Internet using a PC based browser or mobile device.

### <span id="_Toc463095767" class="anchor"><span id="_Toc487716158" class="anchor"></span></span>Objectives

-   Enable APEX in the Alpha Clone PDB.

-   Create APEX REST services

-   Deploy and access an Alpha Office APEX application.

    1.  ### Lab Requirements

-   The following lab assume that the steps outlined in lab guides 100
    and 200 have been completed.

-   VNC Viewer for access to the cloud client image

-   The SSH tunnels must be active in a terminal window.

    1.  ### <span id="_Toc463095770" class="anchor"><span id="_Toc487716160" class="anchor"></span></span>Alpha Office and APEX

        1.  #### **<span style="font-variant:small-caps;">APEX Workspace Administration</span>**

The Alpha Clone database contains an unused APEX configuration. As the
first part of this lab we will complete the configuration of the cloned
database APEX configuration.

**Note:** The standard install of APEX for a 12c database created many
objects shared by both the container and pluggable database but user and
password information is always local to the database we access. In other
words, the APEX password we set in Lab 100 has not been in the cloned
database.

-   Make sure the SSH tunnels you set up in lab 100 are still active in
    your terminal window, if not refer to lab 100 to set up the
    SSH tunnels.

<!-- -->

-   During the plug-in operation, many of the common objects in the
    pluggable database were evaluated by the database and some changes
    were made to the new database to work with its new container. One of
    these adjustments was locking the database accounts used to provide
    REST services. We will need to unlock the APEX\_LISTENER and
    APEX\_REST\_PUBLIC\_USER accounts.

<!-- -->

-   If it’s not already running, startup SQL Developer from the Cloud
    Client desktop on the VNC connection.

-   Open the DBA Window and locate the **Alpha Clone - DBCS** item
    (created in Lab 200) in the **DBA Navigator**

-   Expand it and click on the **SecurityUsers** item.

<img src="./media6/image1.png" width="272" height="408" />

-   Right-mouse on **APEX\_LISTENER** and select **Unlock User…**

<img src="./media6/image2.png" width="336" height="165" />

-   Click the **Apply** button to unlock APEX\_LISTENER. You may also
    use the SQL tab to review the unlock statement.

<img src="./media6/image3.png" width="203" height="192" />
<img src="./media6/image4.png" width="203" height="192" />

-   Repeat the **Unlock User…** operation for the
    **APEX\_REST\_PUBLIC\_USER**.

<img src="./media6/image5.png" width="419" height="234" />

<img src="./media6/image6.png" width="213" height="202" />

#### **<span style="font-variant:small-caps;">Create the Alpha Office workspace </span>**

-   In the **Chrome** browser, open up a new tab and test the updated
    rule by accessing the APEX instance in the container database from
    the Internet. Use the Public IP address from the cloud instance we
    created in the first lab.

**Note:** Be sure to use the https protocol.

**https://&lt;your-Public-IP&gt;/apex/alphaclone/apex\_admin**

<img src="./media6/image7.png" width="509" height="383" />

-   After you’ve accepted the SSL certificate and see the APEX
    administration page, enter the following admin credentials and click
    the **Login to Administration** button:

| **Username:** | admin       |
|---------------|-------------|
| **Password:** | Alpha2014\_ |

<img src="./media6/image8.png" width="283" height="232" />

-   You ***may*** be prompted to change the ADMIN user password, if not,
    skip to the next step. These credentials apply to the APEX objects
    local to the pluggable database. For convenience, we will enter the
    same password as the container database.

-   Enter the following values and click the **Apply Changes** button.

| **Enter Current Password:** | Alpha2014\_ |
|-----------------------------|-------------|
| **Enter New Password:**     | Alpha2015!  |
| **Confirm New Password: **  | Alpha2015!  |

<img src="./media6/image9.png" width="270" height="338" />

-   After logging in successfully, feel free to click around in the APEX
    interface to get familiar with it.

-   When you’re ready to begin, click the **Create Workspace** button

<img src="./media6/image10.png" width="477" height="192" />

-   At the **Identify Workspace** dialog, enter the following workspace
    name and click the **Next** button.

| **Workspace Name:** | AlphaDev |
|---------------------|----------|

<img src="./media6/image11.png" width="566" height="258" />

-   At the **Identify Schema** dialog, select and enter the following
    values followed by the **Next** button.  
      
    **Note:** Use the search icon
    <img src="./media6/image12.png" width="22" height="21" /> to find the
    ALPHA schema.

| **Re-use existing schema?** | Yes   |
|-----------------------------|-------|
| **Schema Name:**            | ALPHA |

<img src="./media6/image13.png" width="605" height="256" />

At the **Identify Administrator** dialog, enter the following values and
click the **Next** button.

| **Administrator Username:** | ADMIN                                   |
|-----------------------------|-----------------------------------------|
| **Administrator Password:** | Alpha2014\_ (May be prompted to change) |
| **Email:**                  | dummy@localhost.localdomain             |

<img src="./media6/image14.png" width="553" height="232" />

-   Review the selections on the Confirm Request page and then click the
    **Create Workspace** button.

<img src="./media6/image15.png" width="464" height="197" />
<img src="./media6/image16.png" width="457" height="154" />

-   APEX will display the ‘Workspace Created’ message

-   Click **Done**

<img src="./media6/image17.png" width="460" height="106" />

-   Click the ADMIN dropdown in the upper right and select **Signout**

<img src="./media6/image18.png" width="595" height="122" />

-   Click the Return to **‘Sign In Page’** to continue

<img src="./media6/image19.png" width="394" height="199" />

#### **<span style="font-variant:small-caps;">Build REST services</span>**

-   Login to the Alpha Office APEX development workspace using the
    following credentials.

| **Workspace:** | AlphaDev    |
|----------------|-------------|
| **Username:**  | ADMIN       |
| **Password:**  | Alpha2014\_ |

<img src="./media6/image20.png" width="260" height="285" />

-   You ***may*** be prompted to change your password. Enter the
    following values and click the **Apply Changes** button.

| **Enter Current Password** | Alpha2014\_ |
|----------------------------|-------------|
| **Enter New Password**     | Alpah2015!  |
| **Confirm New Password **  | Alpha2015!  |

<img src="./media6/image9.png" width="216" height="271" />

-   Once you’ve logged in successfully, click the **SQL
    Workshop** button.

<img src="./media6/image21.png" width="477" height="148" />

-   Click the **RESTful Services** button.

<img src="./media6/image22.png" width="617" height="128" />

-   Click the **Create &gt;** button

<img src="./media6/image23.png" width="564" height="69" />

-   There are three sections on the RESTful Services page:

<!-- -->

-   Restful Services Module

-   Resource Template

-   Resource Handler

<img src="./media6/image24.png" width="576" height="485" />

-   Fill out the information for these sections using the information
    provided below.

<!-- -->

-   For the **RESTful Services Module** section, use the following
    values:

| **Name:**       | alpha.office |
|-----------------|--------------|
| **URI Prefix:** | alphaofc/    |

<img src="./media6/image25.png" width="522" height="261" />

-   In the **Resource Template** section enter the following value:

| **URI Template** | products/ |
|------------------|-----------|

<img src="./media6/image26.png" width="436" height="168" />

-   For the last section titled **Resource Handler** use the following
    values:

| **Method:**      | GET                     |
|------------------|-------------------------|
| **Source Type:** | Query                   |
| **Format:**      | JSON                    |
| **Source:**      | select \* from products |

<img src="./media6/image27.png" width="422" height="204" />

-   Click **Create Module** to complete the REST service creation.

<img src="./media6/image28.png" width="197" height="47" />

-   APEX will show the new service module with a confirmation message.

<!-- -->

-   Click the **GET** handler for our template in the folder structure
    on the left of the screen.

<img src="./media6/image29.png" width="500" height="162" />

-   Review the definition.

-   Since this operation has no parameters, we can easily test it by
    clicking the **Test** button.

<img src="./media6/image30.png" width="438" height="521" />

-   Review the JSON produced by the service.

-   Click the browser's **back button** to return to the APEX page.

<img src="./media6/image31.png" width="575" height="219" />

#### **<span style="font-variant:small-caps;">Create a Parameterized REST Service</span>**

-   In the next section we will create a REST service that takes a
    product number and returns only one database row as a JSON object.

<!-- -->

-   Click the **Create Template** link.

<img src="./media6/image32.png" width="339" height="219" />

-   Enter the following URI Template.

**Note:** The **{id}** syntax indicates the REST call accepts one
parameter named "id" - this is automatically available in later for SQL
queries.

-   When the entry is complete, click the **Create** button.

| **URI Template:** | product/{id} |
|-------------------|--------------|

<img src="./media6/image33.png" width="389" height="136" />

-   APEX displays a success message for the new template

-   Click the **Create Handler** link under the **product/{id}**
    template on the left side of the screen.

<img src="./media6/image34.png" width="582" height="151" />

-   Enter the following SQL statement in the Source field of the
    Resource Handler page. Notice the use of the ":id" bind variable,
    this value comes from the URI template {id} provided when the
    service is invoked.

**Source:**

select \*

from products

where product\_id = :id

-   Once you’ve finished entering the SQL statement, click the
    **Create** button.

<img src="./media6/image35.png" width="443" height="369" />

-   Notice the ‘Action Processed’ at the top of your screen. We will
    test this service just like before, but we need to provide a product
    number to the call.

<!-- -->

-   **Scroll** to the bottom of the page and in the Test section, click
    the **Set Bind Variables** button.

<img src="./media6/image36.png" width="582" height="91" />

-   Enter the following product number and click the **Test** button.

| **:ID** | 1020 |
|---------|------|

<img src="./media6/image37.png" width="582" height="315" />

-   In the new browser window, notice only the single product shows in
    the JSON object.

<!-- -->

-   **Close** this pop-up window.

> <img src="./media6/image38.png" width="575" height="129" />

#### **<span style="font-variant:small-caps;">Install APEX Mobile Application</span>**

-   Click the **Application Builder** menu item on the APEX page.

<img src="./media6/image39.png" width="413" height="112" />

-   Click the **Import** button on the Application Builder page.

<img src="./media6/image40.png" width="376" height="138" />

-   Click the **Browse** button to locate the APEX application
    export file.

<img src="./media6/image41.png" width="522" height="329" />

-   Locate and open the following file and click the Open button:

**/u01/OPCWorkshop/lab/f101.sql**

<img src="./media6/image42.png" width="280" height="175" />

-   Click the **Next** button to continue.

<img src="./media6/image43.png" width="87" height="58" />

-   After a brief pause while the application file is processed, click
    the **Next** button to continue.

<img src="./media6/image44.png" width="481" height="211" />

-   On the final page, select to **Reuse Application ID 101 from Export
    File**

-   Click **Install Application**.
    <img src="./media6/image45.png" width="415" height="365" />

<!-- -->

-   APEX displays a success message for the import

-   Click **Run** **Application**.

<img src="./media6/image46.png" width="376" height="184" />

-   APEX renders the first page of the mobile application in the browser
    – it might not look quite right since we are using a
    mobile template.

<img src="./media6/image47.png" width="194" height="174" />

#### **<span style="font-variant:small-caps;">Access the Alpha Office Mobile Application on your Smart Device</span>**

-   Using any Internet connected smart phone or tablet we will access
    the mobile application using the port we opened earlier in the lab.
    This example is using an Apple iPhone 5s.

<!-- -->

-   Use your device's browser and navigate to the following URL:

**https://&lt; Public IP Address of
Alpha01A-DBCS&gt;/apex/alphaclone/f?p=101**

<img src="./media6/image48.png" width="158" height="279" />

-   The browser should prompt you to accept the unknown certificate.

-   Click of touch **Continue**.

<img src="./media6/image49.png" width="158" height="281" />

-   Touch the screen to explore the application. On the device, touching
    one of the pie slices highlights the slice; a second tap drills into
    that slice.

<img src="./media6/image50.png" width="158" height="279" />

-   Congratulations, you’ve created an application on the Oracle
    Database Cloud. This is the final lab for the DBCS Workshop.


