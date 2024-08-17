![Version](https://img.shields.io/static/v1?label=doe-emofat&message=0.1&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# Template for making annotated bibliography in org-mode

An annotated bibliography is used to summarize notes about papers being read during a research project.
Org-mode is an enhanced markup language that supports the writing of scientific manuscripts.
It was developed for use with the Emacs text editor.

The summary is stored in the annote field in the BibTeX entry in the BibTeX file, like in the example below.

![Screenshot 2024-08-17 at 1 30 54 PM](https://github.com/user-attachments/assets/719987b5-5324-48bf-97f4-d8ec172d8c6a)

This file was opened in Emacs.
Emacs provided the line numbers and syntax highlighting (font locking in Emacs parlance).

When exported to a PDF, the org file reads the BibTeX file with formatting set by the *apacannx.bst* file. 
The top of the output PDF looks like the following:


![Screenshot 2024-08-17 at 1 30 54 PM](https://github.com/user-attachments/assets/eb5d5ee9-b110-4c37-b8ea-0a013a7529f4)

I have also attached an Emacs configuration file (*init.el*) for working with manuscripts and annotated bibliographies in org-mode on macOS.
This configuration includes the Vertico auto-completion stack and org-ref for managing bibliographies.
It supports the use of pdf-tools for viewing PDFs inside of Emacs.

## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)

## Update history

|Version      | Changes                                                                                                                                    | Date                 |
|:-----------:|:------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------:|
| Version 0.1 |   Initial commit. Added badges, funding, and update table.                                                                                 | 2024 August 17        |
