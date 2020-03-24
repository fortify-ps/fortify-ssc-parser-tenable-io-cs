# Fortify SSC Parser Plugin for Tenable.io Container Security

This Fortify SSC parser plugin allows for importing scan results from Tenable.io Container Security.

### Related Links

* **Downloads**:  
  _Beta versions may be unstable or non-functional. The `*-licenseReport.zip` and `*-dependencySources.zip` files are for informational purposes only and do not need to be downloaded._
  * **Release versions**: https://bintray.com/package/files/fortify-ps/binaries/fortify-ssc-parser-tenable-io-cs-release?order=desc&sort=fileLastModified&basePath=&tab=files  
  * **Beta versions**: https://bintray.com/package/files/fortify-ps/binaries/fortify-ssc-parser-tenable-io-cs-beta?order=desc&sort=fileLastModified&basePath=&tab=files
  * **Sample input files**: [src/test/resources](src/test/resources)
* **Automated builds**: https://travis-ci.com/fortify-ps/fortify-ssc-parser-tenable-io-cs
* **Tenable.io Container Security website**: https://www.tenable.com/products/tenable-io/container-security


## Usage

The following sections describe how to install and use the plugin. For generic information
about how to install and use SSC parser plugins, please see the Fortify SSC documentation.

### Plugin Install & Upgrade

* Obtain the plugin binary jar file
  * Either download from Bintray (see [Related Links](#related-links)) 
  * Or by building yourself (see [Information for plugin developers](#information-for-plugin-developers))
* If you already have another version of the plugin installed, first uninstall the plugin by following the steps in [Plugin Uninstall](#plugin-uninstall)
* In Fortify Software Security Center:
  * Navigate to Administration->Plugins->Parsers
  * Click the `NEW` button
  * Accept the warning
  * Upload the plugin jar file
  * Enable the plugin by clicking the `ENABLE` button
  
### Plugin Uninstall

* In Fortify Software Security Center:
  * Navigate to Administration->Plugins->Parsers
  * Select the parser plugin that you want to uninstall
  * Click the `DISABLE` button
  * Click the `REMOVE` button 

### Obtain results

Please see the Tenable.io Container Security documentation for details on scanning applications and 
generating reports. Note that the SSC parser plugin requires the uploaded reports to be in JSON
format.

### Upload results

SSC web interface (manual upload):

* Navigate to the Artifacts tab of your application version
* Click the `UPLOAD` button
* Click the `ADD FILES` button, and select the JSON file to upload
* Enable the `3rd party results` check box
* Select the `TENABLE_IO_CS` type
  
SSC clients (FortifyClient, Maven plugin, ...):

* Generate a scan.info file containing a single line as follows:  
  `engineType=TENABLE_IO_CS`
* Generate a zip file containing the following:
  * The scan.info file generated in the previous step
  * The JSON file containing scan results
* Upload the zip file generated in the previous step to SSC
  * Using any SSC client, for example FortifyClient
  * Similar to how you would upload an FPR file



## Information for plugin developers

The following sections provide information that may be useful for developers of this 
parser plugin.

### IDE's

This project uses Lombok. In order to have your IDE compile this project without errors, 
you may need to add Lombok support to your IDE. Please see https://projectlombok.org/setup/overview 
for more information.

### Gradle

It is strongly recommended to build this project using the included Gradle Wrapper
scripts; using other Gradle versions may result in build errors and other issues.

The Gradle build uses various helper scripts from https://github.com/fortify-ps/gradle-helpers;
please refer to the documentation and comments in included scripts for more information. 

### Commonly used commands

All commands listed below use Linux/bash notation; adjust accordingly if you
are running on a different platform. All commands are to be executed from
the main project directory.

* `./gradlew tasks --all`: List all available tasks
* Build: (plugin binary will be stored in `build/libs`)
  * `./gradlew clean build`: Clean and build the project
  * `./gradlew build`: Build the project without cleaning
  * `./gradlew dist`: Build distribution zip
* Version management:
  * `./gradlew printProjectVersion`: Print the current version
  * `./gradlew startSnapshotBranch -PnextVersion=2.0`: Start a new snapshot branch for an upcoming `2.0` version
  * `./gradlew releaseSnapshot`: Merge the changes from the current branch to the master branch, and create release tag
* `./fortify-scan.sh`: Run a Fortify scan; requires Fortify SCA to be installed

Note that the version management tasks operate only on the local repository; you will need to manually
push any changes (including tags and branches) to the remote repository.

### Versioning

The various version-related Gradle tasks assume the following versioning methodology:

* The `master` branch is only used for creating tagged release versions
* A branch named `<version>-SNAPSHOT` contains the current snapshot state for the upcoming release
* Optionally, other branches can be used to develop individual features, perform bug fixes, ...
  * However, note that the Gradle build may be unable to identify a correct version number for the project
  * As such, only builds from tagged versions or from a `<version>-SNAPSHOT` branch should be published to a Maven repository

### Automated Builds & publishing

Travis-CI builds are automatically triggered when there is any change in the project repository,
for example due to pushing changes, or creating tags or branches. If applicable, binaries and related 
artifacts are automatically published to Bintray using the `bintrayUpload` task:

* Building a tagged version will result in corresponding release version artifacts to be published
* Building a branch named `<version>-SNAPSHOT` will result in corresponding beta version artifacts to be published
* No artifacts will be deployed for any other build, for example when Travis-CI builds the `master` branch

See the [Related Links](#related-links) section for the relevant Travis-CI and Bintray links.


# Licensing
See [LICENSE.TXT](LICENSE.TXT)

