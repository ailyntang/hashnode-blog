## Create a git patch

**Create the patch**
1. Commit what you want to patch (there's probably a way to do this without committing the patch.. )
2. `git show`
3. Copy the patch, which looks something like this

```
diff --git a/Controllers/WebviewController.swift b/Controllers/WebviewController.swift
index a9eb29a3a..4f3fca3be 103644
--- a/Controllers/WebviewController.swift
+++ b/Controllers/WebviewController.swift
@@ -72,10 +72,10 @@ final class WebViewController {
 
     private func Meep() -> Bool {
-        guard moop else { return false }
+//        guard meepMorp else { return false }
```


**Apply the patch**
1. Copy the patch
2. In terminal type, make sure you are in the right branch
3. Type `pbpaste | git apply`
  - This is all on one line
  - The `|` is kind of like a pipe command

**Nice styling for the "apply patch" instructions in MarkDown**

```
<details>
<summary>Expand for a git patch</summary>

------

<insert Code for patch>

Copy the above and run `pbpaste | git apply`


-------

</details>
```
