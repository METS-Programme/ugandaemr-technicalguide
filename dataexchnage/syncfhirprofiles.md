# Sync FHIR PROFILE SETUP
In order to set up the generic FHIR generator and sender, there are two major things to be done.
1. Creating a SyncFhirProfile.
2. Creating a Scheduler Task

## Creating a SyncFhir Profile
The following are the steps taken to create a Sync Fhir Profile
1. On UgandaEMR Home Page Go to System Administrator >>>> UgandaemrSync >>>>Sync FHIR Profile.
2. Click on the “Create Button”  to create a new Sync FHIR Profile. A popup window will show. .
3. Fill in all the necessary fields listed on the form (Refer to the SyncFHirProfile Model above to understand the different settings and what they mean)
4. Save the Profile.  The saved Profile will be shown in the form.
5. Copy the uuid of the Profile. This will be needed in the Creating a Scheduler Task.

## Creating a Scheduler Task
This is divided into two parts.

### Creating a Task Class  (Development Mode)
1. Create a Class under “org.openmrs.module.ugandaemrsync.tasks” in the UgandaEMRSyncModule. example task name “TBSyncTask.java” Take Note of the Name. You may need to use the name while creating the scheduler. For example “TB Sync” .
2. Extend the Class with class “AbstractTask”. This will require implementing the “execute” method.


     public void execute() {
     UgandaEMRSyncService ugandaEMRSyncService = Context.getService(UgandaEMRSyncService.class);
     SyncFhirProfile syncFhirProfile = ugandaEMRSyncService.getSyncFhirProfileByScheduledTaskName("Place Holder Task Name");
     SyncFHIRRecord syncFHIRRecord = new SyncFHIRRecord();
     syncFHIRRecord.generateCaseBasedFHIRResourceBundles(syncFhirProfile);
     syncFHIRRecord.sendFhirResourcesTo(syncFhirProfile);
     }
3. Copy Code above and paste it in your class.
4. Change the "Place Holder Task Name"  to a name that will match your Scheduler task to be created in OpenMRS. for example  “TB Sync”

### Creating Scheduler Task in OpenMRS
In OpenMRS creating a scheduler task see steps below.
1. Go to Legacy System Administration.>>>>Manage Scheduler. This will navigate you to the scheduler task manager.
2. Click on the “Add Task” Link. This will navigate you to the scheduler create the page.
3. Enter a Name that is the same as the placeholder that was replaced in the process of “Creating a Task Class” for example “TB Sync”.
4. Under the “Schedulable Class” field, enter the class reference for example “org.openmrs.module.ugandaemrsync.tasks.TBSyncTask”
5. In the Schedule tab, set time appropriately
Under Properties, add a property with name “syncFhirProfileUUID” and value “uuid of the sync FHIR profile” the one you had to copy.
6. Save the Scheduler.

**NB:** For redistribution purposes, ensure that your  Sync Fhir Profile and the Scheduler are added to the liquibase changeset file for the sync module. 

