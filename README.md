# auto-fix.el

Fix current buffer automatically

![Capture](https://raw.githubusercontent.com/tomoya/auto-fix.el/master/images/capture_20190120131817.gif)

## Variables

This package have 2 important buffer local variables `auto-fix-command` and `auto-fix-option`. Please let you set using the hook.

### auto-fix-command

This is the command to fix code. Default value is `nil`.

### auto-fix-option

This is the option string to fix for the command. Default value is `--fix`.

### auto-fix-temp-file-prefix

This is the prefix for temprary file. Default value is `auto_fix_`.

## Setup

To enable auto-fix before saving add the following to your init file:

### For Ruby

```lisp
(add-hook 'auto-fix-mode-hook
          (lambda () (add-hook 'before-save-hook #'auto-fix-before-save)))

(defun setup-ruby-auto-fix ()
  (setq-local auto-fix-command "rubocop")
  (setq-local auto-fix-option "-a")
  (auto-fix-mode +1))

(add-hook 'ruby-mode-hook #'setup-ruby-auto-fix)
```

### For TypeScript

```lisp
(add-hook 'auto-fix-mode-hook
          (lambda () (add-hook 'before-save-hook #'auto-fix-before-save)))

(defun setup-ts-auto-fix ()
  (setq-local auto-fix-command "tslint")
  (auto-fix-mode +1))

(add-hook 'typescript-mode-hook #'setup-ts-auto-fix)
```

### Combination with flycheck

```lisp
;; Flycheck using project linter
(defun my-use-local-lint ()
  "Use local lint if exist it."
  (let* ((root (locate-dominating-file
                (or (buffer-file-name) default-directory) "node_modules"))
         (tslint (and root (expand-file-name "node_modules/.bin/tslint" root))))
    (when (and tslint (file-executable-p tslint))
      (setq-local flycheck-typescript-tslint-executable tslint)
      (setq-local auto-fix-command tslint))))

(add-hook 'flycheck-mode-hook #'my-use-local-lint)

;; auto fix
(add-hook 'auto-fix-mode-hook
          (lambda () (add-hook 'before-save-hook #'auto-fix-before-save)))

(add-hook 'typescript-mode-hook #'auto-fix-mode)
```
