
## 一个简单的python开发配置
```
;; init.el --- Emacs configuration

;; INSTALL PACKAGES
;; --------------------------------------

(require 'package)

(add-to-list 'package-archives
       '("melpa" . "http://melpa.org/packages/") t)

(package-initialize)
(when (not package-archive-contents)
  (package-refresh-contents))

(defvar myPackages
  '(better-defaults
    elpy
    material-theme))

(mapc #'(lambda (package)
    (unless (package-installed-p package)
      (package-install package)))
      myPackages)


;; BASIC CUSTOMIZATION
;; --------------------------------------

(setq inhibit-startup-message t) ;; hide the startup message
(load-theme 'material t) ;; load material theme
(global-linum-mode t) ;; enable line numbers globally
;; python
(elpy-enable)
;; pyenv
(global-set-key (kbd "C-x p") 'pyvenv-activate)
;; autofix by pep8
(global-set-key (kbd "C-x M-f") 'elpy-autopep8-fix-code)
(setq elpy-rpc-backend "jedi")
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(package-selected-packages
   (quote
    (magit js2-mode 2048-game material-theme elpy better-defaults))))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )
;; enable pair paren
(electric-pair-mode 1)

;; git
(global-set-key (kbd "C-x g") 'magit-status)
;; init.el ends here
```

## 使用自定义模版
yasnippet可以把我们常用的代码段或文本储存起来，到使用的时候只需键入几个字母就会自动带出。

比如我们在写python代码时，常常会在文件的第一行写下： #!/usr/bin/env python，经常这么手工键入是不是很没有效率，用yasnippet来帮你！

方法：
一、新建snippet
使用命令 M-x yas-new-snippet 打开一个新buffer，或者直接新建一个文件，输入内容后保存到你指定的位置即可。

用M-x yas-new-snippet 打开的buffer内容默认如下：
```
# -*- mode: snippet; require-final-newline: nil -*-
# name:
# key:
# binding: direct-keybinding
# –
```
我们将它修改为适合python开发的模版
```
# name: file header
# key: fh
# binding: direct-keybinding
# --
#!/usr/bin/env python
# -*- coding: utf-8 -*-

$0

if __name__ == '__main__':
   pass
```
解释下

name: file header 这是片段的名称，叫做 file header，这个 file header会显示在emacs菜单栏的yasnippet里。

key: fh 这是快捷键，在文件里输入fh后按tab键就会展开这个片段。

从#-- 以下输入代码片段就好了

Filename:`(file-name-nondirectory buffer-file-name)` 这个有意思了，这个是显示当前buffer的名字的，让emacs帮你自动写。


## elpy技巧

使用elpy之前先安装好环境
```
# Either of these
pip install rope
pip install jedi
# flake8 for code checks
pip install flake8
# importmagic for automatic imports
pip install importmagic
# and autopep8 for automatic PEP8 formatting
pip install autopep8
# and yapf for code formatting
pip install yapf
```
下面是一些常用命令
```
M-x elpy-config # 查看配置情况
# 激活／关闭虚拟环境
M-x pyvenv-activate
M-x pyvenv-deactivate
M-x elpy-autopep8-fix-code # 自动用pep8修复代码格式
```
更多命令查看文档https://elpy.readthedocs.io/en/latest/index.html




