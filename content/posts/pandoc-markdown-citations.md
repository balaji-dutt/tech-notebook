---
title: "Using Pandoc to expand citations (without Bibliographies) in Markdown"
date: 2020-05-28T17:12:02+08:00
draft: false
---

Not very surprisingly I think, given the Zettelkasten technique originated from an academic background a number of practitioners are from the same background and thus there is quite a lot of emphasis on citing the proper sources etc. I caught that bug as well if only in that I wanted a more reliable way to track articles I would use in my Zettelkasten than just Pinboard bookmarks. That lead me to Zotero and from there it was a one short fateful step to learning about pandoc. Pandoc is rightly the "Swiss army knife of document manipulation", but so many times while I was desperately trying to bend it to my will I really really wished I had never made the decision to investigate pandoc.

Anyway, this post is about something I learnt to do in pandoc for what will eventually become the public face of my Zettelkasten notes - a way to generate Hugo compatible Markdown files with a full citation but no bibliography sections, all via Pandoc.

Here then in all it's glory is the final invocation I came up with (minus some other bits I will blog about in another post)

```shell
pandoc X:\path\to\source.md -f markdown --standalone --wrap=preserve --atx-headers --filter pandoc-citeproc --metadata=suppress-bibliography -t markdown_mmd+yaml_metadata_block -o X:\path\to\content\posts\target.md --bibliography="X:\path\to\Zotero\bibliography.bib" --csl="X:\path\to\chicago-full.csl"

```

Explanation of the options is below:

* ```X:\path\to\source.md```: Self-explanatory. Input file.
* ```-f markdown```: This just specifies that the input file uses pandoc markdown syntax. Not entirely sure if this is required since file extension will be .md.
* ```--standalone```: Required to ensure that YAML headers appear in the output file. It's unclear why "draft: false" in the YAML headers of the input file gets stripped in output.
* ```--wrap=preserve```: Ensures line-wraps in source file are preserved. Not sure if this is required for YAML headers, but definitely needed for TOML headers.
* ```--atx-headers```: Ensures output markdown file uses "#" for Markdown headers rather than the "--/==" syntax.
* ```--filter pandoc-citeproc```: Where it all started. Makes Pandoc process any references in source file that use the Pandoc "[@]" reference syntax.
* **The Biggie** ```--metadata=suppress-bibliography```: Additional metadata option (This option take precedence over metadata supplied through "--metadata-file" and metadata inside the source file) to prevent a bibliography section from being added at the end of each output file.
* ```-t markdown_mmd+yaml_metadata_block```: Generates a Multimarkdown output file (which supports footnotes) while also ensuring that YAML metadata blocks in source files are retained in the output.
* ```-o X:\path\to\content\posts\target.md```: Path to output directory. Output file name must be supplied by the calling script.
* ```--bibliography="X:\path\to\Zotero\bibliography.bib"```: Path to Bibliography file generated from Zotero. I'm using the Better BibTex plugin in Zotero while outputting the bibliography file. Use quotes to fix any spaces in the Path.
* ```--csl="X:\path\to\chicago-full.csl```: Path to Citation Style Langugage (CSL) file. I'm using the "Chicago Manual of Style 17th edition (full note)" citation style here.

#### Alternative (Not Tested!)

* ```--lua-filter pandoc-zotxt.lua```: This will use zotxt from a running Zotero instance for citations instead of a Bibliography file. Requires [zotxt extension in Zotero](https://github.com/egh/zotxt) and [pandoc-zotxt extension in Pandoc](https://github.com/odkr/pandoc-zotxt.lua)
