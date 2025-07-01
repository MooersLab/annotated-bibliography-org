![Version](https://img.shields.io/static/v1?label=annotated-bibliography-org&message=0.3&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# Template for Classic Annotated Bibliography in Org-mode with BibTeX

## What is this?
An annotated bibliography summarizes notes about papers being read during a research project.
It is one of several methods for applying the knowledge gained from reading.

## Why org-mode
Org-mode is an enhanced markup language that supports the writing of scientific manuscripts.
Org-mode descended from Outline-mode. 
It supports managing documents with a hierarchical structure.
After folding the sections, subtrees of the document are easily shuffled using the Meta-UP or Meta-Down keys.
This feature eases the reorganization of the annotated bibliography.
It was developed for use with the Emacs text editor.

## Annote field in BibTeX entry
The summary is stored in the `annote` field in the BibTeX entry in the BibTeX file, like in the example below:

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

I swapped the option and command keys because I use the command key as the Meta key.
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
echo "Create annotated bibliography subfolder and populate with required files with project number in title."
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
2. Copy one of the bash function and paste into your `.bashrc` or `.zshrc` file.
3. source .bashrc
4. cd project directory
3. orgabib <projectID> to create subfolder 

## Related projects

Having to add notes to the note field is extremely limiting.
The LaTeX software will strip out any white spaces between paragraphs.
You have to go to extreme measures to enclose the paragraphs in some LaTeX code.
You can insert equations, and you can include figures with difficulty.

To ease the writing of annotations that contain

- multiple paragraphs
- big blocks of equations
- figures
- tables
- code listings
- interactive code blocks for literate programming

I am developing a modular and illustrated approach to the annotated bibliography.
In this approach, annotations are stored in Org-mode files that are imported into the final document upon compilation to PDF.
These files can utilize either pure Org-mode syntax or LaTeX to add graphical features.
The graphical features are very effective at stimulating the recall of a paper's content much faster than a wall of text.

This modular approach is independent of the annote field.
The annote field no longer needs to be utilized for storing the notes in global.bib file.
This modular approach enables the reuse of note files across multiple annotated bibliographies for various writing projects.

I plan to make a variant that uses tex files for use in pure LaTeX.
This approach also supports the management of annotated bibliographies using Rmarkdown.

## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)

## Update history

| Version          |  Changes                                                                                                            | Date                      |
|:-----------------|:--------------------------------------------------------------------------------------------------------------------|:--------------------------| 
|  0.1             |   Initial commit. Added badges, funding, and updated the table.                                                     | 2024 August 17            |
| 0.2              |   Updated compile.sh to take ProjectNumber as argument. Add bash function to ease install.                          | 2024  October 1           |
| 0.3              |   Added to header code to add short author list, running title, date printed, and Page of N pages.                  | 2024  October 24          |

