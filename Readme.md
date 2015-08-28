Quick start
-----------

From the top-level directory (adjacent to benchmark.jar), execute this command:

```
java -jar benchmark.jar --scratch-dir /tmp --versions with-jgit-v3.7.1,with-jgit-3abf35b-nio,with-jgit-05acf1c-io-fix --repos github-gem --commands delete-file --only-bfg
```

The BFG benchmark is written in Scala, build with SBT, and I've provided a compiled version for your convenience. The benchmark launches each BFG run in it's own process. The ouput is a bit messy, but you should be able to see how many milliseconds each run took.

Ram Disk
--------

You should set up a 2GB ram disk (eg tmpfs) and tell the BFG where it is with the `--scratch-dir` switch. The BFG defaults to using /dev/shm (a default tmpfs ram disk on Ubuntu). Each run of the BFG will start with a fresh copy of the repo, extracted to your scratch dir, replacing any previous repo.

Reference data
--------------

The BFG is fast, so can only be really be tested with a large repo. I use a snapshot of the IntelliJ Git repo (1.2GB), and so I've stored this as a separate download, available here:

https://docs.google.com/uc?id=0B8R1d5Zpes8HUWUwVzZfUUlrdUU&export=download

Once unzipped, the 'intellij' folder should live next to the 'github-gem' one, ie at:

bfg-benchmark/resources/repos/intellij

Then use the `--repos` switch to specify 'intellij':

```
java -jar benchmark.jar --scratch-dir /tmp --versions with-jgit-v3.7.1,with-jgit-3abf35b-nio,with-jgit-05acf1c-io-fix --repos intellij --commands delete-file --only-bfg
```

Example output
--------------

```
$ java -jar benchmark.jar --versions with-jgit-v3.7.1,with-jgit-3abf35b-nio,with-jgit-05acf1c-io-fix --repos intellij --commands delete-file --only-bfg
Using resources dir : /home/rtyley/bfg-bench/bfg-benchmark/resources
Extracting repo to /dev/shm/repo.git
delete-file - InvocableBFG(Java(java,1.8.0_45-internal),BFGJar(Path(/home/rtyley/bfg-bench/bfg-benchmark/resources/jars/bfg-with-jgit-v3.7.1.jar),Map())) completed in 24,928 ms.
Extracting repo to /dev/shm/repo.git
delete-file - InvocableBFG(Java(java,1.8.0_45-internal),BFGJar(Path(/home/rtyley/bfg-bench/bfg-benchmark/resources/jars/bfg-with-jgit-3abf35b-nio.jar),Map())) completed in 27,593 ms.
Extracting repo to /dev/shm/repo.git
delete-file - InvocableBFG(Java(java,1.8.0_45-internal),BFGJar(Path(/home/rtyley/bfg-bench/bfg-benchmark/resources/jars/bfg-with-jgit-05acf1c-io-fix.jar),Map())) completed in 25,001 ms.
```

BFG versions
------------

BFG jars for test go under the [`bfg-benchmark/resources/jars`](https://github.com/rtyley/bfg-bench/tree/master/bfg-benchmark/resources/jars) folder, and can be specified with the `--versions` flag (remove the `bfg-` prefix and `.jar` suffix when you specify them).

Java versions
-------------

You can specify different Java versions with the `--java` switch. Otherwise the benchmark will just use `java` when launching the BFG jar.

```
--java /usr/lib/jvm/java-7-oracle/bin/java,/usr/lib/jvm/java-8-oracle/bin/java
```
