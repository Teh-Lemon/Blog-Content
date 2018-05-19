+++
title = "Hugo Cheat Sheet"
slug = "hugo-cheat-sheet"
date = 2018-05-17T05:04:10+01:00
tags = ["Hugo", "Web Dev"]
categories = ["Web"]
draft = false
+++
<!--more-->

* Content
    * Posting
        * `hugo new blog/2018/my-first-post.md`  
        * `hugo new notes/2020/how-to.md`
    * Font Awesome
        * `<i class="fas fa-lemon" aria-hidden="true"></i>`
* Testing
    * `hugo server`  
    * `http://localhost:1313`
* Build
    1. Remove old files  
        `rmdir /S /Q public`
    1. Build site  
        `hugo -b https://www.tehlemon.com`
    1. Push to production repo  

        ```bash
        cd public  
        set /p commitmsg="Enter a commit message: "  
        git add --all .  
        git commit -m "%commitmsg%"  
        git push origin master  
        ```

* Theming
    * Adding Pages
        1. Add to nav.html in `/layouts/partials/`
        2. Add to config.toml
        3. Add folder to `/content/`