Docker EntryPoint vs Command

A Container lives till the command is executed

dockerfile - ubuntu-sleep
```
FROM ubuntu
CMD ["sleep", "5"]
```
`docker run ubuntu-sleep`-> sleeps for 5 sec

`docker run ubuntu-sleep sleep 10` -> overrrides the cmd to `sleep 10`

With entrypoint
Can give the arguments in run time
```
FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]    ## default value
```

`docker run ubuntu-sleep` -> sleeps for 5 sec

`docker run ubuntu-sleep 10` -> sleeps for 10 sec

in pod.yaml, override the default value of command (argument for entrypoint)
```
containers:
  - image: ubuntu-sleep
    name: ubuntu-sleep
    args: ["10"]
```

To override ENTRYPOINT command
`docker run --entrypoint <cmd> ubuntu-sleep <arg>`

in pod.yaml
```
containers:
  - image: ubuntu-sleep
    name: ubuntu-sleep
    command: ["sleep2.0"]  ## override ENTRYPOINT
    args: ["10"]           ## override CMD (arg)
```
---
