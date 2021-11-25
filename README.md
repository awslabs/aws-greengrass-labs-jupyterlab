# | aws.greengrass.labs.jupyterlab

This component deploys JupyterLab onto a Greengrass device. The Jupyter environment has access to all the process and environment variable resources set by Greengrass and allows for a simpler testing and development of components written in Python.

You need to upload both `community.greengrass.jupyterlab` and `community.greengrass.libffi` to your account, but you need to include only `community.greengrass.jupyterlab` in your deployments.

For more information on JupyterLab see [JupyterLab Documentation](https://jupyterlab.readthedocs.io/en/stable/).

To allow this component to use Greengrass IPC you need to add an `accessControl` section in the component configuration as explained in [Authorize components to perform IPC operations](https://docs.aws.amazon.com/greengrass/v2/developerguide/interprocess-communication.html#ipc-authorization-policies).

To use other components, such as [Stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html) or any of the [Machine learning components](https://docs.aws.amazon.com/greengrass/v2/developerguide/machine-learning-components.html) you can add them to the deployment. 


## Versions
This component has the following versions:

* 1.0.x

## Type

This component is a generic component. The [Greengrass nucleus](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-nucleus-component.html) runs the component's lifecycle scripts.

For more information, see [component types](https://docs.aws.amazon.com/greengrass/v2/developerguide/manage-components.html#component-types)


## Requirements

This component requires Python 3 and python-venv to be available on the device. 

This component allows the use to write custom code at runtime and additional requirements in term of ports used or permissions cannot be pre-defined.

## Dependencies

When you deploy a component, AWS IoT Greengrass also deploys compatible versions of its dependencies. This means that you must meet the requirements for the component and all of its dependencies to successfully deploy the component. This section lists the dependencies for the released versions of this component and the semantic version constraints that define the component versions for each dependency. You can also view the dependencies for each version of the component in the [AWS IoT Greengrass console](https://console.aws.amazon.com/greengrass). On the component details page, look for the Dependencies list.

### 1.0.0

| Dependency | Compatible versions | Dependency type |
|---|---|---|
| Greengrass nucleus | >=2.0.0 <2.5.0 | Soft |
| Token exchange service | >=0.0.0 | Hard |
| aws.greengrass.labs.libffi | 1.0.0 | Hard |

## Configuration

This component provides the following configuration parameters that you can customize when you deploy the component.

`PASSWORD_HASH`

(Optional) It is recommended to set a password to enable access to the JupyterLab interface. The password hash can be generated using the following command:

`python3 -c "from jupyter_server.auth import passwd; print(passwd())"`

If the password hash is not set, you will need to have to provide a session token to access the notebook. The session token can be obtained from the component log. When using a token, JupyterLab gives the user the possibility to set a password for further access.

## Local log file

This component uses the following log file:

```bash
/greengrass/v2/logs/aws.greengrass.labs.jupyterlab.log
```


## Changelog

The following table describes the changes in each version of the component.

| Version | Changes |
|---|---|
| 1.0.0 | Initial version |
