# Week 8 Lab Report: MarkdownParse Tests
---
[Back To Home](https://ryan-truong.github.io/cse15l-lab-reports/)

---

[Link to Our Group's MarkdownParse Repo](https://github.com/ryan-truong/markdown-parse-new)

---

# How Test With Different Results Were Found
* To find the tests which had different results, we first had to run `java MarkdownParse` on all 652 commonmark-spec tests for each version of MarkdownParse. 
* To do so, we used the bash file given, `script.sh`, which used a for each loop that iterated through all the test files and ran `java MarkdownParse $file` where `$file` was the specific test file. 

```
script.sh

for file in test-files/*.md;
do
    echo $file
    java MarkdownParse $file
done
```

* On the command line, the actual command we used was `bash script.sh > results.txt`. This made it so we had a place to store all the outputs of running `java MarkdownParse` on each test file.

* We ran the command above using our version of MarkdownParse and the CSE15L MarkdownParse and to compare the results, I used `diff results.txt ~lab9/markdown-parse/result.txt`.

**NOTE:** Each implementation of MarkdownParse was stored in a different directory. So in this context, `results.txt` refers to using our group's implementation, while `~lab9/markdown-parse/result.txt` is the CSE15L implementation.

* After `diff` showed which lines in `result.txt` had different results, I used `vim result.txt` to find that result and see which specific test file it corresponded to (since we used `echo $file` in the script).


# Test 1

# Test 2