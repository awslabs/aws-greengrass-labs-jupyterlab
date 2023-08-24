# | aws.greengrass.labs.jupyterlab

This component deploys JupyterLab onto a Greengrass device. The Jupyter environment has access to all the process and environment variable resources set by Greengrass and allows for a simpler testing and development of components written in Python.

## Install

Refer to [BUILD.md](./BUILD.md) for instruction on how to add the components to your AWS account.

Once you have the published the components to the account you can add `aws.greengrass.labs.jupyterlab` to a deployment targeting the device(s).

You can select the following [link](https://console.aws.amazon.com/iot/home?#/greengrass/v2/components/private/aws.greengrass.labs.jupyterlab) to verify that the component has been published correctly to the account. If you get an error message, verify first that the correct AWS region is selected.

### Accessing the Jupyter Lab console

Once deployed this component starts a Jupyter Lab server on `localhost:8888`. For security reasons, this address is not accessible from other hosts, even on the same network. In order to access the server you need to create an SSH tunnel to the device and forward the `8888` port to a local port on your machine.

`ssh -L localhost:8888:localhost:8888 <remote device>`

Using `ssh` requires a route to the device. If the device is deployed in a different network, this might not always be possible. In such case you can use `aws.greengrass.SystemManagerAgent` [component](https://docs.aws.amazon.com/greengrass/v2/developerguide/systems-manager-agent-component.html) to deploy a System Manager agent that allows remote connections from anywhere.

Follow the [instructions](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up-edge-devices.html) to configure the component. If you are deploying the agent in an [hybrid environment](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html), you need to enable [advanced-instances tier](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances-advanced.html) to be able to use the session manager feature.

Once you have performed the necessary configuration and deployed both Jupyter Lab and Sessions Manager Agent components you can run the following command from any machine with internet access and provisioned with [AWS IAM credentials](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-instance-profile.html) that allow the Session Manager connection.

```bash
export INSTANCE_ID=mi-abc
aws ssm start-session --target $INSTANCE_ID --document-name AWS-StartPortForwardingSession --parameters '{"portNumber":["8888"],"localPortNumber":["8080"]}' --region <aws-region>
```

Once the tunnel runs, you can access the Jupyter Lab console at `http://localhost:8080`.

### Additional information

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
| Token exchange service | >=0.0.0 | Hard |
| aws.greengrass.labs.libffi | 1.0.0 | Hard |

## Configuration

This component provides the following configuration parameters that you can customize when you deploy the component.

`PASSWORD_HASH`

(Optional) It is recommended to set a password to enable access to the JupyterLab interface. The password hash can be generated using the following command:

`python3 -c "from jupyter_server.auth import passwd; print(passwd())"`

If the password hash is not set, you will need to have to provide a session token to access the notebook. The session token can be obtained from the component log. When using a token, JupyterLab gives the user the possibility to set a password for further access.

`ALLOW_REMOTE`

(Optional) Set to `true` to allow remote hosts tunneling into the device to access the Jupyter Lab console. This is necessary in case you use AWS Cloud9 instance as a jump host via an System Manager SessionManager tunnel.


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


## Thank you

This component would have not been possible without the great work done by [Project Jupyter](https://jupyter.org/)
