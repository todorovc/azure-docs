---
title: Debug your Azure Service Fabric Application in Eclipse| Microsoft Docs
description: Improve the reliability and performance of your services by developing and debugging them in Eclipse on a local development cluster.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''

ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: vturecek;mikhegn

---
# Debug your Java Service Fabric application using Eclipse
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Start a local development cluster by following the steps in [Setting up your Service Fabric development environment](service-fabric-get-started-linux.md).

2. Update entryPoint.sh of the service you wish to debug, so that it starts the java process with remote debug parameters. This file can be found at the following location: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Port 8001 is set for debugging in this example.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Update the Application Manifest by setting the instance count or the replica count for the service that is being debugged to 1. This setting avoids conflicts for the port that is used for debugging. For example, for stateless services, set ``InstanceCount="1"`` and for stateful services set the target and min replica set sizes to 1 as follows: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. Deploy the application.

5. In the Eclipse IDE, select **Run -> Debug Configurations -> Remote Java Application and input connection properties** and set the properties as follows:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Set breakpoints at desired points and debug the application.

If the application is crashing, you may also want to enable coredumps. Execute ``ulimit -c`` in a shell and if it returns 0, then coredumps are not enabled. To enable unlimited coredumps, execute the following command: ``ulimit -c unlimited``. You can also verify the status using the command ``ulimit -a``.  If you wanted to update the coredump generation path, execute ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### Next steps

* [Collect logs using Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).
* [Monitor and diagnose services locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
