# Releasing UgandaEMR Versions 

UgandaEMR leverages the OpenMRS infrastructure on BinTray offered by JFrog, which is a maven based distribution service. This allows UgandaEMR modules to be used by the greater OpenMRS community, if needed and automation of the processes for release management.

The release process uses the maven release plugin 

## Release Process

The process for releasing is as follows:

1. Setup UgandaEMR CI credentials in the local .m2 repository settings 
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