# A Zotero item key and a Better BibTeX citekey name the same item

An item key is the eight-character key that Zotero gives every item, such as `ABCD2345`.
The MCP tools and the local API take item keys.
A parent item, such as a paper, and its child items, such as a PDF attachment or a note, have different keys.
A search that matches both the parent and its attachment shows the same title twice, and the parent key names the item.

A citekey is the key that Better BibTeX derives from the item, such as `censiMathematicalTheoryCoDesign2016`.
The citekey is derived from the author, the title and the year, so it stays the same unless one of those changes.
A person can read a citekey, and notes and papers cite by it.
The default citekey formula of Better BibTeX is `auth.lower + shorttitle(3,3) + year`.
In Markdown a citation is `[@censiMathematicalTheoryCoDesign2016]`, and in LaTeX it is `\cite{censiMathematicalTheoryCoDesign2016}`.

The tool `zotero_search_by_citation_key` maps a citekey to its item, and the Better BibTeX method `item.citationkey` maps item keys to citekeys.
The link `zotero://select/library/items/<ITEMKEY>` opens the item in Zotero and can accompany a citation.

---

Source: the Better BibTeX documentation on citing (https://retorque.re/zotero-better-bibtex/citing/), read on 2026-10-27, for the default formula, and a setup of a Zotero connection on 2026-09-17, on Zotero 9.0.6.
Related: [Better BibTeX answers JSON-RPC calls on the Zotero port while Zotero runs](better-bibtex-json-rpc.md), [The zotero-mcp-server reads the library in local mode and holds its write switch in the environment](zotero-mcp-server.md).
