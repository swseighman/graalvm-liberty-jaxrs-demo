## GraalVM Performance Comparison on Open Liberty

### Benchmark JAX-RS Example on Open Liberty

Clone this repository:
[https://github.com/swseighman/graalvm-liberty-jaxrs-demo.git
](https://github.com/swseighman/graalvm-liberty-jaxrs-demo.git)

_Credit to Marthen Luther, this is an Open Liberty 'port' of his original [WebLogic project](https://github.com/marthenlt/graalvm-weblogic-jaxrs-demo.git)._

`$ cd graalvm-liberty-jaxrs-demo`

**This particluar example requires JDK 8, so make certain you're using both Java 8 Hotspot and Java 8 GraalVM versions for testing.**

```
$ sdkman use java 8.0.271-oracle
$ java -version
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```

`$ mvn package`

Start the application:

`$ mvn liberty:dev`

Next, execute the `run-test.sh` script in the project directory to begin the benchmark tests. Run the script several times until the VM stablizes.

Example results using Java 8 (u271) Hotspot VM:

```bash
$ ./run-test.sh
Testing with 20,000,000 (20 million) iterations
Time taken to complete in milliseconds: 3978 ; and result is: 3000000000
real    0m3.984s
user    0m0.004s
sys     0m0.000s
```

Stop the application (CTRL-C) and change JDK 8 J9 version:

```
$ sdkman use java 8.0.275.j9-adpt
$ java -version
openjdk version "1.8.0_275"
OpenJDK Runtime Environment (build 1.8.0_275-b01)
Eclipse OpenJ9 VM (build openj9-0.23.0, JRE 1.8.0 Mac OS X amd64-64-Bit Compressed References 20201112_760 (JIT enabled, AOT enabled)
OpenJ9   - 0394ef754
OMR      - 582366ae5
JCL      - b52d2ff7ee based on jdk8u275-b01)
```

Start the application:

```
$ mvn liberty:dev
```
Run the `run-test.sh` script again.  Run the script several times until the VM stablizes.

Example results using Java 8 (J9 u275) Hotspot VM:

```bash
$ ./run-test.sh
Testing with 20,000,000 (20 million) iterations
Time taken to complete in milliseconds: 19091 ; and result is: 3000000000
real	0m19.106s
user	0m0.004s
sys	0m0.006s
```
Stop the application (CTRL-C) and change JDK 8 to the GraalVM version:

```
$ sdkman use java 20.3.0-r8-grlee
$ java -version
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM GraalVM EE 20.3.0 (build 25.271-b09-jvmci-20.3-b06, mixed mode))
```

Start the application:

```
$ mvn liberty:dev
```
Run the `run-test.sh` script again.  Run the script several times until the VM stablizes.

Example results using GraalVM 8 (20.3.0):

```bash
$ ./run-test.sh
Testing with 20,000,000 (20 million) iterations
Time taken to complete in milliseconds: 140 ; and result is: 3000000000
real	0m0.151s
user	0m0.003s
sys	0m0.005s
```
As you can see, we realized significant imrpovement in performance by simply replacing the standard Hotspot VM with GraalVM.  Imagine the performance gains possible with a native image version of the application!

