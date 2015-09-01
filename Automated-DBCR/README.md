# Infra-Addons

As part of the Infrastructure team we come accross various challenges in implementing the Continous Integration cycle in a project. One such challenge was to automate the incremental DBCR process with each run of CI cycle so that the development team is provided with immediate feedback of check-in's and are well aware of any Integration issues much before the sprint delivery.

#General DBCR process

1. Developer identifies DB change and raises a DBCR during or after component design.

2. Usually Jira or another tool is used as a tracker to raise and track DBCR’s lifecycle.

3. DBA or Architect reviews the DBCR for quality and conflicts across tracks. At times DBCR review requires members from different 
tracks to be available for reviewing dependencies.

4. Approved DBCR’s are applied by DBA or the build person during deployment.

5. Rejected DBCR’s are assigned back to developers and start their journey again from bullet point .

# DBCR and CI challenges

1. Lot of development being done is customization of products which come with their starting data model and it is challenging to track changes incrementally. 
Full DB builds are not possible.

2. Ensuring sequence using some of traditional methods like creating a master file which maintains a sequence is time consuming and manual.

3. With single master script to run DBCR’s it becomes cumbersome to tag a set of DBCR’s for a certain build or version number. 

4. Since we have multiple environments which get builds at different frequency, identifying the delta DBCR’s from  the previous build to the new build and creating a 
single script to run the delta is manual and error prone task.

#Where we usually stop?

1. Given the challenges with DBCR management, completely automated CI deployment is not followed.

2. If daily deployments are followed, they require manual intervention either to apply DBCR before or after the build depending on application requirements. 
At times one team member is aligned to perform daily deployments.

3. Excel or another tool is used to manually find delta of DBCR’s to be applied to the destination environment during build phase, instead of deploy phase.

#Solution – DBCR File naming convention

1. Solution approach described pivots on the file naming convention used to name the approved DBCR files

2. Once a DBCR is approved it is added to SCM tool and the file is named as timestamp: - YYYYMMDDHHmm-<<RS ID>>.sql.

3. Minutes (mm) in the filename can be used to sequence the DBCR while they are being applied.

Example: 201502011600-323232.sql file represents a DBCR (323232) that has been approved on 1st Feb 2015 at 4:00 PM

#Solution  - Process Flow

Pre-build Phase - 

1. Developer Raises DBCR
2. Arch or DBA reviews the DBCR and assigns it further to Infra Team for inclusion in the Build.


Build Phase -

1. Get the latest of approved DBCR
2. Package all the approved SQl files till date

Pre-Deploy Phase -

1. Select the DBCR Filenames from DBCRTRACKER TABLE of the environment
2. Find the diff of the filenames present in the deployment packet vs the file list in the DB
3. validate if the diff filelist is part of the  deployment packet

Deploy Phase

1. Iterate Over list of DBCR's to be applied based on the timestamp (to figure out the sequence of the DBCR)
2. Update the SQL files with insert into DBCRTRACKER table
3. Run DBCR (We use ant task here)

#Sample data model for DBCRTRACKER table:

DBCR_ID           | FILENAME                  | BUILD_NUMBER                  | DATE_CREATED 

1. Filename: column is added since at times there can be multiple SQL scripts as a part of the DBCR.
2. Build number: build version, SVN version or any other tracking parameter can be used. Ideally it should be same tracking version being used for rest of the code. This helps in identifying when the DBCR was applied to the environment

#Key success factors and Gaps

1. Success of this process lies in the process of adhering to one single rule: Never modify an approved DBCR or manually apply DBCR’s to any environment.

2. Developers should submit DBCR’s on a daily basis instead of weekly (right before the build)

3. Any file deletion of sql’s from svn/git is easily tracked, which helps a lot in maintaining the dbcr’s and keep infra team always informed.








