$ sphinx-build source docs/build/html
or  
$ `make html`  
or  
$ `omake`

```console
$ git add source/ docs/  
$ git commit -m "update"
$ ghp-import -m "comments" docs/build/html  
$ ghp-import -p docs/build/html
```
