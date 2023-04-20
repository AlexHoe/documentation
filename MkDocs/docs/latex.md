# LaTeX
## Basics 
### Classes
```tex
\documentclass[options]{article}
```
**Basic classes**
- article: short documents without chapters
- report: longer documents with chapters, single-side printing
- book: longer documents with chapters, double-side printing, with front- and back-matter (for example an index)
- letter: correspondence with no sections
- slides: for presentations 
- others: [www.ctan.org/topic/class](https://www.ctan.org/topic/class)

**Extended classes**
- scrartcl
- scrreprt
- scrbook
- scrlttr2

**Classes for presentations**
- beamer
- powerdot


### Body
```tex
\begin{document}
\maketitle
% This line is a comment
\end{document}
```
### preamble
'Documents Setup Section'
- Set font size and paper size
  ```tex
  \documentclass[12pt, letterpaper]{article}
  ```
- Page size and margins: [www.overleaf.com](https://www.overleaf.com/learn/latex/Page_size_and_margins)
- Load external packages
  ```tex
  \usepackage{graphicx}
  ```
- Title
  ```tex
  \title{My first LaTeX document}
  ```
- Author
  ```tex
  \author{Alex}
  ```
- Date
  ```tex
  \date{January 2023}
  \date{\today}
  ```

## Formatting 
### Global options
- twocolumn

### Text
- Bold `\textbf{...}`
- Italics `\textit{...}`
- Underline `\underline{...}`
- Emphasise `\emph{...}`

### Lists
- Unordered list
```tex
\begin{itemize}
    \item First item
    \item Second item
\end{itemize}
```
- Ordered list
```tex
\begin{enumerate}
    \item First item
    \item Second item
\end{enumerate}
```
- Descriptive lists
```tex
\begin{description}
\item[Dog:] member of the genus Canis, which forms part of the wolf-like canids,
  and is the most widely abundant terrestrial carnivore.
\item[Cat:] domestic species of small carnivorous mammal. It is the only
  domesticated species in the family Felidae and is often referred to as the
  domestic cat to distinguish it from the wild members of the family.
\end{description}
```

### Tables
Available column types: 

| type            | description                                                                                                                                   |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| l               | left aligned column                                                                                                                           |
| c               | centered column                                                                                                                               |
| r               | right aligned column                                                                                                                          |
| p{width}        | a column with fixed width; the text will be automatically line wrapped and fully justified                                                    |
| m{width}        | like p, but vertically centered compared to the rest of the row                                                                               |
| w{align}{width} | prints the contents with a fixed width, silently overprinting if things get larger. You can choose the horizontal alignment using l, c, or r. |
| W{align}{width} | like w, but this will issue an overfull box warning if things get too wide.                                                                   |                                                                                                         

Special options:

| type            | description                                                                  |
| --------------- | ---------------------------------------------------------------------------- |
| \*{num}{string} | repeats string for num times in the preamble                                 |
| >{decl}         | this will put decl before the contents of every cell in the following column |
| <{decl}         | this will pul decl after the contents of each cell in the previous column    |
| \|              | add a vertical rule                                                          |
| @{decl}         | replace the space between two columns with decl                              |
| !{decl}         | add decl in the center of the existing space                                                                             |

To add more functionality add `\userpackage{array}`
```tex
\begin{tabular}{c c c}
  cell1 & cell2 & cell3 \\
  cell4 & cell5 & cell6 \\
  cell7 & cell8 & cell9
\end{tabular}
```

#### Rules (lines)
- to add horizontal rules, above and below rows, use the `\hline` command
- to add vertical rules, between columns, use the vertical line parameter `|`
```tex
\begin{tabular}{c|c|c}
  \hline
  cell1 & cell2 & cell3 \\
  cell4 & cell5 & cell6 \\
  cell7 & cell8 & cell9
  \hline
\end{tabular}
```
- for more advanced rules use the extension `\usepackage{booktabs}`
```tex
\begin{tabular}{lll}
  \toprule
  Animal & Food  & Size   \\
  \midrule
  dog    & meat  & medium \\
  horse  & hay   & large  \\
  frog   & flies & small  \\
  \bottomrule
\end{tabular}
```

## Document structure
### Abstracts 
Scientific articles usually provide an abstract which is a brief overview of their topics
```tex
\begin{abstract}
...
\end{abstract}
```
### Paragraphs and new lines
- a new paragraph is created by pressing the **enter** key twice. (blank line)
- a new line can be inserted by using `\\` or `\newline`

### Chapters and sections (not available for all classes)
```tex
\chapter{First Chapter}
\section{Introduction}
\subsection{Subsection}
\subsubsection{Subsubsection}
\section*{Unnumbered Section}
\end{itemsize}
```
### Table of Contents
`\tablecontents`

## Extension
### Localisation
```tex
\usepackage[german]{babel}
```
### Design
```tex 
\usepackage[margin=1in]{geometry}
```
