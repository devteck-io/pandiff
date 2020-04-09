# PanDiff Docker

## Docker

This version use `` docker run -a stdin -a stdout -i --rm --volume "`pwd`:/data" --user `id -u`:`id -g` pandoc/latex:2.6 "$@" `` to run all commands.

## Features

- Prose diffs for [any document format supported by Pandoc](https://pandoc.org/MANUAL.html)
- Supported output formats:
  - [CriticMarkup](http://criticmarkup.com/)
  - HTML
  - PDF, via LaTeX
  - [Word docx](https://en.wikipedia.org/wiki/Office_Open_XML) with [Track Changes](https://support.office.com/en-us/article/track-changes-in-word-197ba630-0f5f-4a8e-9a77-3712475e806a)
- Respects document structure, so won't produce broken markup like `wdiff`

## Installation

First install docker in your server, then run:

```sh
npm install -g pandiff-docker
```

## Usage

```sh
pandiff-docker test/old.md test/new.md
```

````markdown
# {~~Old~>New~~} Title

{--![image](minus.png)--}

{++![image](plus.png)++}

1.  Lorem ipsum dolor {++sit ++}amet
2.  {++[consectetur adipiscing
    elit](https://en.wikipedia.org/wiki/Lorem_ipsum)++}
3.  Lorem{-- ipsum--} dolor sit amet

I really love _italic {~~fonts~>font-styles~~}_ {~~here.~>there.~~}

```diff
 print("Hello")
-print("world.")
+print("world!")
 print("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt")
```

Don’t go around saying {--to people that --}the world owes you a
living. The world owes you nothing. It was here first. {~~One~>Only
one~~} thing is impossible for God: To find{++ any++} sense in any
copyright law on the planet. Truth is stranger than fiction, but it is
because Fiction is obliged to stick to possibilities; Truth isn’t.
````

### Options

```sh
pandiff-docker --help
```

```
Usage: pandiff-docker [OPTIONS] FILE1 FILE2
      --atx-headers
      --bibliography=FILE
      --columns=NUMBER
      --extract-media=PATH
  -F, --filter=STRING
  -f, --from=FORMAT
  -h, --help
      --highlight-style=STRING
      --lua-filter=FILE
  -o, --output=FILE
      --pdf-engine=STRING
      --reference-links
      --resource-path=PATH
  -s, --standalone
  -t, --to=FORMAT
  -v, --version
      --wrap=STRING
```

### Git integration

Configure git by running the following commands:

```sh
git config --global difftool.pandiff-docker.cmd 'pandiff-docker "$LOCAL" "$REMOTE"'
git config --global alias.pandiff-docker 'difftool -t pandiff-docker -y'
```

Now you can use `git pandiff-docker` wherever you would usually use `git diff`.

### HTML output

```sh
pandiff-docker old.md new.md -s -o diff.html
```

[![](test/diff.html.png)](https://rawgit.com/davidar/pandiff-docker/master/test/diff.html)

### PDF output

```sh
pandiff-docker old.md new.md -o diff.pdf
```

[![](test/diff.pdf.png)](https://rawgit.com/davidar/pandiff-docker/master/test/diff.pdf)

### Word Track Changes

```sh
pandiff-docker old.md new.md -o diff.docx
```

```sh
pandiff-docker test/track_changes_move.docx
```

```markdown
Here is some text.

{++Here is the text to be moved.++}

Here is some more text.

{--Here is the text to be moved.--}
```
