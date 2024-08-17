![Version](https://img.shields.io/static/v1?label=annotated-bibliography-org&message=0.1&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# Template for making annotated bibliography in org-mode

An annotated bibliography summarizes notes about papers being read during a research project.
It is one of several methods for working with the knowledge gleaned from reading.

Org-mode is an enhanced markup language that supports the writing of scientific manuscripts.
Org-mode descended from Outline-mode. 
It supports managing documents with a hierarchical structure.
After folding the sections, subtrees of the document are easy to shuffle using the Meta-UP or Meta-Down keys.
This feature eases the reorganizing of a doc.
It was developed for use with the Emacs text editor.

The summary is stored in the annote field in the BibTeX entry in the BibTeX file, like in the example below:

![Screenshot 2024-08-17 at 1 36 04 PM](https://github.com/user-attachments/assets/447df2ef-bf02-49bc-a86f-ad3ee843e233)


This file was opened in Emacs.
Emacs provided the line numbers and syntax highlighting (font locking in Emacs parlance).

When exported to a PDF, the org file reads the BibTeX file with formatting set by the *apacannx.bst* file. 
The top of the output PDF looks like the following:


![Screenshot 2024-08-17 at 1 30 54 PM](https://github.com/user-attachments/assets/eb5d5ee9-b110-4c37-b8ea-0a013a7529f4)

I have also attached an Emacs configuration file (*init.el*) for working with manuscripts and annotated bibliographies in org-mode on macOS.
This configuration includes the Vertico-Orderless-Corfu-Cape-Consult-Embark auto-completion stack, org-ref for managing bibliographies, and pdf-tools for viewing PDFs inside Emacs.
This configuration depends on a handful of external packages.
The *init.el* file takes 2-3 seconds to load. 
You will have to edit some file paths.

I swamped the *option* and *command keys* because I use the command key for the Meta key.
I did not include org-agenda or org-roam.

## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)

## Update history

|Version      | Changes                                                                                                                                    | Date                 |
|:-----------:|:------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------:|
| Version 0.1 |   Initial commit. Added badges, funding, and update table.                                                                                 | 2024 August 17        |
