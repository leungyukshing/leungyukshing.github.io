---
title: Github Updates RSA Key
mathjax: true
date: 2023-05-15 20:38:23

---

# Introduction

Hey, it's been a long time since my last post. It's been a difficult time for me in the past few months and I'm very happy to return to update my blog! Let's start with a news about Github!

<!-- more -->

# What's the problem?

Today when I try to push my code to my github, I got an error:

```
Host key for github.com has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.
```

Yes I havn't pushed my code to Github since January. And I searched about this error. I found a post from Github Blog talking about this issue. It's becasue the public key of Github, which is stored in my laptop is expired. So I am unable to push my code because it failed for the authentication.
It's very unusual because website seldom change their RSA key but it does happen.

---

# Why does Github update RSA key?

Github says it's becuase their previous RSA private key is exposed. As we all know that the private key and the public key is a pair. Without a secured private key, the communication between Github and our laptop will be no longer safe. There could be man-in-the-middle attack. So Github need to change to use a new pair of RSA key and that's why we also need to use the new public key.

---

# How to solve it?

---

It's pretty easy!

+ Remove the old one

  ```bash
  $ ssh-keygen -R github.com
  ```

  This command may create a new file named `known_hosts.old` to store the original content.

+ Then add the new one to `known_hosts`

  ```bash
  github.com ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCj7ndNxQowgcQnjshcLrqPEiiphnt+VTTvDP6mHBL9j1aNUkY4Ue1gvwnGLVlOhGeYrnZaMgRK6+PKCUXaDbC7qtbW8gIkhL7aGCsOr/C56SJMy/BCZfxd1nWzAOxSDPgVsmerOBYfNqltV9/hWCqBywINIR+5dIg6JTJ72pcEpEjcYgXkE2YEFXV1JHnsKgbLWNlhScqb2UmyRkQyytRLtL+38TGxkxCflmO+5Z8CSSNY7GidjMIZ7Q4zMjA2n1nGrlTDkzwDCsw+wqFPGQA179cnfGWOWRVruj16z6XyvxvjJwbz0wQZ75XK5tKSb7FNyeIEs4TT4jk+S4dhPeAUC5y+bDYirYgM4GC7uEnztnZyaVWQ7B381AK4Qdrwt51ZqExKbQpTUNn+EjqoTwvqNj4kqx5QUCI0ThS/YkOxJCXmPUWZbhjpCg56i+2aB6CmK2JGhn57K5mj0MNdBXA4/WnwH6XoPWJzK5Nyu2zB3nAZp+S5hpQs+p1vN1/wsjk=
  ```

---

# Reference

1. [Github Blog](https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/)