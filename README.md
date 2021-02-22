# Marvin Configurator

This is a convience utility to help you configure Marvin*. Go to the web page, and select the versions of the SDKs and Chaincodes that you specifically want. It will give you the configuration you can use to kick-off a specifc manual run. 

It is preconfigured with a starting set of versions that can be used, and it will generate the JSON to be used, along with a `curl` command you can use to trigger the build immediately.

## What can be configured?

There are a number of chaincodes, and SDKs within Marvin. Each of these uses by default, the latest main/master branch of the relavent code. However you can alter each of these to a published version of your own choosing. 

It's important to note that you can get this completely wrong; for example there are 1.4 and 2.2 Node Chaincodes in Marvin. Each chaincode will use the Contract API specific to a version of that API.  You may though configure the 2.2 Chaincode to be built unsing the 1.4 library, or _vice versa_.  This *may* work, or it *may not*.  Though it could be useful to determine the degree of compatibility between APIs.

## How can a Marvin run be configured?
Within Marvin, there is a default set of versions defined in a [JSON file](https://github.ibm.com/ibp/marvin/blob/master/version-specs.json). By supplying a matching JSON file you can override one or more of these values. You don't have to supply the whole file, just the changes that are overlayed on top. This is achieved using `jq`s multiply feature to merge the default set of versions and the provided overrides [jq reference](https://stedolan.github.io/jq/manual/#example15).

There is a specific Tekton trigger that can be used to specify this 'override json file'

## How to determine the contents?

The values in the JSON file are interpretted differently depending on the language - as each has it's own build process. For node, this is an update of the package.json structure. Java Chaincodes will have a textual substituion in the gradle files. For the SDKs, a property is passed into the maven build. 

For specific information check the the `build.sh` scripts of the appropriate sdk or chaincode.  It can be hard to determine what to enter - hence this tool. 

## How to use this configurator?

0. Go to the webpage at [https://pages.github.ibm.com/ibp/marvin-configurator/#/](https://pages.github.ibm.com/ibp/marvin-configurator/#/)
1. It is preconfigured with a starting set of versions that can be used, but you can overridge this with custom values
2. The JSON to use is generated, and is available to copy at the bottom of the screen. You can also copy the curl command, that will initiate a build immediately


## Contributing

The tool is written using vue-js, and is then published to the gh-pages of this repo. The values are of the versions are seeded from the 'populate.sh' script. They are not worked out live. The `curl` command is produced as it's not possible to send a REST request form the browser to IBM DevOps - CORS restructions. 

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

This is configured to put the output into the `docs` folder. 

### Lints and fixes files
```
npm run lint
```

### References

* If you don't have a brain the size of a planet or work for the Sirius Cybernetics Corporation.