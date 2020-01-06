---
title: "latex"
date: 2019-09-10T16:35:49+08:00
lastmod: 2019-11-04T13:31:55+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [Lists](https://www.latex-tutorial.com/tutorials/lists/)

## [format table](https://www.overleaf.com/learn/latex/Tables#Reference_guide) 

> @{}suppresses the space between columns, that means after the preceding column and before the next. This way it affects also the space before the first column and after the last, if positioned there.

*NOTE*: `\\` is very important in latex, as it means new line like `\n`

## [Set text font size](http://www.sascha-frank.com/latex-font-size.html)

```
command	 font size
\tiny	5pt	6pt	6pt
\scriptsize	7pt	8pt	8pt
\footnotesize	8pt	9pt	10pt
\small	9pt	10pt	11pt
\normalsize	10pt	11pt	12pt
\large	12pt	12pt	14pt
\Large	14pt	14pt	17pt
\LARGE	17pt	17pt	20pt
\huge	20pt	20pt	25pt
\Huge	25pt	25pt	25pt
```

## How to add an empty page?

```
% add a empty page for notes
\clearpage
\thispagestyle{empty}
\phantom{a}
\vfill
\centering[This page intentionally left blank]
\vfill
```

## [Setting up LaTeX on Mac OS X](https://www.pydanny.com/setting-up-latex-on-mac-os-x.html)
>>1. Get mactex-basic.pkg from http://www.ctan.org/pkg/mactex-basic
>>2. Click mactex-basic.pkg to install LaTeX.
>>3. Update tlmgr: sudo tlmgr update --self
>>4. Install the following tools via tlmgr:
	sudo tlmgr install titlesec
	sudo tlmgr install framed
	sudo tlmgr install threeparttable
	sudo tlmgr install wrapfig
	sudo tlmgr install multirow
	sudo tlmgr install enumitem
	sudo tlmgr install bbding
	sudo tlmgr install titling
	sudo tlmgr install tabu
	sudo tlmgr install mdframed
	sudo tlmgr install tcolorbox
	sudo tlmgr install textpos
	sudo tlmgr install import
	sudo tlmgr install varwidth
	sudo tlmgr install needspace
	sudo tlmgr install tocloft
	sudo tlmgr install ntheorem
	sudo tlmgr install environ
	sudo tlmgr install trimspaces
>>5. Install fonts via tlmgr: sudo tlmgr install collection-fontsrecommended
>>6. For other package, install as needed: sudo tlmgr install ctex zhnumber

## [Install texlive quickly](https://tex.stackexchange.com/questions/342612/tex-live-installation-specify-just-the-scheme-on-the-command-line)

I used `./install-tl --repository http://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet --profile texlive.profile` after creating a sample profile and `selected_scheme scheme-small`

## [Install new packages with tlmgr](https://en.wikibooks.org/wiki/LaTeX/Installing_Extra_Packages)

Update tlmgr by `sudo tlmgr update --self`

Install some package by `sudo tlmgr install <packagename>`

**NOTE**:latex is case sensitive, so ctex is different than CTEX. Thus you might get error like *! LaTeX Error: File `CTEX.sty' not found* if you use wrong pacakge name as CTEX instead of ctex

## Examples to support Chinese font

```tex
\documentclass{article}
\usepackage{fontspec}
% something in "{}" is the package name, and that in "[]" is the options
\usepackage{ctex}[UTF8]

% provided by fontspec
\setmainfont{Times New Roman} [Scale=MatchLowercase]

% provided by ctex
\setCJKmainfont{Heiti SC Light} [BoldFont=Heiti SC Medium]
\setCJKsansfont{Heiti SC}
\setCJKmonofont{Courier}[Scale=MatchLowercase]
\begin{document}
\begin{table}[h]
	\begin{tabular}{c|c}
			Nanomaterials Synthesis/Characterization & C, Python, {\LaTeX}\\
			{\fangsong 物理} &  化学 \\
	\end{tabular}
\end{table}

\begin{minipage}{0.2\textwidth} % http://www.sascha-frank.com/latex-minipage.html
	\begin{tabular}{|c|c|c|}
% 	\begin{tabular}{c|c|c} % this will remove the left and right |
		%\hline
		A & B & C \\
		\hline
		{\kaishu 这里是楷体显示} & {\songti 这里是宋体显示} & {\heiti 这里是黑体显示}  \\
		\hline 
		4 & 5 & 6 \\
		%\hline
	\end{tabular}
\end{minipage}
\end{document}

```

**NOTE**: `luatex` or `xelatex` is better than `pdflatex` for compiling, and `luatex` is becoming the Tex engine of the day, because it supports Unicode encodings and OpenType fonts and opens up the internals of TEX via the Lua programming language

## About [fontspec][1] which is worth reading for various tips on latex

Use the following command to look up font info: 

```sh
otfinfo -i `kpsewhich lmroman10-regular.otf`
```

REF: [An introduction to Kpathsea and how TeX engines search for files][2]


---
[1]: http://mirrors-wan.geekpie.club/CTAN/macros/latex/contrib/fontspec/fontspec.pdf
[2]: https://www.overleaf.com/learn/latex/Articles/An_introduction_to_Kpathsea_and_how_TeX_engines_search_for_files
