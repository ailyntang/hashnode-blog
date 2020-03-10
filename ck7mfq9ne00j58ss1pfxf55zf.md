## Cutting new releases

When you cut a new release for your app, you make a new release branch, something like `git checkout -b release/vx.x.x`

You can choose to branch off `master` or `develop`. 

* Typically if you are doing a major release, you will branch off `develop` because you want to bring across all the changes you've done in `develop`.

* If you are doing a hotfix, then you will likely branch off `master`, and then cherry pick the changes you want from `develop`.

On your release branch, you will also update the version and build number in your targets. This will be a brand new commit that no other branch has. You may also find some bugs when you dog food the release, and you may choose to make the changes on the release branch.

Once released to the app store, you then want to quickly merge your release branch back into BOTH `master` and `develop`. `Master` should always be a copy of what is live in the app store.

`develop` needs to know what you changed in the release branch, like the version and build numbers, as well as any bug fixes you may have done.