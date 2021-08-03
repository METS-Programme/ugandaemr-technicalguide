# Releasing UgandaEMR Versions 

UgandaEMR leverages the OpenMRS infrastructure on BinTray offered by JFrog, which is a maven based distribution service. This allows UgandaEMR modules to be used by the greater OpenMRS community, if needed and automation of the processes for release management.

The release process uses the maven release plugin 

## Pre-requisites 

The following must be setup 

1. Bintray access to the repository servers 
2. SSH keys for GitHub access for the computer to be used to carry out the release 
3. Write access to the repositories to be released 
4. Maven 3.3.9 and Java 8

## Release Process

The process for releasing is as follows:

1. Setup Bintray credentials in the local .m2 repository settings in the settings.xml 

   `<server>

   ​      <username></username>

   ​      <password></password>

   ​      <id>openmrs-repo</id>

   ​    </server>

   ​    <server>

   ​      <username></username>

   ​      <password></password>

   ​      <id>openmrs-repo-modules</id>

   ​    </server>

   ​    <server>

   ​      <username></username>

   ​      <password></password>

   ​      <id>openmrs-repo-releases</id>

   ​    </server>

   ​    <server>

   ​      <username></username>

   ​      <password></password>

   ​      <id>openmrs-repo-snapshots</id>

   ​    </server>`

2. Create a new clone of the project directly from the module repository, recommended is to name the clone release-openers-module-XXX. This differentiates it from the regular working repositories 

3. Pull all the changes from the master branch 

4. Prepare for release by running the command `mvn release:prepare` and follow the interactive instructions there are usually no changes required from the defaults. This prepares the artifacts for release

5. Perform the release by running the command `mvn release:perform -Darguments="-Dmaven.javadoc.skip=true"` there are is usually no user intervention needed 

6. Update the release versions in the pom.xml files for any dependent modules and repeat the process from #2 above 

## Release Order 

The order of releasing the module is usually as follows, with the ones at the top of the list being released first:

1. openmrs-module-fingerprint - does not have any dependencies on any modules 
2. openmrs-module-ugandaemrsync - has a dependency on ugandaemrreports but does not require an up-to-date version 
3. openmrs-module-ugandaemrreports - has a dependency on ugandaemrsync 
4. openmrs-module-aijar - has the latest versions of all the modules 
5. openmrs-module-ugandaemr-poc - depends on aijar distribution modules 
6. Custom distributions based on UgandaEMR:
   - openmrs-module-ugandaemr-iss - ISS custom implementation
   - Openmrs-module-ugandaemr-mulagoemr - MUJHU custom implementation for MCH in Mulago and Kawempe hospitals 
   - openmrs-module-ugandaemr-prisonemr - Uganda Prisons Service distribution 
   - openmrs-module-ugandaemr-updfemr - Uganda People's Defence Forces distribution 

## Demo Site Upgrade

Once the release process has been completed the demo site needs to be updated with the latest release version 

## Troubleshooting Tips

### Failure to Upload the Release Artifacts 

There are times when performing the release fails, usually due to permission errors (403 Forbidden) or if the servers are not available. In this case the steps below can be used to deploy the artifacts manually 

1. Checkout the tag for the release `git checkout tag`
2. Run `mvn clean deploy` which compiles the artifacts for the tag and deploys them to the relevant repositories  
