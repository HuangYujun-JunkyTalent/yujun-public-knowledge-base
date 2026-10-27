# Public knowledge base

The knowledge base holds the facts on tools, formats and writing that the skills of the ai-workspace rest on.
Everyone may read it, so an item refers only to public things, as `skills/audiences.md` of the ai-workspace says.
A workspace clones this repository into `knowledge-bases/public-knowledge-base/`.

## Group the items by topic folders

The items are grouped in folders named by their topic, and each item is one Markdown file in its topic folder.
A topic folder sits directly under the root, and a topic holds no other topic.
A finer split uses a folder name of the form `topic-subtopic`.

## Name the file of an item by its subject

The file name of an item names the subject of the item, in as few lowercase hyphenated words as tell it apart from the other items of its topic.
An example is `git/merge-tree-dry-run.md`.
The title on the first line states the fact that the item holds, and the file name does not repeat the title.
The file name is the identifier of the item, and every reference to the item holds it.
A title changes whenever the fact is revised, while a change of the file name waits for the agreement of the owner of the knowledge base.

## Keep the files of an item in a folder of the same name

An item `<name>.md` may have a folder `<name>/` beside it in the same topic folder.
The folder holds the files of that item and nothing else, such as the source of a figure, the rendered figure, data and scripts.
The item refers to a file in its folder by relative path, for example `![caption](<name>/figure.svg)`.
A rendered file is generated from its source by a script and is not edited by hand, and the item names the script.

## Refer to an item by its path

A text outside the knowledge base refers to an item in the form below.

```
knowledge item `public-knowledge-base/<topic>/<name>.md`
```

## Find the items on a subject

The items on a topic are the files `<topic>/*.md`, and the first line of each file is its title.
An item is found from its name with `find knowledge-bases/public-knowledge-base -name '<name>.md'`.
A file of an item is found with `find knowledge-bases/public-knowledge-base -name '<file>'`, which reaches into the item folders.

## Write an item as a title, a body and two optional lines

The title is the first line, a sentence that states the fact the item holds.
The body is short prose with one sentence per line, and holds mathematics, examples, figures and code as the fact needs.
An example is made up, or names a public work, tool or standard.
The item ends with an optional `Source:` line and an optional `Related:` line.
The `Source:` line names a public source, such as a page of documentation with the date it was read, a paper, or a test that anyone can repeat.
A source that only some readers can open, such as a private conversation, is named by its kind and its date alone.
A contribution of a person, such as a request, a correction or a decision, is named by its kind, the name of the person and its date, as in `Source: a correction by Yujun Huang of 2026-10-20.`
The `Related:` line links related items of this knowledge base or of another public one.

## Recheck the source of an item that may be out of date

Some items depend on the time or on the version of a tool.
When such an item is used and its content may have changed, recheck its source.
The content may have changed when the tool behaves otherwise than the item says, or when the version in use is newer than the one the item names.
When the source has changed, update the item and the date in its `Source:` line, and remove the content that no longer holds.
