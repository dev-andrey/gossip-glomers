# Solutions for Gossip Glomers Challenges in Scala using ZIO-Maelstrom

What is Maelstrom?

https://github.com/jepsen-io/maelstrom

Gossip Glomers Challenges?

https://fly.io/dist-sys/

ZIO-Maelstrom?

https://zio-maelstrom.bilal-fazlani.com/

---

## Contents

| Status | Challenge                                                                           | Source code                                                                                                                         |
| ------ | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| -      | [1: Echo](https://fly.io/dist-sys/1/)                                               | [/echo/](/echo/src/main/scala/gossipGlomers/)                                                                                       |
| -      | [2: Unique ID Generation](https://fly.io/dist-sys/2/)                               | [/unique-id-generation/](/unique-id-generation/src/main/scala/gossipGlomers/)                                                       |
| -      | [3a: Single-Node Broadcast](https://fly.io/dist-sys/3a/)                            | [/single-node-broadcast/](/single-node-broadcast/src/main/scala/gossipGlomers/)                                                     |
| -      | [3b: Multi-Node Broadcast](https://fly.io/dist-sys/3b/)                             | [/multi-node-broadcast/](/multi-node-broadcast/src/main/scala/gossipGlomers/)                                                       |
| -      | [3c: Fault Tolerant Broadcast](https://fly.io/dist-sys/3c/)                         | [/fault-tolerant-broadcast/](/fault-tolerant-broadcast/src/main/scala/gossipGlomers/)                                               |
| -      | [3d: Efficient Broadcast, Part I](https://fly.io/dist-sys/3d/)                      | [/efficient-broadcast-1/](/efficient-broadcast-1/src/main/scala/gossipGlomers/)                                                     |
| -      | [3e: Efficient Broadcast, Part II](https://fly.io/dist-sys/3e/)                     | [/efficient-broadcast-2/](/efficient-broadcast-2/src/main/scala/gossipGlomers/)                                                     |
| -      | [4: Grow-Only Counter](https://fly.io/dist-sys/4/)                                  | [/grow-only-counter/](/grow-only-counter/src/main/scala/gossipGlomers/)                                                             |
| -      | [5a: Single-Node Kafka-Style Log](https://fly.io/dist-sys/5a/)                      | [/kafka-style-log/](/kafka-style-log/src/main/scala/gossipGlomers/)                                                                 |
| -      | [5b: Multi-Node Kafka-Style Log](https://fly.io/dist-sys/5b/)                       | [/multi-node-kafka-style-log/](/multi-node-kafka-style-log/src/main/scala/gossipGlomers/)                                           |
| -      | [5c: Efficient Kafka-Style Log](https://fly.io/dist-sys/5c/)                        | [/efficient-kafka-style-log/](/efficient-kafka-style-log/src/main/scala/gossipGlomers/)                                             |
| -      | [6a: Single Node, Totally Available Transactions](https://fly.io/dist-sys/6a/)      | [/single-node-totally-available-transactions/](/single-node-totally-available-transactions/src/main/scala/gossipGlomers/)           |
| -      | [6b: Totally-Available, Read Uncommitted Transactions](https://fly.io/dist-sys/6b/) | [/totally-available-read-uncommitted-transactions/](/totally-available-read-uncommitted-transactions/src/main/scala/gossipGlomers/) |
| -      | [6c: Totally-Available, Read Committed Transactions](https://fly.io/dist-sys/6c/)   | [/totally-available-read-committed-transactions/](/totally-available-read-committed-transactions/src/main/scala/gossipGlomers/)     |

## Pre-requisites

1. Maelstrom should be installed and should be available in PATH
2. Coursier should be installed and should be available in PATH
3. GraalVM Community Edition with native-image tool should be installed
4. `sh` / `bash` should be available

## Getting started

This is a template repository. Use the below option to create your own repo using this template.

![image](https://github.com/bilal-fazlani/gossip-glomers-scala-template/assets/3396271/7b419e92-24d1-4984-9171-0093fe94fcb8)

## Setup

Using running jar files does not work because they start too slow and the tests timeout. 
So, we need to compile them to native binaries using graalvm

There are three custom (and useful) commands available on every solution subproject:

1. `bootstrap`

    bootstrap command uses coursier cli to build a fat jar will all dependencies. It also create a maelstrom runner script `testjar.sh` file which can be used to run the jar file via maelstrom with expected load for the challenge.

    ```bash
    #replace with the challenge you want to package
    sbt efficient-broadcast-1/bootstrap
    ```
    
    You can now run `testjar.sh` script from target directory
    
    ```bash
    ./efficient-broadcast-1/target/testjar.sh
    ```
   
2. `maelstromRunAgent`

    In order to create a native binary, we first run the java application (single node) with a very small load from maelstrom. Using command `maelstromRunAgent`.
    This runs with graalvm agent and generates reflection configuration in `resources/META-INF/native-image/`

    Reflection configurations don't change very often. So, we can reuse them for future compilations.

3. `nativePackage`

    After reflection configs are generated, we can now compile the application to a native binary using `nativePackage` command. This task also creates a `test.sh` file which can be used to run the native binary via maelstrom with expected load for the challenge.

    ```bash
    #replace with the challenge you want to compile
    sbt efficient-broadcast-1/maelstromRunAgent
     
    sbt efficient-broadcast-1/nativePackage
    ```
    
    You can now run `test.sh` script from target directory
    
    ```bash
    ./efficient-broadcast-1/target/test.sh
    ```

## Notes

- Take a look at [build.sbt](build.sbt) to get familiar with the build setup
- This is template tested on MacOS. Linux should be supported too. Windows might require minor tweaks
- You may have to change `nativeImageGraalHome` in [build.sbt](build.sbt) to point to your graalvm home directory
- Recommended to use latest Graalvm CE version
