![Version](https://img.shields.io/static/v1?label=annotated-bibliography-org&message=0.3&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# Template for making annotated bibliography in org-mode with BibTeX

## What is this?
An annotated bibliography summarizes notes about papers being read during a research project.
It is one of several methods for working with the knowledge gleaned from reading.

## Why org-mode
Org-mode is an enhanced markup language that supports the writing of scientific manuscripts.
Org-mode descended from Outline-mode. 
It supports managing documents with a hierarchical structure.
After folding the sections, subtrees of the document are easy to shuffle using the Meta-UP or Meta-Down keys.
This feature eases the reorganizing of a doc.
It was developed for use with the Emacs text editor.

## Annote field in BibTeX entry
The summary is stored in the annote field in the BibTeX entry in the BibTeX file, like in the example below:

![Screenshot 2024-08-17 at 1 36 04 PM](https://github.com/user-attachments/assets/447df2ef-bf02-49bc-a86f-ad3ee843e233)


This file was opened in Emacs.
Emacs provided the line numbers and syntax highlighting (font locking in Emacs parlance).

## PDF of Annotated Bibliography
When exported to a PDF, the org file reads the BibTeX file with formatting set by the *apacannx.bst* file. 
The top of the output PDF looks like the following:


![Screenshot 2024-08-17 at 1 30 54 PM](https://github.com/user-attachments/assets/eb5d5ee9-b110-4c37-b8ea-0a013a7529f4)

I have also attached an Emacs configuration file (*init.el*) for working with manuscripts and annotated bibliographies in org-mode on macOS.
This configuration includes the Vertico-Orderless-Corfu-Cape-Consult-Embark auto-completion stack, org-ref for managing bibliographies, and pdf-tools for viewing PDFs inside Emacs.
This configuration depends on a handful of external packages.
The *init.el* file takes 2-3 seconds to load. 
You will have to edit some file paths.

I swapped the *option* and *command keys* because I use the command key for the Meta key.
I did not include org-agenda or org-roam.


## Bash Function to generate subfolder with required files

Edit the file paths as needed.
Takes a project ID as the only argument.
Run from the top level of the writing project directory.

```bash
function abiborg {
echo "Create annotated bibliopgraphy subfolder and populate with required files with project number in title."
if [ $# -lt 1 ]; then
  echo 1>&2 "$0: not enough arguments"
  echo "Usage1: abiborg projectIndexNumber"
  return 2
elif [ $# -gt 1 ]; then
  echo 1>&2 "$0: too many projectIndexNumber"
  echo "Usage1: abiborg projectIndexNumber"
  return 2
fi
projectID="$1"
mkdir abib$1
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/compile.sh ./abib$1/.
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/apacannx.bst ./abib$1/.
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/ab0519.bib ./abib$1/ab$1.bib
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/ab0519.org ./abib$1/ab$1.org
}


function orgabib {
echo "Create annotated bibliopgraphy subfolder and populate with required files with project number in title."
if [ $# -lt 1 ]; then
  echo 1>&2 "$0: not enough arguments"
  echo "Usage1: orgabib projectDirectory"
  return 2
elif [ $# -gt 1 ]; then
  echo 1>&2 "$0: too many projectIndexNumber"
  echo "Usage1: orgabib projectIndexNumber"
  return 2
fi
projectID="$1"
mkdir abib$1
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/compile.sh ./abib$1/.
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/apacannx.bst ./abib$1/.
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/ab0519.bib ./abib$1/ab$1.bib
cp ~/6112MooersLabGitHubLabRepos/annotated-bibliography-org/ab0519.org ./abib$1/ab$1.org
}

```


## Installation

1. git clone this project to your software directory
2. Copy one of the bash function and paste into your .bashr or .zshrc file.
3. source .bashrc
4. cd project directory
3. orgabib <projectID> to create subfolder 


## Convenience function to wrap the insertion citekey

```elisp
(defun mooerslab-wrap-citar-citekey-and-create-abibnote-org ()
    "Replace the citekey under the cursor with LaTeX-wrapped text and create a 
    corresponding empty citekey.org file in abibNotes folder in the home directory.
    Will work with citekeys in citar style or in LaTeX style or plain naked citekeys.
    The LaTeX code uses the bibentry package to inject a bibliographic entry into 
    a section heading that is added in the table of contents. The function also adds
    file links to the PDF and org note files in a comment block for quick access."

    (interactive)
    (let* ((bounds (or (save-excursion
                      ;; Check if we're inside a citation
                      (let ((start (re-search-backward "\\[cite:@" (line-beginning-position) t))
                            (end (re-search-forward "\\]" (line-end-position) t)))
                        (when (and start end)
                          (cons start end))))
                    ;; Otherwise just get the word at point
                    (bounds-of-thing-at-point 'word)))
         (citation-text (when bounds 
                          (buffer-substring-no-properties (car bounds) (cdr bounds))))
         (citekey (when citation-text
                    (if (string-match "\\[cite:@\\([^]]+\\)\\]" citation-text)
                        (match-string 1 citation-text)
                      citation-text))) ;; Plain word if not a citation
  
         ;; Extract directory from current buffer filename
         (current-file (buffer-file-name))
         (current-dir (when current-file (file-name-directory current-file)))
  
         ;; Try to determine default project number from filename
         (default-project-number 
          (cond 
           ;; First try to find "ab2156" pattern in the buffer file name
           ((and current-file 
                 (string-match "ab\\([0-9]+\\).org" (file-name-nondirectory current-file)))
            (match-string 1 current-file))
     
           ;; Look for "2156" in the buffer file name
           ((and current-file 
                 (string-match "\\([0-9]+\\)" (file-name-nondirectory current-file)))
            (match-string 1 current-file))
     
           ;; Default to empty string
           (t "")))
  
         ;; Prompt the user for project number with default from filename
         (project-number (read-string (format "Project number for BibTeX file [%s]: " 
                                             default-project-number)
                                     nil nil default-project-number))
  
         ;; Construct file paths
         (bib-file-name (concat "ab" project-number ".bib"))
         (bib-file-path (and current-dir (concat current-dir bib-file-name)))
         (org-file-dir "/Users/blaine/abibNotes/") ;; Directory for the .org file
         (org-file-path (and citekey (concat org-file-dir citekey ".org"))) ;; Full path for the .org file
         
         ;; Updated wrapped text with file links inside a comment block
         (wrapped-text (and citekey 
                           (format "#+LATEX: \\subsubsection*{\\bibentry{%s}}\n#+LATEX: \\addcontentsline{toc}{subsubsection}{%s}\n#+INCLUDE: %s\n#+BEGIN_COMMENT\nfile:~/abibNotes/%s.org\nfile:~/0papersLabeled/%s.pdf\n#+END_COMMENT"
                                  citekey citekey org-file-path citekey citekey))))

    ;; Debug message to check file paths
    (message "Using bibfile: %s" bib-file-path)

    (if (not citekey)
        (message "No citekey found under the cursor.")
      (progn
        ;; Delete the citation or word at cursor
        (when bounds
          (delete-region (car bounds) (cdr bounds)))
        ;; Insert the wrapped text in its place
        (insert wrapped-text)
 
        ;; Create a minimal .org file if it doesn't already exist - no header, no headline
        (if (file-exists-p org-file-path)
            (message "File %s already exists." org-file-path)
          (with-temp-file org-file-path
            (insert ""))) ;; Empty file
 
        ;; Append the BibTeX entry to the project-specific .bib file in current directory
        (require 'bibtex)
        (when (and (featurep 'citar) current-dir bib-file-path)  ;; Ensure we have all required paths
          ;; Get the bibtex entry using search in citar bibliography files
          (let* ((bib-files (citar--bibliography-files))
                 (bibtex-entry nil))
     
            ;; Look through each bibliography file for the entry
            (when bib-files
              (catch 'found
                (dolist (bib-file bib-files)
                  (with-temp-buffer
                    (insert-file-contents bib-file)
                    (bibtex-mode)
                    (bibtex-set-dialect 'BibTeX t)
                    (goto-char (point-min))
                    (when (re-search-forward (format "@[^{]+{%s," citekey) nil t)
                      (let ((beg (save-excursion 
                                   (bibtex-beginning-of-entry)
                                   (point)))
                            (end (save-excursion
                                   (bibtex-end-of-entry)
                                   (point))))
                        (setq bibtex-entry (buffer-substring-no-properties beg end))
                        (throw 'found t)))))))
     
            (if bibtex-entry
                (progn
                  ;; Now append to the bib file
                  (with-temp-file bib-file-path
                    (when (file-exists-p bib-file-path)
                      (insert-file-contents bib-file-path))
                    (goto-char (point-max))
                    (unless (or (bobp) (bolp)) (insert "\n"))
                    (insert bibtex-entry "\n\n"))
                  (message "Added BibTeX entry to %s" bib-file-path))
              (message "Could not retrieve BibTeX entry for %s" citekey))))
 
        ;; Open the .org file in a new buffer
        (find-file org-file-path)
        (message "Replaced citekey, created .org file, and opened it: %s" org-file-path)))))
```

This function replaces the citekey (e.g., Schaefer2021SEQCROW) with the following:

```elisp
#+LATEX: \subsection*{\bibentry{Schaefer2021SEQCROW}}
#+LATEX: \addcontentsline{toc}{subsubsection}{Schaefer2021SEQCROW}
#+INCLUDE: /Users/blaine/abibNotes/Schaefer2021SEQCROW.org
#+BEGIN_COMMENT
file:~/abibNotes/Schaefer2021SEQCROW.org
file:~/0papersLabeled/Schaefer2021SEQCROW.pdf
#+END_COMMENT
```

The links in the comment are not exported to the PDF.

## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)

## Update history

| Version          |  Changes                                                                                                            | Date                      |
|:-----------------|:--------------------------------------------------------------------------------------------------------------------|:--------------------------| 
|  0.1             |   Initial commit. Added badges, funding, and update table.                                                          | 2024 August 17            |
| 0.2              |   Updated compile.sh to take ProjectNumber as argument. Add bash function to ease install.                          | 2024  October 1           |
| 0.3              |   Added to header code to add short author list, running title, date printed, and Page of N pages.                  | 2024  October 24          |

