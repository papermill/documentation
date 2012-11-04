> Warning: This is a work in progress.  If you're still interested, get in touch and I'll tell you about the state of the project.


---



papermill. 
==========

> *write once, publish everywhere*

![p](http://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Forest_Fibre_Company_Berlin%2C_New_Hampshire.JPG/220px-Forest_Fibre_Company_Berlin%2C_New_Hampshire.JPG)

`papermill` is a toolchain for cross-publishing long form text to different media. The text can be something like an academic paper, an essay like static websites, an Ebook or a printable PDF.

It aims to provide the best possible output, with minimal configuration and manual work. Just 'drop in' your Markdown formatted text file and you'll instantly get a static website and a printable PDF (via LaTeX).

Instead of a framework, `papermill` rather tries to be an opinionated collection of the best tools, standards and practices currently in use, combined with simple scripting to glue it together.


## Workflow

> For now you have to figure out most of the stuff yourself. I will write this as I go along.

1. Get the `Makefile`.  
   `curl foo/Makefile`

2. Check dependencies

You need:

-


3. Setup your Document  
   `make plain`

4. Drop in your Markdown-formatted text.
   `open paper.markdown`

5. `make web`


## Goals   
* modularity -- 

* distinguish between client and server, but work local anyway

* no dependencies other than core tools and POSIX: a project like this has a shitload of dependencies anyway. I don't want to make it worse by adding even more -- what comes with any UNIX and Linux is plentifull.


## Tools

#### Text Input
- `Git` -- version control <del>voodoo</del> software. Takes care of your files and their history -- it's a neverending palimpset! Plus it provides ways to collaborate with editors and sync with remote servers. It also has hooks which we can use for automation.
- [`Markdown`](http://daringfireball.net/projects/markdown/) -- plain text formating that goes out of your way, by [John Gruber](http://daringfireball.net). 
- `MultiMarkdown` -- Markdown is for web writers, but papermill is for everyone. MultiMarkdown provides extensions we need with a consistent syntax:Citations, Footnotes and Bibliography.
- [`mustache`](http://mustache.github.com) -- logic-less templates for (almost) everything.
- `BibTex` -- commonly used with LaTeX, stores your bibliography info in a (plain text) database.

#### Output Generation
- `make` -- a good old friend from `*NIX` which, according to it's [manpage](http://man.cx/make) "shall update files that are derived from other files", which is precisely what we are doing here.

- `punch` -- your friendly neighbourhood static website generator. Actually, it can be used for any kind of text files (just like mustache). This is the most recent piece of the puzzle and it's awesomeness is what got `papermill` finally started.

##### Website Output  
- [`html5boilerplate`](http://html5boilerplate.com) -- *nomen est omen*
- [`bootstrap`](http://twitter.github.com/bootstrap/) web framework -- `HTML`, `CSS` and `javascript` components. A great base to start your own layout, but it also looks great right out of the box.
- [`pandoc-bootstrap`](http://papermill.github.com/pandoc-bootstrap/)
- [Readabilty: Article Publishing Guidelines](http://www.readability.com/publishers/guidelines/#reader) -- good summary of the [hNews microformat specification](http://microformats.org/wiki/hnews) and [Mark Pilgrim's Dive into HTML5](http://diveintohtml5.ep.io/semantics.html#new-elements).

##### Print Output
- [`LaTeX`](#) -- it's huge. 

--- --- --- 

> List of possible names for this project:   
[papermill](https://upload.wikimedia.org/wikipedia/commons/1/19/Forest_Fibre_Company_Berlin%2C_New_Hampshire.JPG). paperfront. paperboy. papermache. papermachine. papeterie. Passepartout. [Samizdat](https://en.wikipedia.org/wiki/Samizdat).


> List of possibly further inspiring Words or Wikipedia Articles:  
    [Vanity](https://en.wikipedia.org/wiki/Vanity). 
    tangeable. 
		Publish or perish -- P.O.P. -- PopUp. 
		Impact -- Impackd -- Imprint. 
		formatters -- form&matter -- form matters