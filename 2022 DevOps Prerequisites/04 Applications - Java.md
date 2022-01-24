
## <span id="Applications">Applications</span>

[[☝️top]](#top)

- Python
- NodeJS
- Java

- Building and Deployment
- Troubleshooting Applications

<br>

- `Python` > intermediary `byte code` > `machine code`

<br>

### Packages / modules / libraries

- `Filesystems`
- `Math`
- `Operating system`
- `HTTP`
- `Security`
- `Networking`

<br>

### Build

- process: `develope` > `compile` > `package` > `document`
  - `...` > `javac Class.java` > `jar cf Class.jar` > `javadoc Class.java`

- Procedure involves...

  - Compile
  - Run tests
  - Vulnerability tests
  - Code signing
  - Package
  - Delivery

- Build Tools

<br>

### Java

**Setup**

1. Manually download the `tar` file at https://jdk.java.net/17/.
2. [Terminal]> `tar -xvf openjdk-17.0.2_linux-aarch64_bin.tar.gz`

<br>

**JDK (Java Development Kit)**

- develop
  - `jdb`: java debugger
  - `jdvadoc`: java source code documentation tool

- build
  - `javac`: build and compile
  - `jar`: archive the developed and dependencies into a single `jar` file. 

- run
  - `JRE`: Java Runtime Environment. Needed to run a java programme in ANY given system.  
  - `java`: java command line utility (loader)

<br>

**Instal Java**

1. Manually click-download `tar` file at https://openjdk.java.net/install/
2. [Terminal] > `tar -xvf openjdk-17*_bin.tar.gz`

<br>

**Problem**

- Install java 17 inside `/opt` on `app_server1` server.

Solution:
1. `ssh app_server1`: you need to change account; hence, `ssh username`.
2. `sudo curl https://download.java.net/.../openjdk-17_linux-x64_bin.tar.gz --output /opt/openjdk-17_linux-x64_bin.tar.gz`: download the file.

        Warning: Binary output can mess up your terminal. Use "--output -" to tell 
        Warning: curl to output it to your terminal anyway, or consider "--output 
        Warning: <FILE>" to save to a file.

3. `sudo tar -xf /opt/openjdk-17_linux-x64_bin.tar.gz -C /opt/`: uncompress the file. 
4. `/opt/jdk-17/bin/java -version`: Check version. 
5. `export PATH=$PATH:/opt/jdk-13.0.2/bin`: set java binary path in environment PATH variable to use java binaries wihtout using the full path to run it.

<br>

### Compiling in `java`


        > WeatherApp.class     WeatherApp.java

<br>

**Compare `source code` and `byte code`**

Source code

        public class WeatherApp {
        public static void main(String[] args) {
              System.out.println("Today's Weather");
            }
        }

Byte code

        java/lang/Object<init>()V


        java/lang/SystemoutLjava/io/PrintStreamToday's Weather

        java/io/PrintStreamprintln(Ljava/lang/String;)WeatherAppCodeLineNumberTablemain([Ljava/lang/String;)V
        SourceFile
              WeatherApp.java!*  


<br>

### Packaging in `java`

`JAR`: Java Archive. Packages up multiple files and required dependencies into a single "**distributable**" package. 
`Main-Class`property in `manifest.mf`: specifies the entry point for the distributed package (`Forecast` in this case).

      jar cf WeatherApp.jar Weather.class Forecast.class WeatherStation.class ...
      > WeatherApp.jar

      > META-INF/MANIFEST.MF
            Manifest-Version: 1.0
            Created-By: 1.8.0_242 (Private Build)
            Main-Class: Weather

<br>

If you are running a jar file, you can run it like this: 

      java -jar WeatherApp.jar
      >> programme runs...

<br>

**Java Build Tools**

Build tools help automate the build process (`compile` > `package` > `document`). 

- `Maven`
- `Gradle`
- `ANT`

Let's install build automation tools: `Ant` & `Maven`

      sudo install -y ant
      sudo install -y maven

Examine the `build.xml` file: `cat build.xml`

      <?xml version="1.0"?>
      <project name="Ant" default="main" basedir=".">
      <!-- Compiles the java code (including the usage of library for JUnit -->
      <target name="compile">
            <javac srcdir="/opt/ant/src" destdir="/opt/ant/build">
            </javac>
      </target>
      <!-- Creates Javadoc -->
      <target name="docs" depends="compile">
            <javadoc packagenames="src" sourcepath="/opt/ant/src" destdir="/opt/ant/docs">
                  <!-- Define which files / directory should get included, we include all -->
                  <fileset dir="/opt/ant/src">
                  <include name="**" />
                  </fileset>
            </javadoc>
      </target>
      <!--Creates the deployable jar file  -->
      <target name="jar" depends="compile">
            <jar basedir="/opt/ant/build" destfile="/opt/ant/dist/WeatherApp.jar" >
                  <manifest>
                  <attribute name="Main-Class" value="WeatherApp" />
                  </manifest>
            </jar>
      </target>
      <target name="run" depends="jar">
            <java jar="/opt/ant/dist/WeatherApp.jar" fork="true" />
      </target>
      <target name="main" depends="compile, jar, docs, run">
            <description>Main target</description>
      </target>
      </project>

<br>

Now compile your app using `ant`.

      ant compile jar

Now runt `ant`.

      ant

<br>

      Buildfile: /opt/ant/build.xml

      compile:
      [javac] /opt/ant/build.xml:5: warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds

      jar:

      docs:
      [javadoc] Generating Javadoc
      [javadoc] Javadoc execution
      [javadoc] Loading source file /opt/ant/src/WeatherApp.java...
      [javadoc] Constructing Javadoc information...
      [javadoc] Standard Doclet version 1.8.0_312
      [javadoc] Building tree for all the packages and classes...
      [javadoc] Building index for all the packages and classes...
      [javadoc] Building index for all classes...

      run:
      [java] Today's Weather...

      main:

      BUILD SUCCESSFUL
      Total time: 1 second

<br>

      <?xml version="1.0" encoding="UTF-8"?>

      <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
            <modelVersion>4.0.0</modelVersion>

            <groupId>org.WeatherFoundation.Weatherapp</groupId>
            <artifactId>WeatherApp</artifactId>
            <version>1.0-SNAPSHOT</version>ForestFoundation
            <url>http://www.ForestFoundation.org</url>

            <properties>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            <maven.compiler.source>1.7</maven.compiler.source>
            <maven.compiler.target>1.7</maven.compiler.target>
            </properties>

            <dependencies>
            <dependency>
                  <groupId>junit</groupId>
                  <artifactId>junit</artifactId>
                  <version>4.11</version>
                  <scope>test</scope>
            </dependency>
            </dependencies>
            
            <build>
            ...
            </build>
      </project>

<br>

Compile & package application. 

      cd /dest_path/WeatherApp/
      sudo mvn package

      [INFO] Building jar: /opt/maven/WeatherApp/target/WeatherApp-1.0-SNAPSHOT.jar
      [INFO] ------------------------------------------------------------------------
      [INFO] BUILD SUCCESS
      [INFO] ------------------------------------------------------------------------
      [INFO] Total time: 8.181s
      [INFO] Finished at: Fri Jan 21 18:16:39 UTC 2022
      [INFO] Final Memory: 24M/1481M
      [INFO] ------------------------------------------------------------------------

<br>

Run application.

      java -cp /opt/maven/WeatherApp/target/WeatherApp-1.0-SNAPSHOT.jar org.ForestFoundation.app.App


<br>

**Example DevOps operations**

- Automating a build process with CI/CD pipelines 
  - (continuous integration and either continuous delivery or continuous deployment)
- Containerising an application with Docker

<br>

**Documentation**

      javadoc -d doc Forecast.java

      Loading source file Forecast.java...
      Constructing Javadoc information...
      Creating destination directory: "doc/"
      Standard Doclet version 13.0.2
      Building tree for all the packages and classes...
      Generating doc/Forecast.html...
      Generating doc/package-summary.html...
      Generating doc/package-tree.html...
      Generating doc/constant-values.html...
      Building index for all the packages and classes...
      Generating doc/overview-tree.html...
      Generating doc/deprecated-list.html...
      Generating doc/index-all.html...
      Building index for all classes...
      Generating doc/allclasses-index.html...
      Generating doc/allpackages-index.html...
      Generating doc/index.html...
      Generating doc/help-doc.html...

<br>
<br>



### <span id="ref">References</span>

[[☝️top]](#top)

  - Kodekloud.com