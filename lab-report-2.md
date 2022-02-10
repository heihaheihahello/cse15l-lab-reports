## Fix bugs
> start with original MarkdownParse.java

---

* ### **bug-fix-1**

    The first [test file](https://heihaheihahello.github.io/lab-report2/test-file1.md) caused bug has no links:

    ```
    # Title

    what ever text
    ```
    - **Symptom**: The problematic output is: 
    ![Image](1w.jpg)
    > MarkdownParse.java got "StringIndexOutOfBoundsException" error related to the line with `substring` method.

    - Then we make the following change:
    ![Image](1_fix.jpg)
    > the fixed markdown of group is [this one](https://github.com/heihaheihahello/markdown-parse/commit/cb05f22b9c82efd0bcccac345a8483712464bd52#diff-c703a0ec03474d601c6bf846740b293e0538bccf38d5f677a302457479e9c652)

    - the fixed output: 

    ![Image](1fixed.jpg)

    > if there is no links, the output should be a single `[]` with nothing inside. Now the output is what we expected, we fixed the bug for this test file.

    - Analysis: Before we fixed, if there is no `[]` and `()`, the index of all `[]()` will be `-1`. Then the `substring(0,-1)` will have error becuase from index `0` to `-1` is not existed. After We add a breaking point after we look for the first `[`, if there is no `[` existed in the file, the index will be `-1` and then immediately break the loop. Then nothing is added in `toReturn` then the error on `substring` will be happened. So we will only output `[]`. 

---

* ### **bug-fix-2**
    

    The second [test file](https://heihaheihahello.github.io/lab-report2/test-file2.md) caused bug, there is a [] and () but they are not markdown of links:

    ```
    # Title

    what ever text

    we will use `[]` for writing links(it must be).
    [another link!](some-page.html)

    ```
    > In this test file, there is a [] and () but they are not markdown of links

    - **Symptom**: The problematic output is:

    ![Image](2-w.jpg)

    > `it must be` is not a link but is also outputed.
      
    - Then we make the following change:
    ![Image](2_fix.jpg)
    > the fixed markdown of group is [this one](https://github.com/heihaheihahello/lab-report2/commit/b18f0e5a3144cf7e7c77be995c9af0cdf1eb8c52#diff-c703a0ec03474d601c6bf846740b293e0538bccf38d5f677a302457479e9c652)

    - the fixed output: 

    ![Image](2fixed.jpg)

    > now we only output the links without normal text in `()`.

    - Analysis: Before we fix, the program will find next `()` after locating a `[]` without considering how many text between `]` and `(`. Only the form of `[](link)` is counted as link. In other words, only when we find the next index of `]` is `(`, we take the content inside `()` as links. The fix is to add this check, if `(` is not immediately behind `]`, we don't add the content in `()` to `toReturn` and `continue` to look for next `[` and `]`.
    
---

* ### **bug-fix-3**

    The third [test file](https://heihaheihahello.github.io/markdown-parse/test-file3.md) caused bug end with `()[]`:

    ```
    [](acacacac)

    ()[]
    ```
    - **Symptom**: The problematic output is: 

    ![Image](3w.jpg)

    > we can see the program infinitely runs the printout statement and does not stop until stop by `ctrl + c`.

    - Then we make the following change:
    ![Image](3_fix.jpg)

    > the fixed markdown of group is [this one](https://github.com/heihaheihahello/markdown-parse/commit/17d4532ef3402b3db58f732f23f611bc3b47d86c#diff-c703a0ec03474d601c6bf846740b293e0538bccf38d5f677a302457479e9c652)

    - the fixed output:

    ![Image](3fixed.jpg)

    >now we run the print statement in finite time and output the only link, which is what we expected.

    - Analysis: The bug happened because we don't have a break point for searching `(`. The breaking point existed is to break when there is no more `[`. After the correct link, the program found the next `[]`, but there is no next `()` so that index of `(` is `-1` and index of `)` is `11`. Then the `currentIndex` is always `12`. In other words, the program just repeated adding the first line with correct link, as a **infinite loop**. So we add another breaking point to check if there is more `(` after `[]`, if not, we end the program and return what we got.

---
End of 2nd lab report. Thanks for watching!





