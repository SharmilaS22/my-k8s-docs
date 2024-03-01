## Container Network Interface

The set of steps carried out by the docker (or container runtimes) while creating and managing network between containers - are the same 
- Like creating network namespaces, veth, bridge, assigning ip address, port-forwarding etc

For executing all these steps, could use a plugin. There are so many plugins available. CNI maintain a standard between container runtimes and plugins to work with each other.

Exception: Docker uses Container Network Model (CNM) - and does not directly support CNI plugins - but can execute the steps manually using plugins.
- eg, add a container to a netns
    `bridge add <cont-id> <netns>`

