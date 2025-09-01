---
title: "Why I Still Sometimes Print Papers: And How I Do It Better?"
date: 2025-09-01
tags:
  - blog
  - scholar
  - academic-workflow
  - productivity
  - linux
  - ocr
  - cli-tools
  - how-to
  - digital-minimalism
  - tutorial
layout: post
---
There comes a time when you must read a long paper, book chapter, or text. This task feels too important to do on a PC or phone screen because you do not want to be distracted. This is the time I want to read long papers without distraction, but printing wastes paper, and e-readers are tricky.

**Attenzione!**  
This post will be expanded soon!
#### tldr

If you're dealing with Image-Based PDF: use [Scantailor](https://scantailor.org/). Prepare images:

<script src="https://gist.github.com/jacekstrzalkowski/9324b91085a7bdbd326a5f771d1044e1.js"></script>

and then

<script src="https://gist.github.com/jacekstrzalkowski/9f848540f5f3b8b7ed8c56100167da26.js"></script>

If you're dealing with True-Digital PDF:

<script src="https://gist.github.com/jacekstrzalkowski/4f500e02ae39734e4fbb5e82e329c02c.js"></script>

and then

<script src="https://gist.github.com/jacekstrzalkowski/cce087eb6842b25f46b100a61d17910e.js"></script>
#### Why it might not be a good idea to implement your scholar workflow if you have something to do.

One idea is to use an eBook reader like Kindle Paperwhite or OnyxBox. Your eyes wouldn't hurt cause of [E Ink](https://en.wikipedia.org/wiki/E_Ink). You think: it is very cool I might be able to *synchronize* my highlights and handwritten notes with my PC environment, like Zotero library or something similar. At least I think like this.

This idea is definitely worth expanding. I'd like to write something about it in the future because this is something I somehow *implemented*. Before you jump straight to it, please read this warning: **it won't take one or two hours** after which you will jump straight into the task with deep focus. It might, but I doubt it. You will implement your super productive scholar workflow, but you will only think about how cool it is and what new features you will add, resulting in picker attention to your reading plans.

If you have the whole weekend to read the paper before next week's classes, feel free to experiment with your workflow. If you succeed, it will be the best feeling to read that paper. If you fail and would still need to manually copy eBook files between eBook reader/PC to get your highlights (*ugh*), you can work on it later.

#### Why would I print it, if it will take so much paper?

Back to the problem: you want to read a paper/book without wasting too much paper. Let's analyze it: 500 pieces of A4 paper cost around 13 PLN. The printer toner, which was not produced by the producer, might cost around 0.0153 PLN per page. From this, we see the $0.0153 + 0.013 \approx 0.03$ PLN per page (not sheet).

Printing the 300 A4 page book will cost around $8$ PLN, which might be a little high for a home environment and needs. Here comes the idea of this blog post: you print many PDF pages on one A4 page. The standard solution is to print two pages side-by-side on a horizontally oriented paper page. This would cut the cost to 4 PLN.

I believe you can print 4 pages of a typical paper article (2x2 portrait) and keep it readable.

The reason why you should do it is not limited to money issues. When you can see four pages simultaneously, alongside your pen notes and highlights, it might be easier to read, think about, and discuss them with people. I believe it is better to hold *at once hold in your eyesight* more pages, more stuff people's understanding reduces to (*memorabilia*, more [Bacon's token](https://plato.stanford.edu/entries/francis-bacon/#Ido) or what?).

This is only my opinion. I believe the purpose of reading philosophical papers, or any complex stuff in general, is to build a more holistic understanding of the text. That usually comes from absorbing the work as a whole. It’s like rereading a mathematical proof: at first it feels opaque, but after going through it several times, the underlying idea suddenly *clicks*. That *click* can feel almost addictive; like a drug for scholars.
#### How to print it 2x2?

I have the following snippet saved since pandemic times, around 2020, in my [notes software](https://simplenote.com/), which I used before moving to [Obsidian](https://obsidian.md/). Assuming you

<script src="https://gist.github.com/jacekstrzalkowski/4f500e02ae39734e4fbb5e82e329c02c.js"></script>

you can

<script src="https://gist.github.com/jacekstrzalkowski/cce087eb6842b25f46b100a61d17910e.js"></script>

[pdfCropMargins](https://pypi.org/project/pdfCropMargins/) works great on *true digital* PDFs created using eg. MS Word or Latex. I like it more than `pdfcrop` - a tool shipped in `texlive-extra-utils` apt package. You might want to make some wrapper like

```bash
cat > ~/script/pdfcropmargins.sh <<'SH'
your/python/venv/path/python -m pdfCropMargins "$@"
SH
```

and invoke it like `~/script/pdfcropmargins.sh`. This is what I was doing for many years before I switched to [pipx](https://github.com/pypa/pipx).

Values `--trim '-9.5mm -4mm -9.5mm -4mm'` relate to the fact that cropping tools often leave some space, which we need very much, cause we want to shrink a big page into a small page. `--offset '5mm 0mm'` set the vertical space between sub-pages to `0` and horizontal to `5mm`. See the cropping problem below for further details.

The result often varies depending on the input file. For example, it might work great for the typical A5 paper size used often for novels:

<img src="{{ '/media/piasecki_in.png' | relative_url }}" alt="piasecki przed" />
<img src="{{ '/media/piasecki_out.png' | relative_url }}" alt="piasecki po" />

The 2x2 page I pasted above looks really good printed! Before I switched to E-Ink-based E-book readers, I printed and read many books like that.

Unfortunately, we might not produce the best results when dealing with certain page sizes, like here:

![wolnelektury_first_page_in.png]({{ "/media/wolnelektury_first_page_in.png" | relative_url }})
![wolnelektury_first_page_out2x2.png]({{ "/media/wolnelektury_first_page_out2x2.png" | relative_url }})

You might have to fold this for reading, but it will be ok.

I would also do this for Image-Based PDFs if I wanted to do things fast and move along. Below, I want to present the approach that works better for Image-Based PDFs, which includes [Human-In-The-Loop](https://en.wikipedia.org/wiki/Human-in-the-loop) and uses [Scantailor](https://scantailor.org/).

#### The cropping problem

The cropping is the most important part of preparing papers for printing: when you have a well-defined region you want to print, and every book page is a separate PDF page (see below to get what I mean), the rest is trivial.

`pdfcrop` works on digital PDFs where things are trivial - you estimate the interest region by looking at the PDF's TextBox. On the other hand, I encourage you to look at those PDF "artifacts" in the best free PDF-edition software on the market - [LibreOffice Draw](https://www.libreoffice.org/discover/draw/).

Image-based PDFs are harder in that aspect. We have a solution for that problem, which I believe (not sure) uses some computer-vision techniques like threshold-out black-points around the region of interest *ROI* (obtained using some biggest cluster algorithm or mentioned OCR layer?).

What is the region of interest? Of course, it is the text you want to read. But does this include footnotes, a header (with information about the chapter and such), or a page number, which can be far down the text? The bigger the region of interest we have, the less readable the printed page will look (cause it will be smaller). There were times when I decided to cut page numbers to make text bigger, and then I would write page numbers with a pen on printed pages.

#### Scanned page case

We often have to deal with scans like this


![raw_scanned_page.jpeg]({{ "/media/raw_scanned_page.jpeg" | relative_url }})

For this, it is good to use software with a GUI to select ROI. I chose [Scantailor Advanced](https://github.com/scantailor/scantailor) for this task. Scantailor is no longer in active development, but it works for my purposes. I encourage you to try [some forks](https://github.com/Tulon/scantailor) of the original repo. I got Scantailor from [flatpak](https://flathub.org/apps/com.github._4lex4.ScanTailor-Advanced).

<script src="https://gist.github.com/jacekstrzalkowski/9324b91085a7bdbd326a5f771d1044e1.js"></script>

Work with Scantailor consists of 6 stages: *Fix orientation*, *Split Pages*, *Deskew*, *Select Content*, *Margins*, and *Output*. You can read about those, but I will focus more on parameters/options crucial to our problem, ie, preparing a PDF for printing.

Page division works as expected:

![nice_page_division.png]({{ "/media/nice_page_division.png" | relative_url }})

Selecting contents works well. If something were to happen, you can adjust the boxes manually for each page you want.

<video controls preload="metadata" style="width:100%;height:auto;" playsinline>
  <source src="{{ '/media/threshold.mp4' | relative_url }}" type="video/mp4">
  Your browser doesn’t support HTML5 video.
  <a href="{{ '/media/selecting.mp4' | relative_url }}">Download the video</a>.
</video>

It is usually a good idea to turn `Margins->Alignment->Match size with other pages` option off.

Also, Scantailor provides an elegant way to control threshold parameters:

<video controls preload="metadata" style="width:100%;height:auto;" playsinline>
  <source src="{{ '/media/threshold.mp4' | relative_url }}" type="video/mp4">
  Your browser doesn’t support HTML5 video.
  <a href="{{ '/media/threshold.mp4' | relative_url }}">Download the video</a>.
</video>

After you are done, you join images into a PDF and OCR it. You can save the Scantailor project if you want - I sometimes have to work twice on the same book, for instance, when you see that the document does not meet your needs after printing, and I have already closed my PC.

<script src="https://gist.github.com/jacekstrzalkowski/9f848540f5f3b8b7ed8c56100167da26.js"></script>

Sometimes you need to print the page to see if it is readable for you.

![Kim_out.png]({{ "/media/Kim_out.png" | relative_url }})

You can remove temporary files if you are satisfied with the result `out2x2.pdf`.

```bash
rm -r images* doc*.pdf out_all.pdf out.pdf out_c.pdf dst.tif >/dev/null
```

[ocrmypdf](https://ocrmypdf.readthedocs.io/en/latest/) is a cool wrapper for Google [tesseract](https://tesseract-ocr.github.io/tessdoc/). `-l pol` lets you select the language for tesseract OCR.