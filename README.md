# IBM Node.js Buildpack with Amalgam8 Sidecar

A Cloud Foundry [buildpack](http://docs.cloudfoundry.org/buildpacks/) for Node based apps using [Amalgam8 Sidecar](https://www.amalgam8.io/docs/sidecar.html). This is based on the [Cloud Foundry buildpack for Node.js] (https://github.com/cloudfoundry/nodejs-buildpack).

Additional documentation can be found at the [CloudFoundry.org](http://docs.cloudfoundry.org/buildpacks/node/index.html).

Official buildpack documentation can be found at http://docs.cloudfoundry.org/buildpacks/node/index.html).

### Amalgam8 Sidecar Support
This buildpack provides Amalgam8 Sidecar support for Node.js applications.

Your application must provide the Cloud Foundry application with the `A8_CONFIG` environment variable.  The `A8_CONFIG` value should be the path to a a8sidecar YAML configuration file.

The a8sidcar YAML configuration file, must include the `commands` property, as follows:

```yaml
commands:
  - cmd: [ "npm", "start" ]
    on_exit: terminate

```

And the Node application's `package.json` must define the startup scripts, as follows:

```json
{
    "name": "helloworld",
    "version": "0.0.0",
    "dependencies": {
        "express": "~4.x",
    },
    "main": "index.js",
    "scripts": {
        "start": "node index.js"
    }
}
```

### Building the Buildpack

1. Make sure you have fetched submodules

  ```bash
  git submodule update --init
  ```

1. Get latest buildpack dependencies

  ```shell
  BUNDLE_GEMFILE=cf.Gemfile bundle
  ```

1. Build the buildpack

  ```shell
  BUNDLE_GEMFILE=cf.Gemfile bundle exec buildpack-packager [ --cached | --uncached ]
  ```

1. Use in Cloud Foundry

  Upload the buildpack to your Cloud Foundry and optionally specify it by name

  ```bash
  cf create-buildpack custom_node_buildpack node_buildpack-offline-custom.zip 1
  cf push my_app -b custom_node_buildpack
  ```

### Testing
Buildpacks use the [Machete](https://github.com/cloudfoundry/machete) framework for running integration tests.

To test a buildpack, run the following command from the buildpack's directory:

```
BUNDLE_GEMFILE=cf.Gemfile bundle exec buildpack-build
```

More options can be found on Machete's [GitHub page.](https://github.com/cloudfoundry/machete)


### Reporting Issues
Open an issue on this project
