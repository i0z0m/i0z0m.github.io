# build by sphinx
$ `make html`  

or autobuild  
$ `make livehtml`



# deploy
```console
$ git add source/ docs/  
$ git commit -m "update"
$ ghp-import -m "comments" docs/build/html  
$ ghp-import -p docs/build/html
```
