# Week 8 Lab Report: MarkdownParse Tests
---
[Back To Home](https://ryan-truong.github.io/cse15l-lab-reports/)

---

# Code Snippet 1

## VSCode Preview
![snippet1](/labreport4_pictures/snippet1.png)

```
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```
## What MarkdownParse Should Produce
* Based on the VSCode preview, the last two links were clickable even if they were in code snippets, so those should be considered links
* The first two links were also clickable links so they should also be considered links

## `MarkdownParseTest.java` Test Code
```
@Test
public void testSnippet1() throws IOException {
    Path fileName = Path.of("labreport4-snippet1.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("url.com", "`google.com", "google.com","ucsd.edu"), MarkdownParse.getLinks(contents));
}
```

## Test Output: My Group's Implementation
![Snippet 1 Output 1](/labreport4_pictures/snippet1_output.png)

* Since the expected output was printing out all the links and the test passed, this means that our MarkdownParse handles this case correctly

## Test Output: Reviewed Group's Implementation
![Snippet 1 Output 2](/labreport4_pictures/snippet1_output2.png)

* The expected output was printing out all the links and the test failed, specifically `ucsd.edu` was not printed

## Answering Questions
* No code change is needed for Snippet 1 because the expected output was printed, which was shown through the passed test. 

* For the reviewed group's version of MarkdownParse, a small code change can fix this issue. Their problem with this example is the usage of brackets in backticks. They check if a link is valid by checking to see if the number of open brackets is more than closed brackets (shown below) and it is evident in the last example, this wouldn't hold true. To fix this, they can also check if the line contains backticks.

```
return container.length() >= 2 && numOpenBrackets >= 1 &&
    numClosedBrackets >= 1 && numOpenBrackets >= numClosedBrackets;
```

# Code Snippet 2

## VSCode Preview
![snippet2](/labreport4_pictures/snippet2.png)

```
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```

## What MarkdownParse Should Produce
* Based on the preview of the first link, MarkdownParse should produce two links, `a.com` and `b.com`
* For the other two links, based on preview, these are clickable links, thus MarkdownParse should also return `a.com(())`, with the parenthesis in the link, and `example.com`

## `MarkdownParseTest.java` Test Code
```
@Test
public void testSnippet2() throws IOException {
    Path fileName = Path.of("labreport4-snippet2.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("a.com", "b.com", "a.com(())","example.com"), MarkdownParse.getLinks(contents));
}
```

## Test Output: My Group's Implementation
![Snippet 2 Output 1](/labreport4_pictures/snippet2_output.png)

* As shown above, the test did not pass. Our version of MarkdownParse did not include the parenthesis of `a.com(())` 

## Test Output: Reviewed Group's Implemenetation
![Snippet 2 Output 2](/labreport4_pictures/snippet2_output2.png)

* As shown above, the test did not pass. The reviewed group's version of MarkdownParse printed `a.com)](b.com`, which included the parenthesis and brackets in between the two links

## Answering Questions
* A small code change may be possible for our groups implementation of MarkdownParse. The way our code works is to find the period in the sentence and expand outwards in both directions until we find a specific stop character (shown below). A way to fix the code to obtain the parenthesis for the link is to keep count of how many open/closed parenthesis there are and their indices and include all but the first open and last closed parenthesis.

```
for(int periodIndex: periodList) {
            int startIndex = periodIndex;
            int endIndex = periodIndex;
            ArrayList<Character> stopCharacters = new ArrayList<Character>();
            stopCharacters.add('(');
            stopCharacters.add(')');
            stopCharacters.add('[');
            stopCharacters.add(']');
            stopCharacters.add('\n');
            stopCharacters.add(' ');
            //... (more code not included)
```

# Code Snippet 3

## VSCode Preview
![snippet3](/labreport4_pictures/snippet3.png)

```
[this title text is really long and takes up more than 
one line

and has some line breaks](
    https://www.twitter.com
)

[this title text is really long and takes up more than 
one line](
    https://ucsd-cse15l-w22.github.io/
)


[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/
```

## What MarkdownParse Should Produce
* Based on the VSCode preview, all the links are clickable even if they may not be referenced to text, so the output should include all these links

## `MarkdownParseTest.java` Test Code
```
@Test
public void testSnippet3() throws IOException {
    Path fileName = Path.of("labreport4-snippet3.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("https://www.twitter.com", "https://ucsd-cse15l-w22.github.io/", "github.com","https://cse.ucsd.edu/"), MarkdownParse.getLinks(contents));
}
```
## Test Output: My Group's Implementation
![Snippet 3 Output 1](/labreport4_pictures/snippet3_output.png)

* The test failed when ran. It was expected that all the links given would print, but extra output was printed such as `that.` and `"https://www.twitter.com"` and `https://cse.ucsd.edu/` to be printed out twice

## Test Output: Reviewed Group's Implemenetation
![Snippet 4 Output 2](/labreport4_pictures/snippet3_output2.png)

## Answering Questions
* A small code change will not be able to fix this issue for our group's MarkdownParse. The reason why is because our implementation relies on searching for the period in the link and expanding outwards both directions until the closing parenthesis are found. This works when there is only one period within the link, but once there are more as shown above, the expected output is not achieved, thus a larger code change to filter for the correct periods is needed.
