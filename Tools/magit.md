---
date : 2022-05-12 13:41
aliases : []
priority : 2
---
# Metadata
Status :: #Status/Done  
Type :: #Prog/Info
Topics :: # Metadata
# Note
## hot keys
* ctrl-x g: bring up magit console
* tab: fold/unfold changed
* c - c: staged all modified files
* ctrl-c - ctrl-c: commit changed 
* ctrl-q: close help window (open by "h")
* : ls-files: show all trace files
## view diff current and commits
### current
```lisp
magit-diff-buffer-file
```
### commits
```lisp
mgit-log-buffer-file
```
will generate a commits buffer like:
```
1ba68c8 * master check in for move exprdoc to QAfData
9e976e2 * add gui for selec flow
5be8774 * remove don't use action, change GUI behavior
c62d88f * prototype for update PWDE
a87c2ba * seperate delete session into slot mechanism
fe6a54d * For select flow prtotype
e802735 * fisrt check in
```
key in 
```
E r 1ba68c8..9e976e2
```
## emacs setting
```lisp
(add-to-list 'load-path "~/lisp/dash")
(add-to-list 'load-path "/remote/us01home19/ktc/lisp/transient/lisp")
(add-to-list 'load-path "~/lisp/with-editor/lisp")
(add-to-list 'load-path "/remote/us01home19/ktc/lisp/compat.el")
(require 'transient)
(add-to-list 'load-path "/remote/us01home19/ktc/lisp/magit/lisp")
(require 'magit)
(global-set-key (kbd "C-c m =") 'magit-diff-buffer-file)
(global-set-key (kbd "C-c m l") 'magit-log-buffer-file)
```


