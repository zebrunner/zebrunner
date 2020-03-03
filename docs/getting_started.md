# QPS-Infra - Getting started
## Software prerequisites
* Docker requires a user with uid=1000 and gid=1000 for simple volumes sharing, etc.
  Note: to verify the current user uid/gid, execute
  ```
  id
  uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),102(netdev),999(docker
  ```
* Install docker ([Ubuntu 16.04](http://www.techrepublic.com/article/how-to-install-docker-on-ubuntu-16-04/), [Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04), [MacOS](https://pilsniak.com/how-to-install-docker-on-mac-os-using-brew/)) and [docker-composer](https://docs.docker.com/compose/install/#install-compose)


## Initial setup
* Clone [qps-infra](https://github.com/qaprosoft/qps-infra). You can also create your private repo to migrate the infrastructure easily
* Go to the qps-infra folder and launch the setup.sh script providing your hostname or ip address as an argument
```
git clone https://github.com/qaprosoft/qps-infra.git
cd qps-infra
./setup.sh myhost.domain.com
```
* Optional: update default credentials if neccessary (strongly recommended for publicly available environments)
  Note: If you changed RABBITMQ_USER and RABBITMQ_PASS, please, update them in config/definitions.json and config/logstash.conf files as well
* Optional: adjust docker-compose.yml file by removing unused services. By default, it contains:
  nginx, postgres, zafira/zafira-ui, jenkins-master, jenkins-slave, selenium hub, sonarqube, rabbitmq, elasticsearch
* Execute ./start.sh to start all containers
* Open http://myhost.domain.com to access direct links to the sub-components: Zafira, Jenkins, etc.

## Additional setup
* Create secret value for AUTH_TOKEN_SECRET and encode to Base64 format via tool for encoding. 
* Note: AUTH_TOKEN_SECRET= value is by application for encryption of sensitive settings (such as passwords, integration settings, etc) and thus should not be set to default one.
* Create secret value for CRYPTO_SALT
* Note: CRYPTO_SALT= It should be randomized alpha-numeric string.

```
cd qps-infra
nano variables.env
```
* Set new secret value for AUTH_TOKEN_SECRET
* Set new secret value for CRYTPO_SALT
* Save changes in variables.env

```
./start.sh
```


## Services start/stop/restart
* Use ./stop.sh script to stop everything
* Opional: it is recommended to remove old containers during the restart
* Use ./start.sh to start all containers
```
./stop.sh
./start.sh
```

## Env details
* After QPS-Infra startup, the following components are available. Take a look at variables.env for default credentials:
* [Jenkins](http://demo.qaprosoft.com/jenkins)
* [Selenium Grid](http://demo.qaprosoft.com/grid/console)
* [Zafira Reporting Tool](http://demo.qaprosoft.com/zafira)
* [SonarQube](http://demo.qaprosoft.com/sonarqube)
  Note: admin/qaprosoft are hardcoded sonarqube credentials, and they can be updated inside the Sonar Adminisration panel
  
# Start via Jenkins

## Update Jenkins credentials 
* Open Jenkins by the url of your env.
* Open Credentials in the left menu
* Update Username and Password for "ghprbhook-token"
* Note: Enter Password value of "Git token access" that was generated before for this user

## Register Organization
* Open Jenkins by the url of your env.
* Open Management_Jobs folder
* Click "RegisterOrganization"
* Click "Build with Parameters"
* Set "folderName" value
* Click "Build"

## Register Repository
* Open Jenkins by the url of your env.
* Open organization folder
* Click "RegisterRepository"
* Click "Build with Parameters"
* Set "repo" value 
* Click "Build"