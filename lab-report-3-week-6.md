# Week 6 Lab Report: Copying whole directories with `scp -r`
---
[Back To Home](https://ryan-truong.github.io/cse15l-lab-reports/)

---

# Copying whole markdown-parse directory into ieng6

* To copy a whole directory, use the command `scp -r . cs15lwi22xxx@ieng6.ucsd.edu:~/markdown-parse`
* When doing so, everything in the local directory will be copied over to a directory called `markdown-parse` on the remote server (this includes all the git commits and components)


![scp_1](/labreport3_pictures/scp_1.png)
![scp_2](/labreport3_pictures/scp_2.png)
![scp_3](/labreport3_pictures/scp_3.png)

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