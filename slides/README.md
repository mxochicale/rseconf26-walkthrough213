# Setting up slides in quarto and github
The following are few steps to setup your quarto slides in github pages.


## Creating scalfolding, ci workflows, code of conduct, contributions and slides

```bash
git clone git@github.com:mxochicale/{ADD_REPOSISTORY_NAME}.git
git checkout #hash
```
See [hash for template]( TODO_HASH_TEMPLATE )


## Edit and preview slides

### Dependencies
* quarto installation (See more: https://github.com/mxochicale/tools/tree/main/quarto)
```bash
wget https://raw.githubusercontent.com/mxochicale/tools/refs/heads/main/quarto/download_install_quart.bash
bash download_install_quart.bash
rm download_install_quart.bash
quarto check
```

* quarto version
```bash
quarto --version
1.9.38
```

### Quarto extensions
```bash
quarto list extensions
quarto add quarto-ext/fontawesome
quarto remove quarto-ext/fontawesome
```

### Edit and preview slices
* Open [index.qmd](index.qmd) to edit slides. 
* Then you can preview them:
```bash
cd slides
quarto preview index.qmd
```
* Clean project
```bash
cd slides
rm -rf _site docs _freeze .quarto */.jupyter_cache
```

## Commit and upload slides
* first commit
```bash
git add .
git commit -m ':fire: 1st commit: adds scalfolding for slides #1'
git branch -M main
git push -u origin main
```


* Create gh-pages branch
```bash
git checkout --orphan gh-pages 
#An orphan branch is not connected to the other branches and commits, and its working tree has no files at all. 
#See [here](https://git-scm.com/docs/git-checkout) for more info.
git reset --hard
git commit --allow-empty -m "Initializing gh-pages branch"
git push origin gh-pages
git checkout main
#https://jiafulow.github.io/blog/2020/07/09/create-gh-pages-branch-in-existing-repo/
```
* Go to Seetings/Pages and Select `Deploy from a branch` and select `gh-pages` branch (`/root` path)


## Push changes and publish slides
```bash
git add .
git commit -m '<add message> CI #ISSUE_NUMBER'
git push origin <feature_branch>
```

## References
* https://quarto.org/docs/presentations/revealjs/
* https://quarto.org/docs/presentations/revealjs/advanced.html

