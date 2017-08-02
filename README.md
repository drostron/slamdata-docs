# How to Edit SlamData Documentation


SlamData documentation is a public repository on github at https://github.com/slamdata/slamdata-docs  This document does not yet cover how to create a new branch.

This document assumes:

* you have a git command line client installed.


## Obtaining the Documentation


First we’ll need to clone the documentation repository so we can make changes.

```
[/Users/damon]$ mkdir git
[/Users/damon]$ cd git
[/Users/damon/git]$ git clone https://github.com/slamdata/slamdata-docs.git
Cloning into 'slamdata-docs'...
remote: Counting objects: 1165, done.
remote: Compressing objects: 100% (88/88), done.
remote: Total 1165 (delta 39), reused 77 (delta 17), pack-reused 1060
Receiving objects: 100% (1165/1165), 12.71 MiB | 2.71 MiB/s, done.
Resolving deltas: 100% (594/594), done.
[/Users/damon/git]$ cd slamdata-docs
```


## Making Changes


Whenever a specific change is needed we should create a branch to hold this change. Doing this allows us to keep focused on a single area of the documentation that needs changing. It also makes for easier review and merging later when we only have to review one section and it doesn’t compete with changes from others.

Let’s say we want to make a change Section 3 in the Administrator’s Guide. To prep for that, we could use the following commands:


```
[../damon/git/slamdata-docs]$ git checkout -b damon-admin-section3
Switched to branch 'damon-admin-section3'
```


Now any changes we make will be limited to the `damon-admin-section3` branch and will not affect either our local master copy or the remote repository.

Once the necessary changes have been made to the administrators guide, we need to make sure it looks good before committing our changes.


## Testing Documentation Locally


We currently use ReadTheDocs.org to host our documentation. It is a free service and has integration with GitHub’s branch management. This also means when you build the local docs it should look almost identical to how it looks live on the doc web site. After the prerequisites have been installed (not covered here, yet) you can make changes to the docs, build them, then test them. See the example below and know that you may see some warnings but these are typically benign.


```
[../damon/git/slamdata-docs]$ make html
sphinx-build -b html -d build/doctrees   source build/html
Making output directory...
Running Sphinx v1.2.3
loading pickled environment... not yet created
building [html]: targets for 12 source files that are out of date
updating environment: 12 added, 0 changed, 0 removed
reading sources... [100%] users-guide
looking for now-outdated files... none found
pickling environment... done
preparing documents... done
writing output... [100%] users-guide
writing additional files... genindex search
copying images... [100%] images/SD4/screenshots/date-only.png
copying static files... WARNING: html_static_path entry u'/Users/damon/git/slamdata-docs/source/_static' does not exist
done
copying extra files... done
dumping search index... done
dumping object inventory... done
build succeeded, 9 warnings.

Build finished. The HTML pages are in build/html.
```


The process above has now created a new directory called `build` under the current directory. You can open the `build/html/index.html` file to see your local documentation now. On macOS you can simply type the following command rather than opening a browser and pointing it to the file:


```
[../damon/git/slamdata-docs]$ open build/html/index.html
```


## Pushing Updates to GitHub


Once you have made your changes and are satisfied with how they look in your locally built environment, it’s time to push them back up to github. This will push the changes to github into our `damon-admin-section3` branch, but not to master yet.

It may help to first see what changes have been made:


```
[../damon/git/slamdata-docs]$ git status
On branch damon-admin-section3
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   source/administration-guide.rst

no changes added to commit (use "git add" and/or "git commit -a")
```


This output is telling us that we are working with the `damon-admin-section3` branch and the `administration-guide.rst` file has been changed but has not been staged for a commit. Files must be staged before committed. Now we can stage the file:


```
[../git/slamdata-docs/source]$ git commit -a
```


You’ll then be prompted (typically with your command line text editor like vi) to insert a brief description of your changes, with several lines of template information below it. The first line in the following example is what I typed, then I saved it (via `:wq` in vi)


```
Changed a few lines here and there.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch damon-admin-section3
# Changes to be committed:
#   modified:   source/administration-guide.rst
#
```


You should see output on the command line similar to this after you save:


```
[damon-admin-section3 8a9ac39] Changed a few lines here and there.
 1 file changed, 1 insertion(+), 1 deletion(-)
 ```


Finally, push your local committed changes up to the working branch in GitHub:


```
[../damon/git/slamdata-docs]$ git push -u origin damon-admin-section3
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 369 bytes | 0 bytes/s, done.
Total 4 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
To https://github.com/slamdata/slamdata-docs.git
 * [new branch]      damon-admin-section3 -> damon-admin-section3
Branch damon-admin-section3 set up to track remote branch damon-admin-section3 from origin.
```


Now that the remote tracking branch is setup (the `-u` feature) we can simply call git push without any parameters to push the changes.

You may continue to make changes using the steps from above:


```
git status
git commit -a
git push
```


## Submitting the Pull Request


Changes have been committed. Now the changes must be approved by a team member. Go to the repository URL again https://github.com/slamdata/slamdata-docs

Create a new Pull Request by clicking on the **New pull request** button:

Select the branch you want to submit the Pull Request for:


Click on the **Create pull request** button:


Update the title of the pull request if necessary and any comments in the body section, then finally click **Create pull request** again:


This starts the approval workflow. Someone from the team must now go in and approve the changes before they are merged with master.


## Updating documentation website


After the Pull Request has been approved and the changes have been merged into the master branch, we can push those changes to our documentation website.

The doc website has a versioning system that allows users to select which version of the documentation is displayed. The default version is `latest` which corresponds to the GitHub `master` branch. The steps below explain pushing out the latest changes to the doc web site.

1. Go to https://readthedocs.org/
2. Login
3. Click “SlamData Documentation” in the list of Projects:
![List of Projects](/git_images/docs1.png?raw=true)
4. Click the “Builds” button:
5. Ensure “latest” is selected from the drop down, then click “Build Version”:

The build process will begin. You can click on the top most entry in the table that says **Triggered** if you’d like to watch the build progress. Once complete, you can visit the doc site at http://docs.slamdata.com to verify the latest branch has been updated.


## Making New Version Releases


Newer versions of documentation (such as v4.3, etc) will be documented in the near future. For now, this is a placeholder. A great resource is this link https://www.atlassian.com/git/tutorials/comparing-workflows which explains how we’re using the **Feature Branch Workflow** approach. This will require periodic refreshing of the latest branch (i.e. v4.2, or whatever is latest) from master.
