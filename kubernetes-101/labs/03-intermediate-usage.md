# Lab 4

## Intermediate Usage

If you want to try some debug of a cotainer. Logs and exec are very useful commands.

### 1. Logs

You can tail logs from containers pretty easily. kubectl allows you to do this right from you desktop without having to hop onto the host.

First locate your container

```
kubectl -n my-namespace get po
```

make a note of the container name, then run:

```
kubectl -n my-namespace logs <container-name>
```

you can also follow the logs with:

```
kubectl -n my-namespace logs -f <container-name>

### 2. Exec

Exec lets you run a command in a container much like the ```docker exec``` command. Once again first find your conatiner name:

```
kubectl -n my-namespace get po
```

make a note of the container name, then run:

```
kubectl -n my-namespace exec -it <container-name> <command>
```

the ```-it``` flag makes the session interactive so if running bash you can debug a container from within.
