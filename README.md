## Tool that helps more easier run your apps in AWS Device Farm
[![Go Report Card](https://goreportcard.com/badge/github.com/artemnikitin/devicefarm-ci-tool)](https://goreportcard.com/report/github.com/artemnikitin/devicefarm-ci-tool)  [![codebeat badge](https://codebeat.co/badges/681739e3-a860-404c-a99d-3b02a98606fb)](https://codebeat.co/projects/github-com-artemnikitin-devicefarm-ci-tool)  [![Build Status](https://travis-ci.org/artemnikitin/devicefarm-ci-tool.svg?branch=master)](https://travis-ci.org/artemnikitin/devicefarm-ci-tool)     
#### Description
Did you try to run an app in AWS Device Farm via CLI or API? It was easy, right? Right now you can probably say ARN of your project after wake up in the middle of night.    
This tool helps to run apps in AWS Device Farm easier. You don't need to know ARN, because it's for machine, not for humans.

It's not a replacement for existing AWS CLI tools. It was created for a very specific purpose, to run tests in CI without a lot of configuration that required for existing solutions. It based on an assumption that all setup is already done. It means, that if you will specify an unexisted project name, then the tool wouldn't create it.   

#### AWS Credentials

Set environment variables     
```
export AWS_ACCESS_KEY_ID=<key>    
export AWS_SECRET_ACCESS_KEY=<secret>
```     

#### Run it
Get it via    
``` 
go get github.com/artemnikitin/devicefarm-ci-tool 
``` 

Or you can just download binaries for Windows, Mac OSX or Linux from [latest release](https://github.com/artemnikitin/devicefarm-ci-tool/releases)   
   
Required launch parameters:   
```
devicefarm-ci-tool -project=name -app=/path/to/my/app.apk
```
By default, "BUILTIN_FUZZ" tests will be run for your app.

#### Optional parameters:   
- ```region``` set S3 region, by default region will be set to ```us-west-2```(At this moment, will be set to ```us-west-2``` in any case, because it's only supported region for the moment).          
Example:    
``` 
devicefarm-ci-tool -project=name -app=/path/to/my/app.apk -region=region-name 
```    
- ```devices``` specify name of device pool where app will be run.      
Example:   
``` 
devicefarm-ci-tool -project=name -app=/path/to/my/app.apk -devices=my-device-pool
```   
- ```config``` specify path to config in JSON format.      
Example:   
``` 
devicefarm-ci-tool -project=name -app=/path/to/my/app.apk -config=/path/to/config.json
```   
- ```wait``` will wait for end of run. Disabled by default. Useful for CI.     
Example:   
``` 
devicefarm-ci-tool -project=name -app=/path/to/my/app.apk -wait=true
```   

You can specify parameter ```-log``` for logging AWS requests and responses.

#### Configuration file description
All parameters in the configuration file is optional. For reference you can look at http://docs.aws.amazon.com/devicefarm/latest/APIReference/API_ScheduleRun.html  

Example of config:
```json
{
   "runName":"name",
   "test":{
      "type":"BUILTIN_FUZZ|BUILTIN_EXPLORER|APPIUM_JAVA_JUNIT|APPIUM_JAVA_TESTNG|APPIUM_PYTHON|APPIUM_WEB_JAVA_JUNIT|APPIUM_WEB_JAVA_TESTNG|APPIUM_WEB_PYTHON|CALABASH|INSTRUMENTATION|UIAUTOMATION|UIAUTOMATOR|XCTEST",
      "testPackageArn":"string",
      "testPackagePath":"string",
      "filter":"string",
      "parameters":{
         "key":"value",
         "key":"value"
      }
   },
   "additionalData":{
      "extraDataPackageArn":"string",
      "extraDataPackagePath":"string",
      "networkProfileArn":"string",
      "locale":"string",
      "location":{
         "latitude":1.222,
         "longitude":1.222
      },
      "radios":{
         "wifi":"true|false",
         "bluetooth":"true|false",
         "nfc":"true|false",
         "gps":"true|false"
      },
      "auxiliaryApps":[
         "string1",
         "string2"
      ],
      "billingMethod":"METERED|UNMETERED"
   }
}
```
