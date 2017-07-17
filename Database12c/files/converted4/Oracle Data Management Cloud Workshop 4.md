<img src="./media4/image1.jpeg" width="162" height="304" />

-   Accept the defaults and Click **OK**.

<img src="./media4/image2.jpeg" width="487" height="157" />

-   Close the Confirmation pop up by clicking **OK**.

-   Reopen the pluggable database via **Actions Open**.

-   Accept the default to open it Read / Write. Click **OK**.

<img src="./media4/image3.jpeg" width="442" height="167" />

-   Close the Confirmation pop up by clicking **OK**.

-   Refresh the browser page using the refresh icon in the top
    right corner.

<img src="./media4/image4.png" width="576" height="61" />

-   Note that the violations are now gone.

<img src="./media4/image5.jpeg" width="576" height="128" />

-   Click the **ALPHACLONE** container name link to review the
    database information.

<img src="./media4/image6.png" width="575" height="272" />

#### **<span style="font-variant:small-caps;">Create an SQL Developer connection to the Public Cloud database ALPHACLONE schema</span>**

-   In the SQL Developer application, click the green plus sign
    <img src="./media4/image7.png" width="21" height="22" /> in the
    Connections window to create a new connection; enter the following
    connection details:

| **Connection Name:** | Alpha Clone – DBCS                                     |
|----------------------|--------------------------------------------------------|
| **Username:**        | alpha                                                  |
| **Password:**        | oracle                                                 |
| **Check:**           | “Save Password”                                        |
| **Connection Type:** | SSH                                                    |
| **Service Name:**    | Alphaclone.&lt;Your ID Domain&gt;.oraclecloud.internal |

***Note:** You can optionally select a color for the connection to
differentiate it from other connections.*

<img src="./media4/image8.png" width="478" height="304" />

-   Click **Test** to confirm the information was entered correctly.

<img src="./media4/image9.png" width="576" height="301" />

-   Click **Connect** to save the connection information and open a
    new SQL Worksheet.

<img src="./media4/image10.png" width="576" height="372" />

-   You have successfully migrated a pluggable database from on premise
    to the cloud. In the next section we’ll migrate data using
    Data Pump.

    1.  ### <span id="_Toc463095443" class="anchor"><span id="_Toc487716149" class="anchor"></span></span>Cloud Migration Using Data Pump

        1.  #### **<span style="font-variant:small-caps;">Export the Alpha Schema</span>**

The first step will be to create a local Data Pump Directory.

-   In the Connections Tab inside the "On-Premise" folder navigate to
    the **Alpha - PDB Directories** item, right-mouse click and select
    **Create Directory…**

<img src="./media4/image11.png" width="152" height="288" />

> **Note**: The default Data Pump directory object, DATA\_PUMP\_DIR,
> does not work with PDBs. Data Pump requires an explicit directory
> object within the PDB for exporting or importing schemas or tables.

-   Enter the following values and click **Apply**. Remember to use the
    SQL tab to review the actual DDL statement. Click **OK** to dismiss
    the confirmation.

| **Directory Name:**            | alpha\_backup\_dir (not case sensitive) |
|--------------------------------|-----------------------------------------|
| **Database Server Directory:** | /u01/OPCWorkshop                        |

> **NOTE:** You may receive an error message stating that “An error was
> encountered performing the requested operation:” and that the
> directory cannot be created. To eliminate this error right-click on
> Alpha - PDB and choose Disconnect. Then Reconnect. The error occurs
> because you were connected earlier while performing the UNMOUNT /
> REMOUNT and during the previous “cloning” of the PDB container the
> connection information was lost. Reconnecting will normally solve this
> issue.

<img src="./media4/image12.jpeg" width="397" height="167" />

<img src="./media4/image13.png" width="358" height="242" />

<img src="./media4/image14.png" width="358" height="241" />

<img src="./media4/image15.png" width="193" height="102" />



-   Now that we’ve created the Data Pump export directory the next steps
    will outline how to create and run a Data Pump Export job using SQL
    Developer

<!-- -->

-   In the DBA Window, **Add Connection** by clicking on the Green
    Plus sign.

<img src="./media4/image16.png" width="174" height="104" />

-   Select the **Alpha - PDB** connection and click the **OK** button.

<img src="./media4/image17.png" width="220" height="101" />

-   Expand **Alpha - PDB**, expand **Data Pump**, then right-mouse-click
    on **Export Jobs,** and then select the **Data Pump Export Wizard…**
    menu item.

<img src="./media4/image18.png" width="192" height="178" />

-   Select the **Schemas export** type and click the **Next** button.

<img src="./media4/image19.png" width="299" height="225" />

-   Select the **ALPHA** schema and use the blue arrow to move it to the
    right-hand column. Click **Next**.

<img src="./media4/image20.png" width="299" height="224" />

-   We are not filtering out any objects, click the **Next** button.

<img src="./media4/image21.png" width="299" height="224" />

-   We are not applying where clauses to table data, click the
    **Next** button.

<img src="./media4/image22.png" width="299" height="224" />

-   We want a log for this export, and just like the actual export file,
    we must pick a directory from the list of directories in
    the database.

<!-- -->

-   Select **ALPHA\_BACKUP\_DIR** from the list and click the
    **Next** button.

<img src="./media4/image23.png" width="281" height="211" />

-   The most important selection for any Data Pump operation is choosing
    the directory where the export file will be written.

<!-- -->

-   Select **ALPHA\_BACKUP\_DIR** from the Directories drop down list.

-   Then, select the **Delete Existing Dump Files** radio button and
    click the **Next** button.

**Note**: Data Pump always uses a server side directory for all export
or import operations.

<img src="./media4/image24.png" width="299" height="224" />

-   Data Pump jobs can be scheduled to run at any time and on any
    desired times of the day, week or year. We will run the job
    immediately - click the **Next** button.

<img src="./media4/image25.png" width="299" height="224" />

-   On the Summary panel, click the **PL/SQL** tab to review the
    job definition. Review the PL/SQL use of Oracle Supplied PL/SQL
    subprograms for Data Pump. Click the **Finish** button to create
    the job.

| <img src="./media4/image26.png" width="299" height="225" /> | <img src="./media4/image27.png" width="221" height="165" /> |
|------------------------------------------------------------|------------------------------------------------------------|

-   For a brief time, SQL Developer shows a progress dialog while it
    creates the job in the database.

**Note:** the import actually runs as a job in the database so this
message is only about creating and scheduling the export.

<img src="./media4/image28.png" width="242" height="110" />

-   While the job is running, you may view status information by
    clicking on the export job added to the DBA Navigator panel. It may
    take a couple of minutes so click the **Refresh**
    icon<img src="./media4/image29.jpeg" width="28" height="23" /> until
    the job is completed (NOT RUNNING).

<img src="./media4/image30.png" width="158" height="144" />
<img src="./media4/image31.png" width="340" height="139" />

-   Now we’ll copy the export Data Pump file to the server

<!-- -->

-   Start a Terminal window using the top panel icon.

<img src="./media4/image32.png" width="514" height="118" />

-   Enter the following commands to print the working directory
    (**pwd**), list the directory (**ls**) contents and review the Data
    Pump log file.

$ pwd

$ ls

$ cat EXPDAT.LOG

<img src="./media4/image33.png" width="555" height="126" />

<img src="./media4/image34.png" width="576" height="297" />

Use the following secure copy (**scp**) command to transfer the Data
Pump export to the DBCS server. Use the Database Service Private IP
address you identified in the first lab.

$ scp -i lab/labkey EXPDAT01.DMP oracle@&lt;Alpha01A-DBCS public
IP&gt;:~

***Note:** the tilde (~) represents the oracle user's home directory.*

<img src="./media4/image35.png" width="576" height="81" />

#### **<span style="font-variant:small-caps;">Import Alpha to a new Schema</span>**

-   As we begin the import phase of this example we’ll first create an
    import directory in the Alpha Clone PDB.

<!-- -->

-   Use SQL Developer and expand the **Alpha Clone - DBCS** connection.

-   Right-mouse-click on the **Directories** tree item and select the
    **Create  Directory…** menu item.

<img src="./media4/image36.png" width="212" height="419" />

-   Enter the following values and click the **Apply** button.

-   Click **OK** to dismiss the confirmation message. This lets the
    database access the same directory where the Data Pump export file
    was copied.

| ***Directory Name:***                                      | alpha\_import\_dir |
|------------------------------------------------------------|--------------------|
| ***Database Server Directory:***                           | /home/oracle       |
| <img src="./media4/image37.png" width="316" height="213" /> |                    |

-   The next few steps will outline creating the Data Pump Import job.
    To access the Data Pump features, we need to add the clone
    connection to the DBA Navigator.

<!-- -->

-   Click on the green plus sign, Add Connection icon on the DBA
    Navigator panel

<img src="./media4/image16.png" width="174" height="104" />

-   Select **Alpha Clone - DBCS** connection and click **OK**.

<img src="./media4/image39.png" width="216" height="98" />

-   Expand **Alpha Clone - DBCS** **Data Pump**

-   Right-mouse on the **Import Jobs** menu item, and select **Data Pump
    Import Wizard…** menu item.

<img src="./media4/image40.png" width="217" height="210" />

-   Select **Schemas** from the ‘Type of Import box and Choose
    **ALPHA\_IMPORT\_DIR** from the ‘Choose Input Files’ drop down list,
    then click **Next**.

<img src="./media4/image41.png" width="299" height="224" />

**Note:** This action might take a few minutes. There is some wait time
while the database locates and scans the import file in the selected
directory.

<img src="./media4/image42.png" width="264" height="178" />

-   Move the **ALPHA** schema from the left to the right column using
    the arrow button and click **Next**.

<img src="./media4/image43.png" width="299" height="224" />

-   For this lab, we are creating a new schema, so we will enter the new
    schema name as the destination.

<!-- -->

-   Under the Re-Map Schemas section click **Add Row**.

-   Enter the following values and click the **Next** button.

| **Source: **     | ALPHA (should be the default item) |
|------------------|------------------------------------|
| **Destination:** | ALPHA\_COPY                        |

<img src="./media4/image44.png" width="299" height="224" />

-   We want to see the log output so we will select the same directory
    as the import file directory.

<!-- -->

-   Select **ALPHA\_IMPORT\_DIR** and click the **Next** button.

<img src="./media4/image45.png" width="295" height="220" />

**Note:** For lab purposes we will execute the import immediately. In
normal operations this job could be set up to refresh a development
database on a daily basis.

-   Click the **Next** button.

<img src="./media4/image46.png" width="299" height="224" />

-   Click the PL/SQL tab to review the small program that establishes
    the import job. Click the **Finish** button to create the job.

<img src="./media4/image47.png" width="277" height="208" />
<img src="./media4/image48.png" width="251" height="188" />

**Note:** For a period of time SQL Developer shows a progress dialog
while the job is being created. The job does not run locally you’re
seeing the progress of creating the job in the database.

<img src="./media4/image28.png" width="248" height="113" />

-   Locate and click on the job name to see the detailed status as the
    job runs. When the job completes, the database automatically removes
    the job. You will need to use the **Refresh** icon
    <img src="./media4/image29.jpeg" width="28" height="23" /> to see
    when the job finishes.

<img src="./media4/image49.png" width="198" height="185" />
<img src="./media4/image50.png" width="312" height="144" />

-   If you are interested in verifying that the ALPHA\_COPY schema is
    the same as the ALPHA schema, feel free to create a connection
    and compare.

    1.  ### <span id="_Toc463095446" class="anchor"><span id="_Toc487716150" class="anchor"></span></span>Cloud Migration Using SQL Developer Carts

        1.  #### **<span style="font-variant:small-caps;">Creating an SQL Developer Cart</span>**

The SQL Developer Cart is a convenient method for organizing the
deployment of database objects and data from one database to another. In
this trivial example, we want to update the data of just the CUSTOMERS
and PRODUCTS table in the development cloud database. More elaborate
usages of the cart can help package entire application deployments,
including pre and post processes from multiple data sources.

-   Show the Cart using the **View** &gt; **Cart** menu option.

<img src="./media4/image51.png" width="299" height="244" />

-   If Cart\_1 is not already created (it should be), Click on the **New
    Cart** icon.

<img src="./media4/image52.png" width="542" height="283" />

-   Drag the **CUSTOMERS** table from the **Alpha - PDB** connection to
    the cart.

<img src="./media4/image53.png" />
<img src="./media4/image54.png" width="576" height="298" />

-   Drag the **PRODUCTS** table to the cart.

<img src="./media4/image55.png" width="576" height="298" />

-   Include a script that runs before any other Cart activity. For this
    lab, we are disabling all the referential integrity constraints so
    we can delete and insert data without regard to foreign keys on
    our tables.

-   In the Cart window click small down arrow next to the **green plus**
    “+” icon and select **Add Initial Script**

<img src="./media4/image56.png" width="576" height="160" />

-   Click the **Browse…** button.

<img src="./media4/image57.png" width="299" height="144" />

-   Locate the following file and click **Open**:

/u01/OPCWorkshop/lab/disable-constraints.sql

<img src="./media4/image58.png" width="299" height="233" />

-   Click **OK**.

-   Click the down arrow next to the **green plus** “+” icon again and
    select **Add Final Script** which is included as the last operation
    performed during the cart operations.

**Note:** There can only be one Initial or Final script in a Cart.

<img src="./media4/image59.png" width="299" height="145" />

-   Click the **Browse…** button

<img src="./media4/image60.png" width="299" height="145" />

-   Locate the following file and click **Open**:

/u01/OPCWorkshop/lab/enable-constraints.sql

<img src="./media4/image61.png" width="299" height="234" />

-   Click **OK**.

-   We are not creating any tables in this lab; uncheck the **DDL**
    column heading.

<img src="./media4/image62.png" width="299" height="84" />

-   We will move the data, include a check the **Data** column heading.

<img src="./media4/image63.png" width="299" height="85" />

-   Before we can overwrite the new rows in the CUSTOMERS table, we need
    to truncate the table.

<!-- -->

-   Click in the **Scripts** column cell for the CUSTOMERS table and
    then click the **pencil icon**.

<img src="./media4/image64.png" width="576" height="102" />

-   Check the **Before Load** box, then click the **Browse…** button and
    select the following file:

/u01/OPCWorkshop/lab/truncate-customers.sql

-   Click **OK**:

<img src="./media4/image65.png" width="181" height="256" />

-   Repeat the operation for the PRODUCTS table; click the **pencil
    icon** on the products row.

<img src="./media4/image66.png" width="576" height="105" />

-   Click the **Before Load** button, then click on the **Browse**…
    button and select the following file:

/u01/OPCWorkshop/lab/truncate-products.sql

-   Click **OK**:

<img src="./media4/image67.png" width="201" height="287" />

#### **<span style="font-variant:small-caps;">Export the SQL Developer Cart</span>**

-   Now that the cart is complete, click the **Export Cart** toolbar
    icon to generate the script of all the elements we inserted in
    the cart.

<img src="./media4/image68.png" width="299" height="101" />

-   Click the **Apply** button to generate script.

**Note:** The selections on this page may be saved and later reused if
the cart is regularly used the same way.

<img src="./media4/image69.png" width="202" height="224" />

-   If the file already exists, SQL Developer asks you to confirm
    overwriting it with new content. If you see this prompt, click the
    **Yes** button.

<img src="./media4/image70.png" width="299" height="109" />

-   Review the contents of the script with particular attention to the
    SQL statements that have been inserted based on the scripts
    we included.

<img src="./media4/image71.png" width="576" height="210" />

-   Run the script by clicking the **Run Script** icon and selecting the
    **Alpha Clone** **— DBCS** connection.

<img src="./media4/image72.png" width="352" height="228" />

-   Click **OK**.

<!-- -->

-   SQL Developer shows a progress bar while the script runs. Depending
    on your window layout, you may see the command output scrolling by
    while the script runs.

<img src="./media4/image73.png" width="377" height="143" />

-   When the script is complete, review the script output looking for
    the execution of both the script elements and the DML statements.

<img src="./media4/image74.png" width="377" height="127" />

-   This concludes Lab 2 – Cloud Migration, proceed to the next lab when
    you’re ready.

<span id="_Toc463095627" class="anchor"></span>

1.  Backup and Recovery
    ===================

    1.  ### Introduction

Oracle Database Backup Service (ODBS) is a new backup-as-a-service
offering that enables customers to store their backups securely in the
Oracle cloud. ODBS provides a transparent, scalable, efficient, and
elastic cloud storage platform for Oracle database backups. The Client
side Oracle Database Cloud Backup Module which is used with Recovery
Manager (RMAN) transparently handles the backup and restore operations.

Oracle Database Cloud Backup Module is the cloud backup module that is
installed in the database server. During the install process, a platform
specific backup module is downloaded and installed. The RMAN environment
of the client database is configured to use the cloud backup module to
perform backups to ODBS. Using familiar RMAN commands, backups and
restores are transparently handled by the cloud backup module.

### <span id="_Toc463095628" class="anchor"><span id="_Toc487716153" class="anchor"></span></span>Objectives

-   Install the Oracle Database Cloud Backup Module onto the VM image
    provided in the workshop. The database provided in the VM represents
    the on premise database in a typical customer situation.

-   Configure RMAN to support the Oracle Database Cloud Backup Module.
    Then, backup the database and take a restore point to be used
    for Point-In-Time-Recovery.

-   Simulate a destructive database operation and then restore and
    recover to a specific Point-In-Time.

    1.  ### <span id="_Toc463095629" class="anchor"><span id="_Toc487716154" class="anchor"></span></span>Lab Requirements

<!-- -->

-   VNC Viewer to access the client system

    1.  ### <span id="_Toc463095631" class="anchor"><span id="_Toc487716155" class="anchor"></span></span>Oracle Public Cloud Backup Recovery

        1.  #### **<span style="font-variant:small-caps;">Start the On-Premise Oracle Database</span>**

<!-- -->

-   Access the Virtual Client image following the prior instructions
    regarding the VNC viewer.

-   If your local database is not running for some reason (it should be
    at this point) locate and double-click the **StartDB** icon**.**

<img src="./media4/image75.png" width="307" height="194" />

#### **<span style="font-variant:small-caps;">Install the Cloud Backup Module</span>**

-   The .jar file (opc\_install.jar) used to install the Cloud Backup
    Module has already been placed into the
    /u01/OPCWorkshop/lab directory.

<!-- -->

-   **Open a Terminal Window**, cd into the **lab** directory and
    execute the following OS commands to verify that
    opc\_install.jar exists.

$ cd lab

$ pwd

$ ls \*.jar

<img src="./media4/image76.jpeg" width="491" height="168" />

-   The installation command with all of the options is rather lengthy.
    In order to make things easier for you and eliminate potential typos
    the installation command has been saved into a text file named
    **Workshop\_Commands\_URLs.txt**. The file is represented by an icon
    on the Client Image Desktop.

<!-- -->

-   Double click on the Workshop\_Commands\_URLs.txt icon to open up
    the file.

<img src="./media4/image77.jpeg" width="150" height="98" />

-   Find the “**OPC Cloud Backup Installation**” section in the
    text file.

-   Replace **&lt;opc-identity-domain&gt; &lt;opc-username&gt;** and
    **&lt;opc-passwd&gt;** (including replacing the &lt;&gt;) with the
    **Identity Domain, Username, and Password** student account
    information you were assigned. Also, be sure to put single quotes
    around your password to avoid any issues with special characters.

<!-- -->

-   Before:

<img src="./media4/image78.png" width="576" height="88" />

-   After

<img src="./media4/image79.png" width="576" height="109" />

-   **Copy and Paste** the updated command from the text file into your
    terminal and hit Enter.

<img src="./media4/image80.png" width="576" height="105" />

-   The installation command creates a configuration file
    “**opcorcl.ora**” and wallet directory “**opc\_wallet**” and places
    these in $ORACLE\_HOME/dbs. It also downloads a library file
    “**opclib.so**” that RMAN uses to communicate with the Oracle
    Database Backup Service and places that in $ORACLE\_HOME/lib. You
    specified these locations in the syntax of the install command.


