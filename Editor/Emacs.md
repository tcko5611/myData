# Setting keys
1.  (global-set-key (kbd "M-g") 'goto-line)
2.  (global-set-key (kbd "\<f2>") 'set-mark-command)

# load library
``````lisp
(setq load-path (cons (expand-file-name "~/emacs/lisp") load-path)) (require 'markdown-mode)
(require 'markdown-mode)
``````
-   C-M-a
-   C-M-e move in a function
-   C-M-p
-   C-M-n move in a parenthesis
# Edit Commands
## Numeric Arguments

-   **Ctrl-u** n, repeat n times for the next command
# Gdb

-   use **set startup-with-shell off** to avoid invoke subprocess with shell
# Debug python

- pdb python -m pdb ...

# Change default size
```bash
in .Xdefaults
Emacs*geometry: 80x24
xrdb -load ~/.Xdefaults
```
# P4 and Xcsope problem
* there is a problem in P4 package(/remote/us01home19/ktc/download/p4), xcscope is in it, we need to change "process-kill-without-query" to "process-query-on-exit-flag" new function.
* **Question:** why xcscope package can't use standalone?