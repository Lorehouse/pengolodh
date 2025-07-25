# pengolodh

A text preparation pipeline

Initially focused on extracting text from EPUBs for citation and stand-off annotation.

## To Set Up

- `uv sync`
- activate venv

## To Run

- `pengolodh title <book-id-or-path>`

will either print the title (from both metadata and ncx) of the volume or, if the assertions are too strict, throw an exception.

- `pengolodh spine <book-id-or-path>`

will either print the "spine" of the volume, or, if the assertions are too strict, throw an exception.

- `pengolodh extract-map <book-id-or-path> [<item-ref>] [<address>] [--recurse]`

will give information about HTML elements in the EPUB.

If there is an `item-ref` then only that item (i.e. file) will be considered otherwise all items will be traversed.

If there is an `address` then only that element will be extracted otherwise the root will be extracted.

If there is a `--recurse` then information about the descendants will also be given.

The results are in tuple form if `--recurse` is used, otherwise they are in a dictionary.

Note that the name `extract-map` is historical and will likely change.

- `pengolodh text <book-id-or-path> <item-ref> [<address>]`

will extract the plain text of the given item (or the specific address, if given)

- `pengolodh xml <book-id-or-path> <item-ref> [<address>]`

will extract the XML of the given item (or the specific address, if given)

- `pengolodh tree <book-id-or-path> <item-ref> [<address>] [--depth <depth>]`

will show the tree structure of the given item (or the specific address, if given) optionally up to the given depth.

- `pengolodh list-books`

will list any books configured with ids (see under What is a `book-id-or-path`?)

## Some Examples of `extract-map`

```
$ pengolodh extract-map <book-id-or-path> chapter01
{'label': 'body.text#text', 'offset': 0, 'length': 170401, 'child_count': 1}

$ pengolodh extract-map <book-id-or-path> chapter01 1
{'label': 'div.chapter#chapter01', 'offset': 1, 'length': 170400, 'child_count': 4}

$ pengolodh extract-map <book-id-or-path> chapter01 1.3.2
{'label': 'h2.chapterTitle', 'offset': 7, 'length': 20, 'child_count': 1}

$ pengolodh extract-map <book-id-or-path> chapter01 1.3.2.1
{'label': 'span.bold', 'offset': 7, 'length': 20, 'child_count': 0}

$ pengolodh extract-map <book-id-or-path> chapter01 1.3.2 --recurse
('h2.chapterTitle', 7, 20, [('span.bold', 7, 20, [])])
```

`$ pengolodh text <book-id-or-path> chapter01 1.3.2` will then give the extracted plain text.

## What is a `book-id-or-path`?

This can either be the full path to an EPUB file, an unzipped EPUB, or a book identifier set in `$XDG_CONFIG/pengolodh.toml` as follows:

```
[books]
<book-id> = "<path-to-epub>"
...
```

## What is an `item-ref`?

An `item-ref` is an identifier for a particular HTML file in the EPUB given by the first column of the output of the `spine` command.

## What is an `address`?

An `address` is a dot-separated path to a particular element in an HTML file. `5.1.3` would mean the third child or the first child of the fifth child of the root.
