---
layout: post
title: PostgreSQL With JIT
---

In recent a couple of years, you have barely heard about any significant speed improvements besides remarkable features in the database domain. This year has already brought something new.

Apparently, inspired by unveiled [research](https://www.pgcon.org/2017/schedule/attachments/467_PGCon%202017-05-26%2015-00%20ISPRAS%20Dynamic%20Compilation%20of%20SQL%20Queries%20in%20PostgreSQL%20Using%20LLVM%20JIT.pdf), PostgreSQL team has implemented JIT compilation [to optimize the execution code and other operations at run time]( https://www.postgresql.org/about/news/1855/). According to the results of the TPC-H Benchmark in the aforementioned research, the overall speed improvement can reach 35%.

I believe this will lead to renewed interest in searching ways of database speed improvements besides widespread distribution of database over multiple computers.

Moreover, I have made the Docker image that contains PostgreSQL 11 Beta 2 that is with JIT in order to facilitate a try of this new feature:

`docker run --rm -p 5432:5432 --name postgresql wapxmas/postgresql-jit:11beta2`

`pg user/password: docker/docker`

[Here Dockerfile](https://github.com/wapxmas/DockerImages/blob/master/PostgreSQL-JIT/Dockerfile), so that you can build an image by yourself.
