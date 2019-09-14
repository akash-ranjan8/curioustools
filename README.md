>>>> So this would be a mix of both github and mk docs environment.  
>>>> All the content of site has to stay directly inside gh-pages folder, so our git repository must have **only** the contents of our git repository(i.e .git, css, fonts,img,js,search,etc)  
>>>> So tldr , always use `git clone "www.github.com/link..." site` in some folder `xyz` and then take out readme.md, docs and mkdocs.yml out of  site folder such that your hiarchy becomes:  
```
xyz/
  - site/
      -  ... //all the git and website stuff
	  
  - docs/
  - readme.md
  - mkdocs.yml
```

>>>> 