---
title: "Adding files to gitignore"
datePublished: Sun Apr 16 2023 07:08:42 GMT+0000 (Coordinated Universal Time)
cuid: clgj2fx5l000d09ieatv1cxxl
slug: adding-files-to-gitignore
tags: git

---

* Update the `.gitignore` file and save it
    
* Then in terminal, type `git rm -r --cached .`
    
    * This will remove all the cached files
        
* Then type `git add .` and commit the changes with `git commit -m "Update git cache"`
    

That should be it!

If you have already started on a project and add these steps later:

* Your additions to the gitignore file will not be effective until you remove the cache, as per the steps above