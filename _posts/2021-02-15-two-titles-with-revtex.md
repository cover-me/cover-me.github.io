---
layout: post
title: Two Titles with REVTeX 4
---

I want to combine the main text and supplementary in a single TEX file. I find it not easy to have two titles in a single TEX file. I can not use `\maketitle` twice. I don't find a good solution after searching (using the `titling` package changes the style, `\@author` does not return the author list.). But I find the reason for `\maketitle` not working twice: titles, authors, even the command itself are cleared (which is not necessary nowadays) to save the memory used (Ref. 1). Then I look at the code snippet in REVTeX 4 ([link](http://tph.tuwien.ac.at/~svozil/publ/revtex4.pdf)):

```latex
% \maketitle
% Put it all together to format the title block.
% Note: using \@tempcnta and \@tempa to communicate between procedures.
765 \def\maketitle{%
766 % \say\@authors
767 \@author@finish
768 \title@column\titleblock@produce
769 \suppressfloats[t]%
% Now save some memory.
770 \let\and\relax
771 \let\affiliation\@gobble@opt@one
772 % \let\address\affiliation
773 \let\author\@gobble
774 \@author@init
775 \let\@authors\@empty
776 \let\@authors@curr\@empty
777 \let\@affil@list\@empty
778 \let\keywords\@gobble
779 \let\@keywords\@empty
780 \let\email\@gobble
781 \let\@address\@empty
782 \let\maketitle\relax
783 \let\thanks\@gobble
784 \titlepage@sw{%
785 \clearpage
786 }{}%
787 }%

```

It works if we redefine the `\maketitle` by removing lines after `% Now save some memory`.

```latex
\documentclass[twocolumn,superscriptaddress,amsmath,amssymb]{revtex4-2}

% Redefine \maketitle so that it can be used twice (for supplementary)
\makeatletter
\def\maketitle{
\@author@finish
\title@column\titleblock@produce
\suppressfloats[t]}
\makeatother

\begin{document}
\title{Particle Garden}

\author{Majorana zero mode}
\affiliation{Anyon,low dimensional quasi-particle}

\author{Composite fermion}
\affiliation{Anyon,low dimensional quasi-particle}

\author{Cooper Pair}
\email{cp@superconductor}
\affiliation{Low temperature}
\affiliation{High pressure}

\maketitle

\title{Supplementary information: Particle Garden}
\maketitle

\end{document}
```

![](/images/twotitles.png)

Note that there are two emails in the footnote. It's not something difficult to deal with.

[Ref. 1] https://tex.stackexchange.com/questions/330358/maketitle-not-printing-twice
