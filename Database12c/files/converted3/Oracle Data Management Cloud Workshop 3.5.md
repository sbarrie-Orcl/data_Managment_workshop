-   Enter the following connection details:

| **Connection Name:** | sys - OPCDBCS                                    |
|----------------------|--------------------------------------------------|
| **Username:**        | sys                                              |
| **Password:**        | Alpha2014\_                                      |
| **Check:**           | “Save Password”                                  |
| **Connection Type:** | SSH                                              |
| **Role:**            | SYSDBA                                           |
| **Service Name:**    | ORCL.&lt;Your Domain ID&gt;.oraclecloud.internal |

***Note:** You can optionally select a color for the connection to
differentiate it from other
connections.*<img src="./media3/image1.jpeg" width="476" height="308" />

-   Click **Test** to confirm the information was entered correctly.
    Verify that you receive a ‘Success’ status.

<img src="./media3/image2.png" width="576" height="303" />

-   Click **Connect** to save the connection information which opens a
    new SQL Worksheet.

<img src="./media3/image3.jpeg" width="497" height="226" />

#### **<span style="font-variant:small-caps;">Copy the Clone Pluggable Database to the Cloud</span>**

In this step we will copy the cloned pluggable database to the cloud
using SQL Developer.

-   Click on **ViewTask Progress** to open up the Task Progress window.

-   In the DBA window expand ‘**sys – CDB**’ and expand ‘**Container
    Database**’, then right-click on **ALPHACLONE** and select “**Clone
    PDB to Oracle Cloud**”

<img src="./media3/image4.jpeg" width="381" height="276" />

-   Nothing needs to be changed in this window, verify that the default
    properties include your Public Cloud Connection. Click **Apply**.

<!-- -->

-   Source PDB: **ALPHACLONE**

-   Destination Connection: **sys - OPCDBCS**

-   **Action after clone: RePlug**

<img src="./media3/image5.jpeg" width="422" height="155" />

-   You will note in the Task Progress window the progress of moving the
    datafiles over to the cloud database. This task will take about a
    minute to complete.

<img src="./media3/image6.jpeg" width="537" height="268" />

-   Upon completion of the transfer you will be alerted to at least two
    Plugin Violations. This is because the patch level of the local
    ALPHACLONE pluggable database is different than the Container
    database in the cloud. We will remedy this in the next few steps.

-   Click **OK** for **each** popup.

**Note:** The datafiles will be transferred despite what the pop up
implies.

<img src="./media3/image7.jpeg" width="254" height="135" /><img src="./media3/image8.jpeg" width="268" height="134" />

####  **<span style="font-variant:small-caps;">Use EM Express to plug the transferred database</span>**

-   Open Chrome by clicking the icon on the menu bar or the Desktop.

<img src="./media3/image9.png" width="309" height="21" />

-   Enter the following URL into the Address bar or click the "**EM
    Express - DB**" link in the header bar –
    **https://localhost:5500/em**

**Note:** When using localhost:5500 in the URL below, your browser
request is routed through the SSH proxy that we loaded in a terminal
window in the first lab. If for some reason that window was closed, or
is not working, you should refer to the first lab in step 1.6.2

<img src="./media3/image10.png" width="453" height="60" />

-   Enter the following login credentials, check the "**as sysdba**" box
    and click the Login button:

| **User Name:** | sys         |
|----------------|-------------|
| **Password:**  | Alpha2014\_ |
| **Check:**     | as sysdba   |

<img src="./media3/image11.png" width="458" height="226" />

-   We will now plug the Alpha Clone database into the Cloud database.
    From the Database Home page, click the **CDB(1 PDBs)** link.

<img src="./media3/image12.png" width="254" height="145" />

-   Open the **Actions** list and select the **Plug** command

<img src="./media3/image13.png" width="131" height="181" /><img src="./media3/image14.png" width="327" height="256" />

-   Enter the following filename and directory location in the Metadata
    File field in the Plug PDB dialog box.

Metadata File:
**/u02/app/oracle/oradata/ORCL/ALPHACLONE/ALPHACLONE.XML**

-   **Uncheck** the **Reuse source datafile location from Metadata
    File** check box

-   Enter the Source Datafile Location:

Source Datafile Location: **/u02/app/oracle/oradata/ORCL/ALPHACLONE**

-   Click OK

<img src="./media3/image15.jpeg" width="343" height="185" />

-   The Processing message displays for the 2 minutes (approximately)
    required to plug the database into the container. Click the **OK**
    button when the Confirmation message displays.

| <img src="./media3/image16.png" width="241" height="60" /> |     | <img src="./media3/image17.png" width="284" height="100" /> |
|-----------------------------------------------------------|-----|------------------------------------------------------------|

-   Notice the database is now in the list of Containers**. **

**Note: There will be Violations because of the patch level mismatch
between the original source Pluggable database and the Cloud Container
database**.

<img src="./media3/image18.jpeg" width="576" height="110" />

-   You now need to SSH into the Cloud database server in order to patch
    the database. Example is shown below. Substitute your Cloud database
    server IP address (Alpha01A-DBCS)

-   Open a Terminal and type the following SSH command to connect to the
    cloud database server.

-   ssh -o ServerAliveInterval=60 -i /u01/OPCWorkshop/lab/labkey
    oracle@&lt;Alpha01A-DBCS-IP-address&gt;

<img src="./media3/image19.png" width="575" height="62" />

-   We’ll need to run the **datapatch script** to apply any
    missing patches. It should run with no errors.

**Note: If it complains the first time about not being able to determine
the current opatch status then wait a minute until it’s had time to pick
up the newly cloned pluggable database and retry.**

-   $ORACLE\_HOME/OPatch/datapatch -verbose

<img src="./media3/image20.jpeg" width="576" height="182" />

<img src="./media3/image21.jpeg" width="576" height="273" />

In the next few steps we’ll upgrade the PDB.

**Note:** If you receive an error message like, “**The pluggable
databases that need to be patched must be in upgrade mode**” complete
the following upgrade PDB Step. If not, proceed directly to the next
step (Close and Reopen ALPHACLONE PDB).

<img src="./media3/image22.png" width="576" height="303" />

-   Put the database in upgrade mode to correct the patch errors.

<!-- -->

-   Connect to container database using SQL Plus and place the database
    in **upgrade mode**. Once completed run **datapatch** again and you
    should have no errors. Run the following commands to complete
    this step.

-   sqlplus / as sysdba

-   alter pluggable database ALPHACLONE close;

-   alter pluggable database ALPHACLONE open upgrade;

-   exit

-   $ORACLE\_HOME/OPatch/datapatch -verbose

<img src="./media3/image23.png" width="576" height="285" />

<img src="./media3/image24.png" width="576" height="306" />

-   **Close and Reopen ALPHACLONE PDB**

<!-- -->

-   The final step is to close and reopen the ALPHACLONE
    pluggable database. Go back to EM Express, with the ALPHACLONE
    row highlighted. (DO NOT CLICK THE ALPHACLONE LINK).

-   Select **Actions Close**.


