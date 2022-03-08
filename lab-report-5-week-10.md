## lab report 5

---

* ## Looking for bugs using diff
    1. we need to create a file contains all the result from test cases by using
    
    ```
    bash script.sh > results.txt
    ```
    > this command can run the `script.sh` and store all the result of test cases into a file named `results.txt`.

    ![Image](createresult.jpg)
    >In the directory we can see a new file `results.txt` is created. In this file we can see all the test files and corresponding results.

    2. Repeating the step 1 for my own `markdownParse` and provided `markdownParse`

    3. Now we can use command `diff` to compare two groups of results by using
    ```
    diff results.txt ../markdown-parse-politz/results.txt
    ```

    Then we can get the following comparison:
    ![Image](diff.jpg)
    > for each difference, we can see the line having difference in `results.txt`, and the difference itself. By tracing the line in the `results.txt`, we can see which test file causing difference and look for the bugs.

---
* ## Analyze two test with different results
    * ## **test 1-[41.md](41.md)**
    ```
    [a](url &quot;tit&quot;)

    ```
    Difference: 
    ![Image](test1.jpg)
    > by looking at line 691 of my `results.txt` we can see this difference is caused by `41.md`, and my output is `[url &quot;tit&quot;]` while the provided `MarkdownParse`'s output is `[]`.

    ### **Analysis**: 
    - **whose is correct**； By using the preview function, this "link" turns in `[a](url "tit")` and is not counted as link. So my `MarkdownParse` is wrong and the provided `MarkdownParse` is correct. 
    - **analyze the bug**: If we look at the preview, it is interesting that `&quot;` turn in `"` in the preview instead of keeping the form of txt `&quot;`. When this `md` file turn into `html` file, all chars of `&quot;` will turn in `"` and prevent the contents inside `()` to be a link. In my `MarkdownParse`, I didn't check if the potential link contains `&quot;` and then cause this bug. In addition, after I search in the internet, there are many HTML Character Entities like this, like `&gt;` to `>`. 
    - **Solution**: One way to solve it is to add a check method which checks if a potential link contains any Character Entities like "&quot;". But this way seems unwise because there are tons fo Character Entities. Comparing one by one is time-consuming. Maybe we can use another way. All these entities are in form of `&xxxx;` (start with `&` and end with `;`). I think we can use a check method to see if there is a one pair of `&` and `;`. This is exactly like the searching of `[]` pair and `()` pair. So it would not be hard to solve it.
    >we can add a new method. Right before we append the link into final result, we can put another if statement of check html character entities inside this `if` statement:
    ![Image](test1placetofix.jpg) 

    ---
    * ## **test 2-[577.md](577.md)**
    ```
    ![foo](train.jpg)

    ```
    Difference: 
    ![Image](test2.jpg)
    > by looking at line 1064 of my `results.txt`, we can see this difference is caused by `577.md`, and my output is `[]` while the provided `MarkdownParse`'s output is `[train.jpg]`.

    ### **Analysis**: 
    - **whose is correct**； By using the preview function, this "link" is actually a image and is not counted as link. So my `MarkdownParse` is correct and the provided `MarkdownParse` is incorrect. 
    - **analyze the bug**: becuase the code to add image `![...](...)`is almost the same as the code of link`[...](...)`. The only difference is the `!` before `[]`. And the provided `MarkdownParse` does not check if there is a `!` before `[]` so that it will treat all images as links, and causing bugs. My 'MarkdownParse' has this check so it works good. 
    - **Solution**: It is easy to solve this bug. We just need to check if there is a `!` before the `[...](...)` form we found. So at the `if` statement to add the link into result, we can add the follow checking statement: 
    ```
    markdown.substring(nextOpenBracket-1, nextOpenBracket).equals("!") == false)
    ``` 
    >In the provided `MarkdownParse`, we can find the the following `if` statement as the last checking point and add the fixing statement here:
    ![Image](test2placetofix.jpg) 

---
end of 4th lab report.
Thanks for watching:)

    

