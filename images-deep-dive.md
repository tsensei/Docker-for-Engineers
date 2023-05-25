# Images : Deep Dive

Images are made up of file system changes and metadata of them.

Images are designed using the union file system (ufs) concept of making layers about the changes.

Each layer is uniquely identified and stored only once on host.

When we start a container based on a image, Docker adds a read/write layer for that container on top of the base image.&#x20;

#### Naming and Tagging Images

```bash
docker build -t <name>:<tag> .
```
