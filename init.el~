;; Do some things for MELPA (http://melpa.org/#/getting-started).
(require 'package)
(let* ((no-ssl (and (memq system-type '(windows-nt ms-dos))
                    (not (gnutls-available-p))))
       (proto (if no-ssl "http" "https")))
  (when no-ssl
    (warn "\
Your version of Emacs does not support SSL connections,
which is unsafe because it allows man-in-the-middle attacks.
There are two things you can do about this warning:
1. Install an Emacs version that does support SSL and be safe.
2. Remove this warning from your init file so you won't see it again."))
  ;; Comment/uncomment these two lines to enable/disable MELPA and MELPA Stable as desired
  (add-to-list 'package-archives (cons "melpa" (concat proto "://melpa.org/packages/")) t)
  ;;(add-to-list 'package-archives (cons "melpa-stable"
  ;;  (concat proto "://stable.melpa.org/packages/")) t)
  (when (< emacs-major-version 24)
    ;; For important compatibility libraries like cl-lib
    (add-to-list 'package-archives (cons "gnu" (concat proto "://elpa.gnu.org/packages/")))))

;; Added by Package.el.  This must come before configurations of
;; installed packages.  Don't delete this line.  If you don't want it,
;; just comment it out by adding a semicolon to the start of the line.
;; You may delete these explanatory comments.
(package-initialize)

;; A lot of the following calls were taken from https://www.youtube.com/watch?v=HTUE03LnaXA

;; Start yasnippet.
(require 'yasnippet)
(yas-global-mode 1)

;; Start auto-complete.
;; (require 'auto-complete)

;; auto-complete default config.
;; (require 'auto-complete-config)
;; (ac-config-default)

;; Start auto-complete-c-headers
;; (defun ac-c-header-init ()
;;   (require 'auto-complete-c-headers)
;;   (add-to-list 'ac-sources 'ac-sources-c-headers)
;;   )

;; (add-hook 'c++-mode-hook 'ac-c-header-init)
;; (add-hook 'c-mode-hook 'ac-c-header-init)

;; Start irony.
(require 'irony)

;; Configure irony-mode (C/C++ code completion)
(add-hook 'c++-mode-hook 'irony-mode)
(add-hook 'c-mode-hook 'irony-mode)
(add-hook 'objc-mode-hook 'irony-mode)

(add-hook 'irony-mode-hook 'irony-cdb-autosetup-compile-options)

;; Configure company
(add-hook 'c++-mode-hook 'company-mode)
(add-hook 'c-mode-hook 'company-mode)
(setq company-dabbrev-downcase nil) ;; Enforce case-sensitivity.
(setq company-idle-delay 0) ;; Make company immediately give suggestions.

;; Configure company-irony
(require 'company-irony)
(eval-after-load 'company
  '(add-to-list 'company-backends 'company-irony))

;; Configure jedi
;; (add-hook 'python-mode-hook 'jedi:setup)
;; (setq jedi:complete-on-dot t)

;; Configure elpy
(require 'elpy)
(require 'jedi)
;; (add-hook 'ein:connect-mode-hook 'ein:jedi-setup)

;;(add-to-list 'load-path "/home/tyler/.emacs.d/elpa/elpy-20191024.2007")

;; Configure company-AUCTeX
;; (add-to-list 'load-path "/home/tyler/.emacs.d/elpa/company-auctex-20180725.1912/company-auctex.el")
(require 'company-auctex)
(add-hook 'LaTeX-mode-hook 'company-mode)
(company-auctex-init)
(define-key LaTeX-mode-map (kbd "C-c h") 'latex-help)
;; (eval-after-load '
;;   '(progn
;;      (define-key LaTeX-mode-map (kbd "C-c h") 'latex-help)))

;; Configure pdf-tools
(require 'pdf-tools)
(pdf-tools-install)
;; With LaTeX magic
(setq TeX-view-program-selection '((output-pdf "PDF Tools"))
      TeX-source-correlate-start-server t)
(add-hook 'TeX-after-compilation-finished-functions
	  #'TeX-revert-document-buffer)
;; (add-hook 'after-save-hook
;; 	  (lambda ()
;; 	    (when (string= major-mode 'latex-mode)
;; 	      (TeX-run-latexmk
;; 	       "LaTeX"
;; 	       (format "latexmk -xelatex %s" (buffer-file-name))
;; 	       (file-name-base (buffer-file-name))))))

;; Configure emacs ipython notebook
(require 'ein)
;;(require 'ein-mumamo)
(setq ein:use-auto-complete t)
(setq ein:use-smartrep t)
;;(setq ein:completion-backend 'ein:use-ac-jedi-backend)
(setq ein:completion-backend 'ein:use-company-backend)

;; Set theme to deeper-blue, but only in GUI mode
(if (display-graphic-p)
    (load-theme 'deeper-blue 1)
  nil)

;; Configure visual-regexp-steriods
(require 'visual-regexp-steroids)
(global-set-key (kbd "C-c r") 'vr/replace)
(global-set-key (kbd "C-c q") 'vr/query-replace)
;; (global-set-key (kbd "C-c a") 'vr/
(define-key esc-map (kbd "C-r") 'vr/isearch-backward)
(define-key esc-map (kbd "C-s") 'vr/isearch-forward)

;; Configure omnisharp (for .NET Core)
(package-install 'omnisharp)
;; (add-hook 'csharp-mode-hook 'omnisharp-mode)
;; The following was copied directly from the README at
;; https://github.com/OmniSharp/omnisharp-emacs
(eval-after-load
    'company
  '(add-to-list 'company-backends #'company-omnisharp))
(defun my-csharp-mode-setup ()
  (omnisharp-mode)
  (company-mode)
  (flycheck-mode)

  (setq indent-tabs-mode nil)
  (setq c-syntactic-indentation t)
  (c-set-style "ellemtel")
  (setq c-basic-offset 4)
  (setq truncate-lines t)
  (setq tab-width 4)
  (setq evil-shift-width 4)

  ;csharp-mode README.md recommends this too
  (electric-pair-local-mode 1)

  (local-set-key (kbd "C-c r r") 'omnisharp-run-code-action-refactoring)
  (local-set-key (kbd "C-c C-c") 'recompile))
(add-hook 'csharp-mode-hook 'my-csharp-mode-setup t)

;; Configure racket-mode
(put 'type-case 'racket-indent-function 2)

(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(ein:polymode t)
 '(package-selected-packages
   (quote
    (omnisharp visual-regexp-steroids ein-mumamo elpy ein racket-mode org-pdfview pdf-tools jedi company-auctex auctex cmake-mode flycheck-irony irony-eldoc yasnippet company-irony company-irony-c-headers irony))))

(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )

;;------------------------
;; Start the customization
;;------------------------

;; Change the emacs clipboard to the real clipboard.

(defun copy-to-clipboard ()
  (interactive)
  (if (display-graphic-p)
      (progn
        (message "Yanked region to x-clipboard!")
        (call-interactively 'clipboard-kill-ring-save)
        )
    (if (region-active-p)
        (progn
          (shell-command-on-region (region-beginning) (region-end) "xsel -i -b")
          (message "Yanked region to clipboard!")
          (deactivate-mark))
      (message "No region active; can't yank to clipboard!")))
  )

(defun cut-to-clipboard ()
  (interactive)
  (if (display-graphic-p)
      (progn
        (message "Yanked region to x-clipboard!")
        (call-interactively 'clipboard-kill-ring-save)
        )
    (if (region-active-p)
        (progn
          (shell-command-on-region (region-beginning) (region-end) "xsel -i -b")
          (message "Yanked region to clipboard!")
	  (delete-region (region-beginning) (region-end))
          (deactivate-mark))
      (message "No region active; can't yank to clipboard!")))
  )

(defun paste-from-clipboard ()
  (interactive)
  (if (display-graphic-p)
      (progn
        (clipboard-yank)
        (message "graphics active")
        )
    (insert (shell-command-to-string "xsel -o -b"))
    )
  )

(defun delete-word (arg)
  "Kill characters forward until encountering the end of a word.
With argument ARG, do this that many times."
  (interactive "p")
  (delete-region (point) (progn (forward-word arg) (point))))

(defun backward-delete-word (arg)
  "Kill characters forward until encountering the end of a word.
With argument ARG, do this that many times."
  (interactive "p")
  (delete-word (- arg)))

(global-set-key (kbd "C-w") 'cut-to-clipboard)
(global-set-key (kbd "M-w") 'copy-to-clipboard)
(global-set-key (kbd "C-y") 'paste-from-clipboard)
(global-set-key (kbd "C-<backspace>") 'backward-delete-word)
(global-set-key (kbd "M-DEL") 'backward-delete-word)
(global-set-key (kbd "M-d") 'delete-word)
;; (eval-after-load 'pdf-view
;; (add-hook 'pdf-view-mode-hook
;; 	  '(progn
;; 	     (local-set-key (kbd "C-w") (quote copy-to-clipboard))
;; 	     (local-set-key (kbd "M-w") (quote copy-to-clipboard))))


;; Change the cc-mode style.
(setq c-default-style "linux" c-basic-offset 4)

;; Make Shift-Enter start a new line without breaking the current.
(defun new-line-below ()
  (interactive)
  (move-end-of-line 1)
  (setq unread-command-events (listify-key-sequence "\r"))
  ;; (newline 1)
  )

(global-set-key (kbd "M-o m") 'new-line-below)
(global-set-key (kbd "<S-return>") 'new-line-below)

;; Make C-d delete the current line.
(defun delete-current-line ()
  (interactive)
  (move-beginning-of-line 1)
  (set-mark (point))
  (setq unread-command-events (listify-key-sequence "\C-n\d"))
  )

(global-set-key (kbd "C-d") 'delete-current-line)
(eval-after-load 'cc-mode
  '(progn
     (define-key c-mode-map (kbd "C-d") 'delete-current-line)
     (define-key c++-mode-map (kbd "C-d") 'delete-current-line)))

(eval-after-load 'csharp-mode
  '(progn
     (define-key csharp-mode-map (kbd "C-d") 'delete-current-line)))

;; lookup function
(defun my-lookup-key (key)
  "Search for KEY in all known keymaps."
  (mapatoms (lambda (ob) (when (and (boundp ob) (keymapp (symbol-value ob)))
                      (when (functionp (lookup-key (symbol-value ob) key))
                        (message "%S" ob))))
            obarray))

;; Show the column position in the modeline (grey bar at the bottom)
(column-number-mode 1)

;; Align with spaces only
(defadvice align-regexp (around align-regexp-with-spaces)
  "Never use tabs for alignment."
  (let ((indent-tabs-mode nil))
    ad-do-it))
(ad-activate 'align-regexp)

;; Setup tex compile "C-." hotkey for latex-mode
(defun latex-compile ()
  (interactive)
  (save-buffer)
  (TeX-command "LaTeX" 'TeX-master-file))
(eval-after-load 'latex
  '(define-key LaTeX-mode-map (kbd "C-.") 'latex-compile))



;; midnite mode hook
 (add-hook 'pdf-view-mode-hook (lambda ()
                                 (pdf-view-midnight-minor-mode))) ; automatically turns on midnight-mode for pdfs

(setq pdf-view-midnight-colors '("#ff9900" . "#0a0a12" )) ; set the amber profile as default (see below)

(defun bms/pdf-no-filter ()
  "View pdf without colour filter."
  (interactive)
  (pdf-view-midnight-minor-mode -1)
  )

;; change midnite mode colours functions
(defun bms/pdf-midnite-original ()
  "Set pdf-view-midnight-colors to original colours."
  (interactive)
  (setq pdf-view-midnight-colors '("#839496" . "#002b36" )) ; original values
  (pdf-view-midnight-minor-mode)
  )

(defun bms/pdf-midnite-amber ()
  "Set pdf-view-midnight-colors to amber on dark slate blue."
  (interactive)
  (setq pdf-view-midnight-colors '("#ff9900" . "#0a0a12" )) ; amber
  (pdf-view-midnight-minor-mode)
  )

(defun bms/pdf-midnite-green ()
  "Set pdf-view-midnight-colors to green on black."
  (interactive)
  (setq pdf-view-midnight-colors '("#00B800" . "#000000" )) ; green 
  (pdf-view-midnight-minor-mode)
  )

(defun bms/pdf-midnite-darkroom ()
  "Set pdf-view-midnight-colors to amber on dark slate blue."
  (interactive)
  (setq pdf-view-midnight-colors '("#df1200" . "#000000" )) ; darkroom
  (pdf-view-midnight-minor-mode)
  )

(defun bms/pdf-midnite-colour-schemes ()
  "Midnight mode colour schemes bound to keys"
        (local-set-key (kbd "!") (quote bms/pdf-no-filter))
        (local-set-key (kbd "@") (quote bms/pdf-midnite-amber)) 
        (local-set-key (kbd "#") (quote bms/pdf-midnite-green))
	(local-set-key (kbd "$") (quote bms/pdf-midnite-original))
	(local-set-key (kbd "%") (quote bms/pdf-midnite-darkroom))
 )  

(add-hook 'pdf-view-mode-hook 'bms/pdf-midnite-colour-schemes)

;; Make C-x the prefix command it term-mode (terminal mode)
(add-hook 'term-mode-hook
	  (lambda ()
	    (term-set-escape-char ?\C-x)
	    (define-key term-raw-map "\M-y" 'yank-pop)
	    (define-key term-raw-map "\M-y" 'kill-ring-save)))
