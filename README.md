# ceur-add-pagenum
This script adds page numbers to an index.html file formatted according to the
CEUR-WS specification: https://ceur-ws.org/HOWTOSUBMIT.html. It requires the `lxml` and `PyPDF2` packages.

The script is used with the following syntax:
```bash
python3 ceur-add-pagenum [<dir>]
```
where `<dir>` is the directory containing the `index.html` file (defaults to the current directory if omitted). Typically, this
directory also contains the PDF files to be processed, although this is not strictly required.

The script processes the `index.html` file by:
1. Saving the original file as index.html.bak.
2. Counting the number of pages in the referenced PDF files.
3. Updating the file with the corresponding page ranges.

## Excpected format of the index file

`ceur-add-pagenum makes` specific assumptions about the structure of the `index.html` file.
In particular, each paper to be included in the page count must be represented by an `<li>` element containing:
  * a child `<a>` element with the link to the PDF file, and
  * a child `<span>` element with the CEURPAGES class, which the script will populate with the corresponding page range.

If you are preparing proceedings for submission to CEUR-WS using their official template, your `index.html` will almost certainly already follow this structure.

No assumptions are made about the filenames of the PDF files, since their paths are extracted directly from the HTML file.

For example, consider the following fragment of an index.html file:

```html
<h2> Table of Contents </h2>

<ul>
  <li id="preface">
    <a href="preface.pdf">Preface</a><br>
    This is the preface
    <span class="CEURPAGES"></span>
  </li>
</ul>
<br>

<h3><span>Invited Talks</span></h3>
<ul>
  <li id="invited1">
    <span>Invited Paper 1</em>
    <br>
    <span><em>My favourite author</em></span>
  </li>
</ul>

<h3><span class="CEURSESSION">REGULAR SESSION</span></h3>
<ul>
  <li id="goodpaper"><a href="goodpaper.pdf">
    <span class="CEURTITLE">A nice paper</span></a>
    <span class="CEURPAGES"></span>
    <br>
    <span class="CEURAUTHOR">Author 1 </span>,
    <span class="CEURAUTHOR">Author 2</span>,        
  </li>

  <li id="badpaper"><a href="badpaper.pdf">        
    <span class="CEURPAGES"></span>
  </li>
</ul>
```

If the files `preface.pdf`, `goodpaper.pdf`, and `badpaper.pdf` each contain 10 pages, the three `<span>` tags with class `CEURPAGES` will be replaced by the ranges 1–10, 11–20, and 21–30, respectively, in the order in which they appear.
