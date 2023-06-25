---
title: Docker TTY
mathjax: true
date: 2023-06-25 00:39:18

---

# What is "tty" in Docker

Many `docker-compose.yml` files will contain a line `tty:true`. A lot of engineers only copy this line when they create a new one but few of them actually know what it is for. Why do we need to write it down in our service? Do we really need it? In this post, I'll explain why we use it and how can we use it.

<!-- more -->

# What is tty?

First thing first. `tty` is a linux command which will retrun the file name of the terminal. I got the following in my terminal:

```bash
$ tty
/dev/ttys000
```

Remember that linux operating system considers everything as a file, like the output devices that we attach are also represented as a file. So the terminal is also a file, that's why we can have a file name for it.

# Why do we use it in docker?

The main reason we use `tty:true` in compose file is to **keep the container running**. When we bring up a container, it will quit automatically if we don't pass a command to keep it running. Consequently, we use `tty:true` to keep it running. Here I will use two examples to show you how it works. I will bring up a ubuntu service in detach mode with `docker-compose up -d` without passing any command to it. Then I use `docker-compose ps` to inspect the state of the container.

## without `tty: true`

`docker-compose.yml`:

```yaml
version: '3'
services:
  test-local:
    image: ubuntu
```

```bash
$ docker-compose up -d
$ docker-compose ps
             Name                 Command    State    Ports
-----------------------------------------------------------
docker-playground_test-local_1   /bin/bash   Exit 0        
```

## with `tty:  true`

`docker-compose.yml`:

```yaml
version: '3'
services:
  test-local:
    image: ubuntu
    tty: true
```

```bash
$ docker-compose up -d
$ docker-compose ps
             Name                 Command    State   Ports
----------------------------------------------------------
docker-playground_test-local_1   /bin/bash   Up           
```

## Analysis

Compare the output and you can see how it works. With `tty: true`, the container's state is Up.

# Conclusion

In a word, `tty: true` is docker compose file help us keep the container running. However we can also do it in different ways, like pass commands like: `sleep infity` or `tail -f /dev/null`. But at least as an engineer, it's helpful to know what it means when we write down the code!