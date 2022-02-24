# Week 8 Lab Report: MarkdownParse Tests
---
[Back To Home](https://ryan-truong.github.io/cse15l-lab-reports/)

---

# Code Snippet 1

## CommonMark Preview
![snippet1](/labreport4_pictures/snippet1-new.png)

```
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```
## What MarkdownParse Should Produce
* Based on the CommonMark preview, the last three links were clickable even if they were in code snippets, so those should be considered links
* The first link was not clickable, which stemmed from the fact that the first open bracket was in a code snippet.

## `MarkdownParseTest.java` Test Code
```
@Test
public void testSnippet1() throws IOException {
    Path fileName = Path.of("labreport4-snippet1.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("`google.com", "google.com","ucsd.edu"), MarkdownParse.getLinks(contents));
}
```

## Test Output: My Group's Implementation
![Snippet 1 Output 1](/labreport4_pictures/snippet1_output-new.png)

* Since the expected output was parsing all but the first link and the test failed, this means that our MarkdownParse handles this case incorrectly. Specifically, `url.com` was outputted when it was not supposed to be

## Test Output: Reviewed Group's Implementation
![Snippet 1 Output 2](/labreport4_pictures/snippet1_output2-new.png)

* The expected output was parsing out all the links except for `url.com` and the test failed, specifically `url.com` was parsed and `ucsd.edu` was not parsed

## Answering Questions
* A small code change may be possible to fix our MarkdownParse. For this specific test file, we don't want to print a link if the open or closed brackets are embedded in an inline code, so we can perform a check to find `` `[ `` and `` ` `` and if both those exists on the line of the link, do not output the link


# Code Snippet 2

## CommonMark Preview
![snippet2](/labreport4_pictures/snippet2-new.png)

```
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```

## What MarkdownParse Should Produce
* Based on the preview of the first link, MarkdownParse should produce one link, `a.com`
* For the other two links, based on preview, these are clickable links, thus MarkdownParse should also return `a.com(())`, with the parenthesis in the link, and `example.com`

## `MarkdownParseTest.java` Test Code
```
@Test
public void testSnippet2() throws IOException {
    Path fileName = Path.of("labreport4-snippet2.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("a.com", "a.com(())","example.com"), MarkdownParse.getLinks(contents));
}
```

## Test Output: My Group's Implementation
![Snippet 2 Output 1](/labreport4_pictures/snippet2_output-new.png)

* As shown above, the test did not pass. Our version of MarkdownParse included `b.com` and did not include the parenthesis of `a.com(())` 

## Test Output: Reviewed Group's Implemenetation
![Snippet 2 Output 2](/labreport4_pictures/snippet2_output2-new.png)

* As shown above, the test did not pass. The reviewed group's version of MarkdownParse printed `a.com)](b.com`, which included the parenthesis and brackets in between the two links and `b.com`

## Answering Questions
* A larger code change will be needed to make snippet 1 work. This is because the way our MarkdownParse works is that it searches for periods within a line and indexes outwards in both directions from the period until it finds stop characters like `(` and `)`.
* Based on the actual output received, we would have to implement a check for making sure the period of the link was contained within a nested link so we don't print `b.com` and check if parenthesis are part of the link instead of stopping at them so we don't print only `a.com`

# Code Snippet 3

## CommonMark Preview
![snippet3](/labreport4_pictures/snippet3-new2.png)

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



)

And then there's more text
```

## What MarkdownParse Should Produce
* Based on the CommonMark preview, the only link that should be parsed is `https://ucsd-cse15l-w22.github.io/` because it is the only clickable link

## `MarkdownParseTest.java` Test Code
```
@Test
public void testSnippet3() throws IOException {
    Path fileName = Path.of("labreport4-snippet3.md");
    String contents = Files.readString(fileName);
    assertEquals(List.of("https://ucsd-cse15l-w22.github.io/"), MarkdownParse.getLinks(contents));
}
```
## Test Output: My Group's Implementation
![Snippet 3 Output 1](/labreport4_pictures/snippet3_output-new.png)

* The test failed when ran. It was expected that only `https://ucsd-cse15l-w22.github.io/` would be parsed, but extra output was parsed such as `that.`, `"https://www.twitter.com"` and `https://cse.ucsd.edu/` twice, and all the other links

## Test Output: Reviewed Group's Implemenetation
![Snippet 3 Output 2](/labreport4_pictures/snippet3_output2-new.png)

* This test failed when ran. It was expected that only `https://ucsd-cse15l-w22.github.io/` would be parsed, but nothing was outputted at all

## Answering Questions
* Fixing snippet 3 is going to require a larger code change because  our group made the assumption that within a link there would be only one period which we can index out from to obtain the link, but as shown above when there are multiple periods, links get parsed multiple times. To fix this we need to implement some sort of check to find the last period within the link and ignore all the other ones
* Another issue that needs to be addressed is that our MarkdownParse outputs out all the links within the file, even though only one was previewed as a link. To fix this we will need to implement a code change that ensures that if there is one line of empty space within the link, the link won't output and if there is no ending parenthesis, the link won't output
