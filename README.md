- So this would be a mix of both github and mk docs environment.  
- All the content of site has to stay directly inside gh-pages folder, so our git repository must have **only** the contents of our git repository(i.e .git, css, fonts,img,js,search,etc)  
- So tldr , always use `git clone "www.github.com/link..." site` in some folder `xyz` and then take out readme.md, docs and mkdocs.yml out of  site folder such that your hiarchy becomes:  
```
xyz/
  - site/
      -  ... //all the git and website stuff
	  
  - docs/
  - readme.md
  - mkdocs.yml
```

- Then go to this `xyz/` folder and add your new blogs and stuff to `docs/` folder. don't forget to add the blog name in the `mkdocs.yml`.
- finally, open cmd in this `xyz/` folder and run command `mkdocs build` .This will generate new content in the `site/` folder based on your content in `docs` folder and `mkdocs.yml` .**ALWAYS ENSURE THAT YOUR CONTENT IS IN THE CORRECT FORMAT BEFORE RUNNING THIS COMMAND .I.E"**  
```
xyz/
  - site/
      -  ... //all the git and website stuff
	  
  - docs/
  - readme.md
  - mkdocs.yml
```

- Now simply put back those folders in the site folder and you can simply do `git add.` , `git commit -m "message"` and `git push origin gh-pages` to upload the site back on github's gh-pages :)
