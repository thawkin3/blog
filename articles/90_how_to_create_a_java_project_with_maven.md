# How to Create a Java Project with Maven

Maven is a popular tool for managing software projects. It can help you create, build, test, document, and deploy your app. Maven is a great choice for Java projects and can be run through the command line or with an IDE like IntelliJ.

If you’re new to Java, the tooling ecosystem can feel daunting to learn. But, you can create your first Java project with Maven in just a few minutes by using a project template called an archetype! In this article, we’ll show you how.

## Step 1: Install Java

You can download and install Java from [Oracle’s website](https://www.oracle.com/java/technologies/downloads/). Choose the version you want for macOS, Linux, or Windows.

## Step 2: Set your Java environment variables

It’s recommended to set an environment variable called `JAVA_HOME` that stores the path to where Java is installed on your machine. For Mac users, you can add this to your `~/.bash_profile` file like this:

```bash
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-18.jdk/Contents/Home"
```

Just be sure to modify that value with your correct location and version of Java.

## Step 3: Verify Java is installed

Once you have Java installed on your machine, you can run the following from your command line as a sanity check that it is in fact installed:

```bash
java -version
```

This will output to your console a few lines that look like this:

```
java version "18" 2022-03-22
Java(TM) SE Runtime Environment (build 18+36-2087)
Java HotSpot(TM) 64-Bit Server VM (build 18+36-2087, mixed mode, sharing)
```

Again, the exact output will vary based on your operating system and the version of Java you’re using.

## Step 4: Install Maven

Next, we can download and install Maven from the [Apache Maven Project website](https://maven.apache.org/download.cgi). The latest version at the time of writing is 3.8.5.

## Step 5: Set your Maven environment variables

Once we have Maven installed, we’ll need to set a couple environment variables for Maven as well. For Mac users, this can again go in your `~/.bash_profile` file:

```bash
export MAVEN_ROOT="$HOME/path/to/maven/apache-maven-3.8.5"
export PATH="$MAVEN_ROOT/bin:$PATH"
```

Again just be sure to modify that value with your correct location of your Maven directory.

The `MAVEN_ROOT` variable we used here is just a helper variable. The important part is adding the executable for Maven to your `PATH` variable so that your machine can execute Maven commands successfully.

## Step 6: Verify Maven is installed

We can verify that we’ve installed Maven properly by running the following from the command line:

```bash
mvn --version
```

The output will look something like this:

```
Apache Maven 3.8.5 (3599d3414f046de2324203b78ddcf9b5e4388aa0)
Maven home: /Users/yourusername/path/to/maven/apache-maven-3.8.5
Java version: 18, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-18.jdk/Contents/Home
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.14.4", arch: "x86_64", family: "mac"
```

If you get an error that the `mvn` command cannot be found, make sure the path in your environment variable is correct and then try running the command in a new terminal shell or reloading your bash profile file with source `~/.bash_profile`.

## Step 7: Generate your project using an archetype

Now that we have Java and Maven successfully installed, we can create a new Maven project using an archetype. Archetypes are project templates that bootstrap an app for you based on the archetype you choose.

We’ll use the `maven-archetype-quickstart` archetype for our example. Run the following from the command line in a directory where you want your project to be created:

```bash
mvn archetype:generate -DgroupId=com.tylerhawkins.examples -DartifactId=plinko -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Let’s break it down.

`mvn archtype:generate` is the command we’re running. The `-D` prefix is used to add arguments to the command.

We use `-DgroupId` to specify our group ID, which will be the high-level package we want this to be stored under. Your company may have a common convention they use, but this is really an arbitrary value. I picked `com.tylerhawkins.examples`, but it could have been anything.

We use `-DartifactId` to name our project and the build artifact it creates. I was building a Plinko game, so I named mine `plinko`.

We use `-DarchetypeArtifactId` to specify the archetype, or project template, we’d like to use. We chose `maven-archetype-quickstart`.

Finally, we use `-DinteractiveMode=false` to run this command without requiring any further interactive input from us.

The newly created project will be inside a directory called `plinko` (the name of our artifact ID), and the directory structure will look like this:


![Project directory structure for a new Maven project](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hxx9rxi6hjg09mgc8fyq.png)
<figcaption>Project directory structure for a new Maven project</figcaption>

Maven is opinionated about the directory structure of its projects, favoring convention over configuration. You can see there are two file trees in the `src` directory, one for the app code and one for the test code. The `target` directory is where compiled and packaged code goes. The `pom.xml` file is a Project Object Model file that contains all the metadata about our project.

## Step 8: Specify the source and target version of Java you’d like to use

We’re almost ready to build our app. First though, we need to specify the source and target version of Java that we’re using. The source is the version of Java that we’re using to write our code, and the target is the version of Java that we want our compiled app to be compatible with.

We can add these properties to our `pom.xml` file. I’m using Java 18 for both the source and the target:

```xml
<properties>
  <maven.compiler.source>18</maven.compiler.source>
  <maven.compiler.target>18</maven.compiler.target>
</properties>
```

## Step 9: Build and test the app

With that, we’re ready to build and test our app. Maven has built-in lifecycles that contain phases where various work gets done. Some of the phases of the default Maven build lifecycle are `validate`, `compile`, `test`, `package`, `integration-test`, `verify`, `install`, and `deploy`.

We can build and test our app by running the following from our project root directory:

```bash
mvn clean package
```

The `clean` command removes previous build output from the `target` directory, and then the `package` command runs through several phases to compile, test, and package the app into an executable JAR file.

## Step 10: Run the app

Finally, we can run our app by running the following from our project root directory:

```bash
java -cp target/classes com.tylerhawkins.examples.App
```

This will run our `App` file, which simply outputs:

```
Hello World!
```

We did it! We created a Java app using Maven and successfully ran it!

## Next Steps

Now that you have the working barebones of an app, you can starting writing your Java code. You’ll likely add more source files, add more configuration to your `pom.xml` file, and write more tests. We won’t cover any of that here, but by now you should know enough to at least get up and running. If you’d like to see the full code for a working example, here’s the [Plinko app](https://github.com/thawkin3/plinko-java-maven) I created.

Thanks for reading, and happy coding!
