`project-eDNA-yellow/code`

All file names should adhere to this format:

00.00-<code_topic>.Rmd

- `00.00`: Numbers to left of decimal should be incremented for each new type of analysis, which is unrelated to other existing analyses. Numbers to the right of the decimal should be incremented for analyses related to existing analyses.

	- E.g. `01.00-exon-counts.Rmd`
	- E.g. `01.01-exon-counts-per-gene.Rmd`
	

- `<code_topic>`: Provide a brief description of code functionality. E.g. `exon_counts`.

- `.Rmd`: Most code is anticipated to be run using R Markdown. However, other formats are fine (e.g. Jupyter Notebooks). 

All outputs from a script should be directed to a corresponding directory with the same name (excluding the suffix) in the `../ouput` directory.

---