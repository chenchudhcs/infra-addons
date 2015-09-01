# Infra-Addons

As part of the Infratructure team we come accross various challenges in implementing the Continous Integration cycle in a project.
One such challenge was to automate the incremental DBCR process with each run of the CI cycle so that the development team is provided with immediate feedback 
of the check-in's and are well aware of any Integration issues much before the sprint delivery.

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


