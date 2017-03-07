<!-- .slide: data-state="main" data-background="../../images/master.png" data-background-size="contain" -->
# Maven For Java Web Development <!-- .element: style="text-align: center;font-size: 2em;" -->

---

# Build System

---

## Conventions

- Variables `${Key}`
- Home Directory `${HOME}`
    - Linux: `$HOME` or `~`
    - Windows: `%HOMEPATH%`
- Path Separator `/`
    - Linux - `/`
    - Windows - `\`

---

## General Goals

- Manage source and configuration files
- Distribute and execute in a test environment
- Integrate third-party libraries
- Package and distribute project

---

## Maven Build System

- Implements best practices into build defaults
- Comprehensive build system
    - Compile
    - Test
    - Package
    - Distribute
    - Version
    - IDE integration
    - Report generation
- Customizable behavior

---

## Maven Architecture

- Maven Client
    - Plugin execution framework
- Project
    - Source tree
    - Maven configuration
- Repository
    - Addressable (URL) repository

---

## Maven Architecture...

- Local repository cache
    - Plugin downloads
    - Direct and transitive dependencies
    - Defaults to `${HOME}/.m2/repository`
       - Edit `settings.xml` for alternatives
       - `<settings><localRepository/></settings>`

---

## Client Personalization

- Global
    - Maven installation root `conf/settings.xml`
- User
    - `${HOME}/.m2/settings.xml`

---

## Maven Installation Process

- Verify prerequisites
    - JDK 1.4+
    - JAVA_HOME set to JDK installation root
- Download Maven
    - http://maven.apache.org/download.html
    - Maven 3.3.9 (zip/tar.gz/tar.bz2)
- Unpack download
    - Extract to target installation path

---

## Maven Installation Process...

- Set Maven Home
    - set variable `M2_HOME` to the installation path
        - contains bin, boot, conf, and lib directories
- Add M2_HOME bin directory to execution path
    - Add `${M2_HOME}/bin` to PATH environment setting
- Verify repository settings
    - Official, mirror, or corporate

---

## Verify Repository Settings

- Default
    - Official repository : a.k.a central: http://repo1.maven.org/maven2
- Mirror
    - Traditional Mirror
        - redundant site
    - Corporate repository
    - Private network
        - without internet access

---

## Repository Mirror

```xml
<settings>
    <mirrors>
        <mirror>
            <id>mirror</id>
            <name>mirror of central</name>
            <url>http://mirror/maven2</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
    </mirrors>
</settings>
```

---

# Build Process

---

## General Approach

- Compile source
    - Reference third-party libraries
    - Reference peer modules
- Integrate project with IDE
- Test binaries
- Package source for distribution

---

## Process

- Create project structure
    - project/src/main/java
    - project/src/main/resources
    - project/src/test/java
    - project/src/test/resources

---

## Process...

- Create project/pom.xml
    - Register Maven 2 NS
    - Define project meta-data
        - e.g project identifiers
    - Plugin settings
    - Specify dependencies
        - Third-party libraries
        - Local projects

---

## Process...

- Integrate with IDE
    - Prepare for eclipse "mvn eclipse:eclipse"
        - Plugins available for NetBeans and intellij
    - Refresh or import project into IDE
- Develop Source
    - src/main/java
- Perform build activities
    - Compile, test, package, generate reports and more ..
    - e.g maven clean install

---

## Create Project Structure

```
/projects-api
    /src
        /main
            /java
            /resources
        /test
            /java
            /resources
```

---

## Create pom.xml

```
/projects-api
    pom.xml
    /src
        /main
            /java
            /resources
        /test
            /java
            /resources
```

---

 - Register Maven schema and namespace mapping
 ```xml
<?xml version="1.0" encoding="UTF-8"?>
 <project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/maven-v4_0_0.xsd">

</project>
 ```

 ---

 - Define project identifiers
  ```xml
<?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
     http://maven.apache.org/maven-v4_0_0.xsd">

     <modelVersion>4.0.0</modelVersion>
     <groupId>coreservlets</groupId>
     <artifactId>coreservlets-api</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>jar</packaging>
     Java EE training: http://courses.coreservlets.com
     <packaging>jar</packaging>
     <name>coreservlets-api</name>

 </project>
  ```

---

- Define Compiler settings
```
<project>
    ...
    <build>
        <plugins><plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>2.0</version>
            <configuration>
                <source>1.5</source>
                <target>1.5</target>
            </configuration>
        </plugin></plugins>
    </build>
</project>
```

---

- Specify Dependencies

```xml
<dependencies>
    <dependency>
        <groupid>log4j</groupid>
        <artifactId>log4j</artifactId>
        <version>1.2.14</version>
    </dependency>
</dependencies>
```

---

## Develop Source

```
/projects-api
    pom.xml
    /src
        /main
            /java
                /projects
                    Customer.java
                    CustomerDao.java
            /resources
        /test
            /java
            /resources
```

---

## Perform Build Activities

- General Build tasks
    - mvn clean
    - mvn compile
    - mvn test
    - mvn package
    - mvn install
- Optionally, specify multiple build tasks
    - mvn clean install

---

## Build Artifacts

- Project projects-api
    - JAR
        - projects-api-1.0-SNAPSHOT.jar
