> :warning: This is a work in progress.  If you're still interested, get in touch and I'll tell you about the state of the project. :warning: 


---



papermill. 
==========

> *write once, publish everywhere*

![p](http://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Forest_Fibre_Company_Berlin%2C_New_Hampshire.JPG/220px-Forest_Fibre_Company_Berlin%2C_New_Hampshire.JPG)

**`papermill` is a toolchain for cross-publishing long form text** to different media. The text can be something like an academic paper, an essay like static websites, an Ebook or a printable PDF.

It aims to provide the best possible output, with minimal manual labor and configuration. Just 'drop in' your Markdown formatted text file and you'll instantly get a static website and a printable PDF (via `LaTeX`).

Instead of a *framework*, `papermill` rather tries to be an opinionated collection of the best tools, standards and practices currently in use, combined with simple scripting to glue it together.


## Workflow

> :warning: For now you have to figure out most of the stuff yourself. I will write this as I go along. :warning:

1. Install the CLI tool [`mill`](https://github.com/papermill/mill) and dependencies according to instructions.

2. Start a new Paper with the Name "Testing"

    `````
    mill new Testing
    cd Testing
    `````

3. Write Text

4. Generate Output

    `````
    mill web
    mill print
    `````
5. [git commit](http://git-scm.com/docs/git-commit)

5. Optional: 
   - Automate Output Generation with git hooks (generate HTML after every commit)
   - Automate git commiting ([`flashbake`](https://github.com/commandline/flashbake), [`git-o-mat`](https://github.com/eins78/git-o-mat))


## Goals   

* modularity -- use all the scripts and get stuff done *or* implement part of the chain by using different tools.

* distinguish between client and server, but work local anyway

* no dependencies other than core tools and POSIX: a project like this has a shitload of dependencies anyway. I don't want to make it worse by adding even more -- what comes with any UNIX and Linux is plentifull.


## Architecture Overview: Components & Tools

![high-level overview](https://github.com/papermill/documentation/raw/master/images/papermill.sketch-arch1%402x.png)

### Text Input
- [`Git`](http://git-scm.com) -- version control <del>voodoo</del> software. Takes care of your files and their history -- it's a neverending palimpset! Plus it provides ways to collaborate with editors and sync with remote servers. It also has hooks which we can use for automation.
- [`Markdown`](http://daringfireball.net/projects/markdown/) -- plain text formating that goes out of your way, by [John Gruber](http://daringfireball.net). 
- [`pandoc`](http://johnmacfarlane.net/pandoc/)  -- Markdown is for web writers, but papermill is for everyone. `pandoc` provides extensions we need with a consistent syntax: Citations, Footnotes and Bibliography.
- [`mustache`](http://mustache.github.com) -- logic-less templates for (almost) everything. Also its successor [`handlebars`](http://handlebarsjs.com).
- [`BibTex`](http://www.bibtex.org) -- commonly used with LaTeX, stores your bibliography info in a (plain text) database.

### Output Generation
- `make` -- a good old friend from `*NIX` which, according to it's [manpage](http://man.cx/make) "shall update files that are derived from other files", which is precisely what we are doing here.

- [`punch`](https://github.com/laktek/punch) -- your friendly neighbourhood static website generator. Actually, it can be used for any kind of text files (just like `mustache`). This can be used to even generate your Mardown if you are working with data collections. Right now `papermill` can generate an picture catalogue from a `JSON` file. 

#### Website Output  
- [`html5boilerplate`](http://html5boilerplate.com) -- *nomen est omen*
- [`bootstrap`](http://twitter.github.com/bootstrap/) web framework -- `HTML`, `CSS` and `javascript` components. A great base to start your own layout, but it also looks great right out of the box.
- [`pandoc-bootstrap`](http://papermill.github.com/pandoc-bootstrap/)
- [Readabilty: Article Publishing Guidelines](http://www.readability.com/publishers/guidelines/#reader) -- good summary of the [hNews microformat specification](http://microformats.org/wiki/hnews) and [Mark Pilgrim's Dive into HTML5](http://diveintohtml5.ep.io/semantics.html#new-elements).

#### Print Output

Is taken care of by `pandoc` by converting the text to [`LaTeX`](#), providing a PDF output suitable for print and the ability to use costum `LaTeX` templates.

Literal `LaTeX` code can be used in the text and will only affect print output. This can be used to insert a `\pagebreak[4]` where a page break is wanted in the print output. 

The downside is that (right now) there is no way to add certain other codes like the `LaTex` logo itself (i.g. `\LaTeX`) while providing a fallback for the web output. This is one of the problems this projec wants to solve. Still, it should not be an huge issue in an academic context (where a lot of literal `LaTex` might be needed). Here the PDF output is needed as a final version and the web output serves as a preview since most papers can't be published online before they are turned in, graded, etc.

#### E-Book Output

Is also taken care of by `pandoc`, which has support for a wide range of output formats in general. 

Right now, the status is alpha quality: It produces output for simple texts, but some stuff breaks which shouldn't. Output to `ePub` is usable at least for proof reading on eBook devices.

#### Other Output

`pandoc` support a wide range of other output formats with different levels of quality. Especially the output to RTF and other Rich-Text formats should be interesting for authors which need to turn in files compatible with "Word".

Here ist the full list from the [`pandoc` documentation](http://johnmacfarlane.net/pandoc/README.html):

> it can write 
>
> - plain text
> - [markdown](http://daringfireball.net/projects/markdown/)
> - [reStructuredText](http://docutils.sourceforge.net/docs/ref/rst/introduction.html)
> - [XHTML](http://www.w3.org/TR/xhtml1/), [HTML5](http://www.w3.org/TR/html5/)
> - [LaTeX](http://www.latex-project.org/) (including [beamer](http://www.tex.ac.uk/CTAN/macros/latex/contrib/beamer) slide shows), [ConTeXt](http://www.pragma-ade.nl/)
> - [RTF](http://en.wikipedia.org/wiki/Rich_Text_Format)
> - [DocBook XML](http://www.docbook.org/)
> - [OpenDocument
XML](http://opendocument.xml.org/)
> - [ODT](http://en.wikipedia.org/wiki/OpenDocument)
> - [Word
docx](http://www.microsoft.com/interop/openup/openxml/default.aspx)
> - [GNU Texinfo](http://www.gnu.org/software/texinfo/)
> - [MediaWiki
markup](http://www.mediawiki.org/wiki/Help:Formatting)
> - [EPUB](http://www.idpf.org/)
> - [Textile](http://redcloth.org/textile)
> - [groff man](http://developer.apple.com/DOCUMENTATION/Darwin/Reference/ManPages/man7/groff_man.7.html) pages
> - [Emacs Org-Mode](http://orgmode.org)
> - [AsciiDoc](http://www.methods.co.nz/asciidoc/)
> and [Slidy](http://www.w3.org/Talks/Tools/Slidy/), [Slideous](http://goessner.net/articles/slideous/), [DZSlides](http://paulrouget.com/dzslides/) or [S5](http://meyerweb.com/eric/tools/s5/) HTML slide shows. 
> 
> It can also produce [PDF](http://www.adobe.com/pdf/) output on systems where LaTeX is installed.


## Roadmap

1. **better web output** -- think "book as web application". This is currently done in [pandoc-bootstrap](https://github.com/papermill/pandoc-bootstrap).
1. **better workflow** (also as web app)
1. **better collaboration** by integrating even more with `git`

## Alpha Testers

Alpha Testers (authors, writers) are needed to not loose touch with reality.
If you found this by accident, chances are you are a valuable candidate -- please get [in touch](http://twitter.com/eins78).

Alpha Testers so far:

- [Naomi T. Salmon](http://nts.is) (for her doctoral thesis)
- Bastian Bügler




--- --- --- 

> List of considered names for this project:   
[papermill](https://upload.wikimedia.org/wikipedia/commons/1/19/Forest_Fibre_Company_Berlin%2C_New_Hampshire.JPG). paperfront. paperboy. papermache. papermachine. papeterie. Passepartout. tangeable.  
Impact -- Impackd -- Imprint.  
formatters -- form&matter -- form matters.  
Publish or perish -- P.O.P. -- PopUp.  
[Samizdat](https://en.wikipedia.org/wiki/Samizdat).


> List of possibly further inspiring Words or Wikipedia Articles:  
    [Vanity](https://en.wikipedia.org/wiki/Vanity). 
