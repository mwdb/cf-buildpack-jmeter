# cf-buildpack-jmeter

A Build pack that allows calling a jmeter script from a cf environment

* This buildpack accepts a jmeter *.jmx file and other related files as the pushed artifacts.
* The health-check-type should be set to none
* The --no-route flage should be set
* The script to be run must be supplied in an enviroment varaible named _LOAD_SCRIPT_
* If the script can be used for multiple deployments with differing URLS) this can be set with the optional _TEST_URL_ environment variable

**NOTES** 

* jmeter process terminates after run will be determined to be application crashing, and attempts to restart will be made.  Multiple script completions in a short time will result in the application being crashed.
* jmeter stores results in memory and local files, this will eventually cause the container to crash and be restarted

**Sample Manifest**

```
applications:
- name: jmeter-cli
  instances: 1
  memory: 1024M
  disk_quota: 1024M
  no-route: true
  health-check-type: none
  buildpack: https://github.com/icejumper/cf-buildpack-jmeter.git
  stack: cflinuxfs3
  env:
    LOAD_SCRIPT: 01_masterdata-service-supreme-scenario.jmx
    TEST_URL: sap.com
    
```
