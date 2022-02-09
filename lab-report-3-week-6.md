# Week 6 Lab Report: Copying whole directories with `scp -r`
---
[Back To Home](https://ryan-truong.github.io/cse15l-lab-reports/)

---

# Copying whole markdown-parse directory into ieng6

## First Option: Copying Everything
* To copy a whole directory, use the command `scp -r . cs15lwi22xxx@ieng6.ucsd.edu:~/markdown-parse`
* When doing so, everything in the local directory will be copied over to a directory called `markdown-parse` on the remote server (this includes all the git commits and components)


![scp_1](/labreport3_pictures/scp_1.png)
![scp_2](/labreport3_pictures/scp_2.png)
![scp_3](/labreport3_pictures/scp_3.png)

## Second Option: Copying Specific Files
* To copy over only specific files into a directory, the directory first must be made in the remote server by doing:

```
ssh cs15lwi22xxx@ieng6.ucsd.edu
mkdir markdown-parse
```

* Then exit out of the remote server and on the local machine use `scp -r *.java *.md lib/ cs15lwi22xxx@ieng6.ucsd.edu:markdown-parse`

* This will cause only the files that end with .java, .md, and the lib folder from the directory to be copied over

![scp_4](/labreport3_pictures/scp_4.png)
![scp_5](/labreport3_pictures/scp_5.png)


# Logging into ieng6 account & compiling and running tests
* After the whole directory is copied over, `ssh cs15lwi22xxx@ieng6.ucsd.edu` is run to log into the remote server
* Once in the remote server, use `cd markdown-parse` to go inside the markdown-parse directory so tests can be run
* Run the test files created by using:

```
javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParseTest.java

java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest
```

* **NOTE:** Copying over the whole directory will result in the `.class` files being copied over too, which may cause incompatability issues when running the tests. If this occurs remove the class files by using:

```
rm MarkdownParse.class
rm MarkdownParseTest.class
```

![ssh_test](/labreport3_pictures/ssh_test.png)

# Combining `scp`, `;`, and `ssh` to copy whole directory and test in one line
* To perform the two previous steps all in one command line, use:
```
scp -r . cs15lwi22xxx@ieng6.ucsd.edu:~/markdown-parse-one-line; ssh cs15lwi22xxx@ieng6.ucsd.edu "cd markdown-parse-one-line; rm MarkdownParseTest.class; rm MarkdownParse.class; /software/CSE/oracle-java-se-14/jdk-14.0.2/bin/javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParseTest.java; /software/CSE/oracle-java-se-14/jdk-14.0.2/bin/java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest"
```



* **NOTE:** In this example, everything is copied into the remote server directory called markdown-parse-one-line because markdown-parse was already created/used in the previous steps

* The commands shown below are used instead of `javac` and `java` because the way the remote server is set up uses OpenJDK when calling `ssh cs15lwi22xxx@ieng6.ucsd.edu "commands"`, which is outdated to compile our files. Thus, the specific location of the java file must be called.

```
/software/CSE/oracle-java-se-14/jdk-14.0.2/bin/javac
/software/CSE/oracle-java-se-14/jdk-14.0.2/bin/java
```



![scp_oneline1](/labreport3_pictures/scp_oneline1.png)


![scp_oneline2](/labreport3_pictures/scp_oneline2.png)

* **NOTE:** Some of the git files that were copied to the remote server were not included between the screenshots (can also do it using second option mentioned above)