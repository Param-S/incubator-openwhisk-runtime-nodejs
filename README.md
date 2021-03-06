# Apache OpenWhisk runtimes for nodejs
[![Build Status](https://travis-ci.org/apache/incubator-openwhisk-runtime-nodejs.svg?branch=master)](https://travis-ci.org/apache/incubator-openwhisk-runtime-nodejs)


### Give it a try today
To use as a docker action for Node.js 6
```
wsk action update myAction myAction.js --docker openwhisk/nodejs6action
```
To use as a docker action for Node.js 8
```
wsk action update myAction myAction.js --docker openwhisk/action-nodejs-v8
```
This works on any deployment of Apache OpenWhisk

### To use on deployment that contains the rutime as a kind
To use as a kind action using Node.js 6
```
wsk action update myAction myAction.js --kind nodejs:6
```
To use as a kind action using Node.js 8
```
wsk action update myAction myAction.js --kind nodejs:8
```

### Local development
For Node.js 6
```
./gradlew core:nodejs6Action:distDocker
```
This will produce the image `whisk/nodejs6action`

For Node.js 8
```
./gradlew core:nodejs8Action:distDocker
```
This will produce the image `whisk/action-nodejs-v8`


Build and Push image for Node.js 6
```
docker login
./gradlew core:nodejs6Action:distDocker -PdockerImagePrefix=$prefix-user -PdockerRegistry=docker.io 
```

Build and Push image for Node.js 8
```
docker login
./gradlew core:nodejs8Action:distDocker -PdockerImagePrefix=$prefix-user -PdockerRegistry=docker.io 
```
Then create the action using your image from dockerhub
```
wsk action update myAction myAction.js --docker $user_prefix/nodejs6action
```
The `$user_prefix` is usually your dockerhub user id.

Deploy OpenWhisk using ansible environment that contains the kind `nodejs:6` and `nodejs:8`
Assuming you have OpenWhisk already deployed locally and `OPENWHISK_HOME` pointing to root directory of OpenWhisk core repository.

Set `ROOTDIR` to the root directory of this repository.

Redeploy OpenWhisk
```
cd $OPENWHISK_HOME/ansible
ANSIBLE_CMD="ansible-playbook -i ${ROOTDIR}/ansible/environments/local"
$ANSIBLE_CMD setup.yml
$ANSIBLE_CMD couchdb.yml
$ANSIBLE_CMD initdb.yml
$ANSIBLE_CMD wipe.yml
$ANSIBLE_CMD openwhisk.yml
```

Or you can use `wskdev` and create a soft link to the target ansible environment, for example:
```
ln -s ${ROOTDIR}/ansible/environments/local ${OPENWHISK_HOME}/ansible/environments/local-nodejs
wskdev fresh -t local-nodejs
```

# License
[Apache 2.0](LICENSE.txt)


