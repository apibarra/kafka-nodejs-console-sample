# IBM Message Hub Kafka Node.js console sample application
This Node.js console application demonstrates how to connect to [IBM Message Hub](https://console.ng.bluemix.net/docs/services/MessageHub/index.html), send and receive messages using the [node-rdkafka](https://github.com/Blizzard/node-rdkafka) module. It also shows how to create and list topics using the Message Hub Admin REST API.

It can be run locally on your machine or deployed into [IBM Bluemix](https://console.ng.bluemix.net/).

__Important Note__: This sample creates on your behalf a topic named `kafka-nodejs-console-sample-topic` with one partition - this will incur a fee if the topic does not already exist on your account.

## Global Prerequisites
To build and run the sample, you must have the following installed:
* [git](https://git-scm.com/)
* [Message Hub Service Instance](https://console.ng.bluemix.net/catalog/services/message-hub/) provisioned in [IBM Bluemix](https://console.ng.bluemix.net/)

## Prerequisites (Local - macOS and Linux only)
* [Node.js](https://nodejs.org/en/) 6.X LTS
* [node-gyp] (https://www.npmjs.com/package/node-gyp)

Node-rdkafka will build librdkafka automatically. You must ensure you have the dependencies listed below installed. For more details, see [librdakfka's instructions](../docs/librdkafka.md).

##### Linux
* openssl-dev
* libsasl2-dev
* libsasl2-modules
* C++ toolchain

##### macOS 
* [Brew](http://brew.sh/)
* [Apple Xcode command line tools](https://developer.apple.com/xcode/)
* `openssl` via Brew
* Export `CPPFLAGS=-I/usr/local/opt/openssl/include` and `LDFLAGS=-L/usr/local/opt/openssl/lib`
* Validate with echo $CPPFLAGS   echo LDFLAGS
* Open Keychain Access, export all certificates in System Roots to a single .pem file

## Prerequisites (Bluemix)
* [Cloud Foundry Command Line Interface](https://github.com/cloudfoundry/cli/releases) installed

## Installing dependencies (Local)
Run the following commands on your local machine, after the prerequisites for your environment have been completed:
```shell
npm install
```

## Running the Sample (Local - macOS and Linux only)
Once built, to run the sample, execute the following command:
```shell
node app.js <kafka_brokers_sasl> <kafka_admin_url> <api_key> <ca_location>
```

```
Paula Environment:
node app.js kafka03-prod02.messagehub.services.us-south.bluemix.net:9093 https://kafka-admin-prod02.messagehub.services.us-south.bluemix.net:443 tvRS4mqxJwt0QTqKhjR2CzNMt3dwjksCrNmEvujf425B862H /Users/anapaulajimenezibarra/Documents/Certificados.pem
Update the value: /Users/anapaulajimenezibarra/Documents/Certificados.pem
```

To find the values for `<kafka_brokers_sasl>`, `<kafka_admin_url>` and `<api_key>`, access your Message Hub instance in Bluemix, go to the `Service Credentials` tab and select the `Credentials` you want to use.

`<ca_location>` is the path where the trusted SSL certificates are stored on your machine and is therefore system dependent. 
For example:
* Ubuntu: /etc/ssl/certs
* RedHat: /etc/pki/tls/cert.pem
* macOS: The .pem file you created in the prerequisite section

__Note__: `<kafka_brokers_sasl>` must be a single string enclosed in quotes. For example: `"host1:port1,host2:port2"`. We recommend using all the Kafka hosts listed in the `Credentials` you selected.

Alternatively, you can run only the producer or only the consumer by respectively appending the switches `-producer` or `-consumer`  to the command above.

The sample will run indefinitely until interrupted. To stop the process, use `Ctrl+C`, for example.

## Running the Sample (Bluemix)

Open the `manifest.yml` file and rename the `"Message Hub-CHANGEME"` entry to that of your own Message Hub Service Instance name.

Connect to Bluemix with the Cloud Foundry Command Line Interface, then run the following command in the same directory as the `manifest.yml` file:
```shell
cf push
```

## Sample Output
Below is a snippet of the output generated by the sample:

```
Topic mh-nodejs-console-sample-topic created
Existing topics:
[ { name: 'mh-nodejs-console-sample-topic',
    partitions: 1,
    retentionMs: '86400000',
    markedForDeletion: false } ]
The consumer has started
The producer has started
Topic object created with opts {"request.required.acks":-1}
Consumer obtained metadata: {"orig_broker_id":0,"orig_broker_name":"sasl_ssl://kafka01-prod01.messagehub.services.us-south.bluemix.net:9093/0","topics":[{"name":"mh-nodejs-console-sample-topic","partitions":[{"id":0,"leader":0,"replicas":[0,1,4],"isrs":[null,null,null,1]}]}],"brokers":[{"id":2,"host":"kafka03-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":4,"host":"kafka05-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":1,"host":"kafka02-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":3,"host":"kafka04-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":0,"host":"kafka01-prod01.messagehub.services.us-south.bluemix.net","port":9093}]}
Producer obtained metadata: {"orig_broker_id":0,"orig_broker_name":"sasl_ssl://kafka01-prod01.messagehub.services.us-south.bluemix.net:9093/0","topics":[{"name":"mh-nodejs-console-sample-topic","partitions":[{"id":0,"leader":0,"replicas":[0,1,4],"isrs":[null,null,null,1]}]}],"brokers":[{"id":2,"host":"kafka03-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":4,"host":"kafka05-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":1,"host":"kafka02-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":3,"host":"kafka04-prod01.messagehub.services.us-south.bluemix.net","port":9093},{"id":0,"host":"kafka01-prod01.messagehub.services.us-south.bluemix.net","port":9093}]}
Message produced, offset: 1
No messages consumed
Message produced, offset: 2
Message consumed: topic=mh-nodejs-console-sample-topic, partition=0, offset=1, key=key, value=This is a test message #1
Message produced, offset: 3
Message consumed: topic=mh-nodejs-console-sample-topic, partition=0, offset=2, key=key, value=This is a test message #2
Message produced, offset: 4
Message consumed: topic=mh-nodejs-console-sample-topic, partition=0, offset=3, key=key, value=This is a test message #3
```
