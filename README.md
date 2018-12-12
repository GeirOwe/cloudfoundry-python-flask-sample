# Sample Flask application for Cloud Foundry
## About this application
This is a sample application to deploy Flask application for Cloud Foundry (CF).
The contents is based on repository [yuta-hono/flask-cloudfoundry-sample](https://github.com/yuta-hono/flask-cloudfoundry-sample).

## Environment to run
- Cloud Foundry (Diego) or the version which Buildpack available

## How to run
1. Make sure that you have already logged into CF with [cf_cli](http://docs.cloudfoundry.org/cf-cli/install-go-cli.html "Installing the cf Command Line Interface").
2. Run `$ cf push <yourapp> -b https://github.com/cloudfoundry/python-buildpack`
3. Go to your application URL. `http://<host>/` just shows "Hello world".

## Files

### Files to declare runtime envinronment

#### requirements.txt
Pip requirements, this automatically satisfied by CF in a staging phase.
There is only "Flask" in the file.
You can add libraries here.

#### runtime.txt
The file specifies Python version to run this application by CF.
See more details at [Python Buildpack](https://docs.cloudfoundry.org/buildpacks/python/index.html "Python Buildpack").
Here I use "python-3.7.1", which is the latest python version as of 2018/12/12.

#### manifest.yml
The file specifies about the specs of instance (memory/disk quota, etc).

#### Procfile
The file specify what commands run the application.
For more details, see at [About Procfiles](https://docs.cloudfoundry.org/buildpacks/prod-server.html#procfile).


### Web application files

#### hello.py
The simple Flask application run on CF.
Important things here are:

- App `host` requires to be set as "0.0.0.0"
  - CF routes the traffic from external in the router, and since all the component is scalable, CF can not restrict the access with source IPs 
- App `port` requires to be set with dynamic value comes from CF envinronmental value
  - Since the port is dynamic value, an application can not specify a static port number. This port number requires to be set with dynamic value also
