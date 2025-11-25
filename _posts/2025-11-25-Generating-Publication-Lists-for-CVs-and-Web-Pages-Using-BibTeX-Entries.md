---
layout: post
title: Generating Publication Lists for CVs and Web Pages Using BibTeX Entries
---

## Biblatex for latex CVs

It is useful to annotate authors in a CV, to highlight your name or add symbols denoting corresponding authors or equal contributions. The following code demonstrates how to achieve this in latex.

In the .bib file, declare author annotations using the field `author+an`.

```tex
% ref.bib

@article{einstein_newton_paper,
  author = {Einstein, Albert and Newton, Isaac},
  author+an = {1=corresponding;2=equalcontrib,highlight;},
  title = {A Groundbreaking Paper on Physics},
  journal = {Journal of Modern Physics},
  volume = {1},
  number = {1},
  pages = {1--10},
  year = {1925},
  doi={10.1007/s12043-024-02215-x},
  url={https://doi.org/10.1007/s12043-024-02215-x},
  note = {Created by AI.}
}

@article{maxwell_schrodinger2024_wave_unification,
  title={Unified Wave Dynamics: Bridging Classical Electromagnetic Waves and Quantum Wave Functions},
  author={Maxwell, James Clerk and Schr{\"o}dinger, Erwin},
  author+an = {2=highlight;},
  journal={Annals of Theoretical Physics},
  volume={22},
  number={4},
  pages={319--347},
  year={2024},
  publisher={Springer Nature},
  doi={10.1007/s12043-024-02215-x},
  url={https://doi.org/10.1007/s12043-024-02215-x},
  note={Created by AI.}
}
```

In the .tex file, 

```tex
% test.tex

\documentclass[12pt]{article}
\usepackage[top=0.7in, bottom=0.7in, left=0.6in, right=0.7in]{geometry}
\usepackage{enumitem}
% for chinese characters
\usepackage{xeCJK}
\usepackage{hyperref}
\hypersetup{colorlinks=true, citecolor=blue, urlcolor=blue, linkcolor=blue}

% Configure biblatex to annotate authors
\usepackage[sorting=none,maxnames=1000,style=phys,biblabel=brackets,giveninits=false]{biblatex}
\DeclareFieldFormat{titlecase}{#1}
\renewcommand*{\mkbibcompletename}[1]{\ifitemannotation{highlight}{\underline{#1}}{#1}}

\renewcommand*{\mkbibnamefamily}[1]{%
  \ifitemannotation{equalcontrib}{%
    \ifitemannotation{corresponding}{{#1}$^{\dagger\#}$}{{#1}$^\dagger$}%
  }{%
    \ifitemannotation{corresponding}{{#1}$^\#$}{#1}%
  }%
}
\addbibresource{ref.bib}

\begin{document}

% publication
\textbf{Publication ($\dagger$：co-first，\#：co-corresponding)}

\begin{refsection}
\nocite{einstein_newton_paper,
        maxwell_schrodinger2024_wave_unification}
\printbibliography[heading=none]
\end{refsection}

\end{document}
```

The result:

<img width="698" height="177" alt="图片" src="https://github.com/user-attachments/assets/d855f91b-6c2b-4bb0-a448-d674feaeba92" />

## For web pages

It may be convinent to use [bibtex-js](https://github.com/pcooksey/bibtex-js) for generating publication lists on webpages. However, bibtex-js lacks support for author annotations (or I didn't figure it out). So I used an AI to assist me in generating javascript code to handle this. It still takes much time to finetune and debug, especially for refining the regular expression that parses keys and values in bibtex entries.

### How does it work

The javascript (see below) retrieves bibtex data from a hidden `<textarea>` element (id = "bib_entries"), processes and outputs a formatted HTML string to an `<ul>` element (id = "publications").

### Special treatments for latex commomnds in titles and author names

LaTeX commands unrecognized by MathJax, such as `\textendash` (en dash) or `\"o` (umlauted 'o'), may appear in author names or titles. These commands are usually enclosed by "{" and "}". Configuring MathJax to load macros like `textcomp` and take "{" and "}" as  inline math symbols can resolve some of the issues, but not all. If one wants to keep the bibtex string untounched, the simplest workaround is to explicitly replace unrecognized commands in the javascript part when formatting names or titles. See `formattedName = formattedName.replace('{\\"o}','&ouml;');` and `<em>${title.replace('{\\textendash}','&ndash;')}</em>` below.

```html
<!-- test.html -->

<!-- Begin HTML design -->
<ul id="publications"></ul>
<!-- End HTML design -->

<!-- Begin bib entries -->
<textarea id="bib_entries" style="display:none;">
@article{einstein_newton_paper,
  author = {Einstein, Albert and Newton, Isaac},
  author+an = {1=corresponding;2=equalcontrib,highlight;},
  title = {A Groundbreaking Paper on Physics},
  journal = {Journal of Modern Physics},
  volume = {1},
  number = {1},
  pages = {1--10},
  year = {1925},
  doi={10.1007/s12043-024-02215-x},
  url={https://doi.org/10.1007/s12043-024-02215-x},
  note = {Created by AI.}
}

@article{maxwell_schrodinger2024_wave_unification,
  title={Unified Wave Dynamics: Bridging Classical Electromagnetic Waves and Quantum Wave Functions},
  author={Maxwell, James Clerk and Schr{\"o}dinger, Erwin},
  author+an = {2=highlight;},
  journal={Annals of Theoretical Physics},
  volume={22},
  number={4},
  pages={319--347},
  year={2024},
  publisher={Springer Nature},
  doi={10.1007/s12043-024-02215-x},
  url={https://doi.org/10.1007/s12043-024-02215-x},
  note={Created by AI.}
}
</textarea>
<!-- End bib entries -->

<!-- Begin JS code -->
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-mml-chtml.js"></script>

<script>
MathJax = {
  tex: {
    inlineMath: {'[+]': [['$', '$']]}
  }
};
</script>

<script>
	document.addEventListener('DOMContentLoaded', function() {
		function parseAndFormatBibtex(bibtexString) {
			const entryRegex = /@(\w+)\{([^,]+),\s*([\s\S]+?)\s*\}\s*(?=@|$)/g;
			const entries = [];
			let match;

			for (match of bibtexString.matchAll(entryRegex)) {
				const fieldsString = match[3];
				const fields = {};
				const fieldRegex = /([\w\+]+)\s*=\s*(?:\{\{([\s\S]*?)\}\}|\{([\s\S]*?)\}|"([^"\\]*?)(?:\\.[^"\\]*?)*"|'([^'\\]*?)(?:\\.[^'\\]*?)*')(?=\s*,|$)/g;
				let fieldMatch;
				for (fieldMatch of fieldsString.matchAll(fieldRegex)) {
					const fieldName = fieldMatch[1];
					let fieldValue = fieldMatch[2] || fieldMatch[3] || fieldMatch[4] || fieldMatch[5] || '';
					fields[fieldName.toLowerCase()] = fieldValue.trim();
				}
				entries.push(fields);
			}

			let formattedOutput = '';

			entries.forEach((fields, index) => {
				const author = fields.author || 'No authors';
				const title = fields.title || '';
				const journal = fields.journal || fields.booktitle || '';
				const volume = fields.volume || '';
				const pages = fields.pages || '';
				const year = fields.year || '';
				const doi = fields.doi || '';
				const authorAn = fields['author+an'] || '';

				const authorAnnotations = {};
				
				if (authorAn) {
					const annotationParts = authorAn.split(';');
					annotationParts.forEach(part => {
						const keyValue = part.split('=');
						if (keyValue.length === 2) {
							const authorIndex = parseInt(keyValue[0].trim()) - 1;
							const annotationTypes = keyValue[1].split(',').map(type => type.trim());
							if (!isNaN(authorIndex)) {
								authorAnnotations[authorIndex] = annotationTypes;
							}
						}
					});
				}
				const authorsArray = author.split(/\s+and\s+/).map(name => name.trim());
				const formattedAuthors = authorsArray.map((name, authIdx) => {
					const annotationTypes = authorAnnotations[authIdx] || [];
					let formattedName = name;
					if (formattedName.includes(',')) {
						formattedName = formattedName.split(',').map(item => item.trim()).reverse().join(' ');
					}
					annotationTypes.forEach(annotationType => {
						switch (annotationType) {
							case 'highlight':
								formattedName = `<b>${formattedName}</b>`;
								break;
							case 'corresponding':
								formattedName = `${formattedName}#`;
								break;
							case 'equalcontrib':
								formattedName = `${formattedName}*`;
								break;
						}
					});
					formattedName = formattedName.replace('{\\"\\i}','&Iuml');
					formattedName = formattedName.replace('{\\o}','&oslash');
					formattedName = formattedName.replace('{\\"o}','&ouml;');
					formattedName = formattedName.replace('\\ifmmode \\check{Z}\\else \\v{Z}\\fi{}','&#381');
					return formattedName;
				}).join(', ');

				let journalInfo = journal;
				if (volume) {
					journalInfo += ` ${volume}`;
				}
				if (pages) {
					journalInfo += `, ${pages}`;
				}
				
				formattedOutput += `
					<li>
						${formattedAuthors}, <em>${title.replace('{\\textendash}','&ndash;')}</em>, <a href='https://doi.org/${doi}' target='_blank' style="text-decoration: none;">${journalInfo}, (${year})</a>.
					</li>
				`;
			});
			return formattedOutput;
		}

		// --- main ---
		let bibtexInput = document.getElementById('bib_entries');
		let bibliographyContainer = document.getElementById('publications');
		bibliographyContainer.innerHTML = parseAndFormatBibtex(bibtexInput.value);
	});
</script>
<!-- End JS code -->
```

The result:

<img width="1020" height="78" alt="图片" src="https://github.com/user-attachments/assets/c3794cec-c783-4e55-b4d9-d39b85da0e0f" />





