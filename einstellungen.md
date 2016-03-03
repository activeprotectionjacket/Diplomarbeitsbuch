# Was man einstellen kann

Einige *Dinge* kann man bei der Diplomarbeit auch noch anpassen. Einiges ist sogar 
in der lyx- bzw. tex-Datei schon vorgesehen -- bitte den Anfang des Source-Codes
aufmerksam lesen.


## Inhalt/Kapitel

Einige Ideen zur Gliederung der Arbeit gibt es unter
http://www.diplomarbeiten-bbs.at/ 
^[http://www.diplomarbeiten-bbs.at/erstellung/durchf%C3%BChrung/gliederung-der-diplomarbeit-und-formale-vorgaben-0]

## Einseitig/zweiseitig

Korrekturexemplar immer einseitig drucken. Beim fertigen Buch sind beide Varianten 
möglich -- bitte mit dem Betreuer abklären.

## Farben

* Deckblatt mit Logo immer in Farbe
* Farbausdrucke sind viel teurer -- Notwendigkeit prüfen und mit dem Betreuer abklären.
* am aufwändigsten: ein paar Seiten in Farbe
* falls Schwarz-Weiß: kein Logo beim laufenden Text

## Autor

Bei der Diplomarbeit muss jeder Text einem Autor zuzuordnen sein. Man kann das wie in 
der Vorlage in der Fußzeile machen. Alternativ kann man auch eine Übersicht als Anhang
eingefügt werden --  bitte mit dem Betreuer abklären.

## Absätze

### Einzug

Den Beginn eines neuen Absatzes kann man durch Abstand oder durch Einrücken kennzeichnen.

Diese Einstellung wird in der Vorlage bzw. im Header gemacht:

```latex
%\parindent0pt % auskommentieren, wenn keine Einrueckung der 
               % ersten Absatzzeile gewuenscht
%\parskip1.5ex plus0.5ex minus0.5ex % flexibler Absatzabstand
```

Die Option `parskip=half` bei `documentclass` ersetzt bereits den Absatzeinzug durch einen Absatzabstand.

oder http://ctan.org/pkg/parskip

```latex
\usepackage[parfill]{parskip}
```

### noch schöner

http://www.khirevich.com/latex/microtype/

## Aufzählungen

Man kann die Einrückung und vieles mehr anpassen:

```latex
\usepackage{enumitem}
\setlist[1]{labelindent=\parindent}
\setlist{align=left}
\setlist[itemize]{leftmargin=*}

% oder: 
\setlength\partopsep{0.5ex}
```

## Warnungen

```latex
%\sloppy  % etwas laxere Abstandskontrolle (weniger Fehlermeldungen)
```

## Listings


Sonderzeichen: 

http://en.wikibooks.org/wiki/LaTeX/Source_Code_Listings#Encoding_issue

Anpassen: 

http://stackoverflow.com/questions/1965702/how-to-mark-line-breaking-of-long-lines
  
http://www.bollchen.de/blog/2011/04/good-looking-line-breaks-with-the-listings-package/


## Zitierstil:

Bitte mit dem Betreuer abklären.

* plaindin - mit nummern [1]
* alphadin - mit Text+Jahr [Hor99]


## Glossar 

* http://texwelt.de/wissen/fragen/10496/glossaries-alle-symbole-nur-verwendete-abkurzungen-anzeigen
* http://en.wikibooks.org/wiki/LaTeX/Glossary für die verschiedenen Formen

## Ausdruck zu weit oben/unten

Man kann das Layout anpassen: Position auf Seite

```latex
\voffset10mm
```

## URLs

Man kann/sollte das `hyperref`-Paket anpassen, die bunten Links kann man ausschalten.

```latex
\hypersetup{breaklinks=true,
bookmarks=true,
pdfauthor={Mein Name},% <------------------- anpassen!
pdftitle={Die Diplomarbeit},% <------------------- anpassen!
colorlinks=true,
citecolor=blue,
urlcolor=blue,
linkcolor=magenta,
pdfborder={0 0 0}}

\urlstyle{same}
```

## Pandoc-Caption mit Verweis

siehe  https://github.com/chiakaivalya/thesis-markdown-pandoc

This is how you insert figures using markdown. Also how to insert citations copied 
over from your bibliography manager (I specifically used Pandoc Citations in Papers).

```
![Figure from Walczak, 2010[@Walczak:2010uk]. \label{mitosis} ](figures/mitosis_Walczak.pdf)
```

## Seitennummern

http://www.golatex.de/wiki/%5Cfrontmatter

```latex
\frontmatter % switches to roman numbering
\mainmatter
\backmatter
```

## TU Wien -- Informatik

* https://gitlab.cg.tuwien.ac.at/auzinger/vutinfth.git
    * super Vorlage und Build-Skripts (auch für Windows)
* http://www.informatik.tuwien.ac.at/dekanat/abschluss-master
* http://www.informatik.tuwien.ac.at/fakultaet/informatik-code-of-ethics.pdf
* Alte Vorlage
    * http://ieg.ifs.tuwien.ac.at/~aigner/download/tuwien.sty
    * von dort sind die Abkürzungen kopiert

# Skripts

##  Pandoc  nach Latex

```bash
#! /bin/bash
#set -x
#set -v
set -e

PANDOCMODULES=markdown+auto_identifiers
PANDOCMODULES=${PANDOCMODULES}+definition_lists
#PANDOCMODULES=${PANDOCMODULES}+compact_definition_lists
PANDOCMODULES=${PANDOCMODULES}+fenced_code_attributes
PANDOCMODULES=${PANDOCMODULES}+autolink_bare_uris
PANDOCMODULES=${PANDOCMODULES}+simple_tables+table_captions
PANDOCMODULES=${PANDOCMODULES}+inline_notes+footnotes


PANDOCOPT="--listings-S -N -f ${PANDOCMODULES}"


mkdir -p ../kaptex/
rm -f ../kaptex/*

for f in *.md
do
   out=$(basename $f .md).tex
   echo -n $f " "
   pandoc ${PANDOCOPT} $f -o ../kaptex/$out
done
```

## Diplomarbeit bauen

Wichtig -- damit alle Seitenummern und Verweise  passen:

* erster Latex-Lauf
* `makeindex` und `bibref` aufrufen
* noch zweimal Latex 

```bash
cd kapmd
./create.sh
cd ..
pdflatex diplbuch.tex &&
makeindex -c -q diplbuch.idx &&
bibtex diplbuch
pdflatex diplbuch.tex &&
pdflatex diplbuch.tex
```
