# CI / CD task for Vananam 



### Prerequisites
- Docker installed
- Docker Compose installed
- Jenkins installed and running

## Set Up Jenkins
-  Open a browser and go to http://localhost:{jenkins port}
-  install Required Jenkins Plugins
    - Docker Pipeline
    - GitHub Integration
    - Pipeline (if not included in suggested plugins)

## Create and Configure Jenkins Pipeline

- Go to Jenkins Dashboard -> New Item -> Pipeline -> OK.
- Go to the pipeline job configuration.
- Under the "Build Triggers" section, check "Poll SCM."
- Set the polling schedule( ``` * * * * * ```).
- set Pipeline Defination ``` Pipeline script from SCM ``` -> Git -> Repositories -> add Git Repo URL -> provide credentials
- Save the Configuration


## Deploy and Verify
Trigger a build in Jenkins by pushing changes to the repository or manually triggering a build.

##  Rollback
- The pipeline includes a rollback stage that triggers on failure.
- Ensure previous successful versions are tagged properly for rollback.


