## lab report 3-copy whole directory with scp -r

---

* ## check if we are in the correct directory for uploading

    We firstly use `pwd` to check current path and 'ls' to check what is inside current directory:  

    ![Image](report3-1.jpg)
    

---

* ## using `scp -r` to copy
    ```
    scp -r . cs15lwi22awj@ieng6.ucsd.edu:~/markdown-parse
    ```
    ![Image](report3-2a.jpg)
    ![Image](report3-2b.jpg)

    >we can see everything in current directory is uploaded, including tons of things uploaded into ssh beside the files we need for markdownparse. Without these unnecessary files, the markdownparse can still run.

     - Analysis: `scp -r` is to copy everything in current directory, including the directory in current directory (`lib`).
    
---

* ## check if we successfullly copied to ssh server
    After we login ssh server, by using `ls `, we can see there is a `markdown-parse` directory:
    ![Image](report3-4.jpg)

    And then we can see everything is successfully uploaded:
    ![Image](report3-5.jpg)

* ## try only copying the files we need 

    By using the following:
    
    ```
    scp -r *.java *.md lib/ cs15lwi22awj@ieng6.ucsd.edu:markdown-parse
    ```

    ![Image](report3-3.jpg)

    > we can see in this time, we only copy all the `.java` files, `.md` files, and the two files in `lib` directory.

    - Analysis: The `scp -r` can combine with restrictions like `*.java *.md lib/` to only copy the files in the restriction. `*.java` means every `java` file. `*.md` means every `md` file. `lib/` means the `lib` directory. With these restriction, we can only upload the file we need, saving some time.

---
End of 3rd lab report. Thanks for watching!





