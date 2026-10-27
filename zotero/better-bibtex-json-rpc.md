# Better BibTeX answers JSON-RPC calls on the Zotero port while Zotero runs

The Better BibTeX plugin adds the endpoint `http://localhost:23119/better-bibtex/json-rpc`, which accepts POST requests with a JSON-RPC 2.0 body.
The endpoint is on whenever Zotero runs with the plugin.
The methods that the skills of the ai-workspace use are the following.

- `item.search` takes free text and returns items with their citekeys.
- `item.attachments` returns the absolute paths and the `zotero://` links of the attachments of a citekey.
- `item.export` returns BibTeX, BibLaTeX or CSL JSON for a list of citekeys.
- `item.notes` returns the notes of a list of citekeys.
- `item.citationkey` maps item keys to citekeys.

```sh
BBT=http://localhost:23119/better-bibtex/json-rpc
H='Content-Type: application/json'
curl -s -X POST $BBT -H "$H" -d '{"jsonrpc":"2.0","method":"item.search","params":["co-design"],"id":1}'
curl -s -X POST $BBT -H "$H" -d '{"jsonrpc":"2.0","method":"item.attachments","params":["censiMathematicalTheoryCoDesign2016"],"id":2}'
curl -s -X POST $BBT -H "$H" -d '{"jsonrpc":"2.0","method":"item.export","params":[["censiMathematicalTheoryCoDesign2016"],"biblatex"],"id":3}'
curl -s -X POST $BBT -H "$H" -d '{"jsonrpc":"2.0","method":"item.notes","params":[["censiMathematicalTheoryCoDesign2016"]],"id":4}'
```

The export formats accepted by `item.export` include `biblatex`, `bibtex` and `csljson`.
A JSON answer to `item.search` shows that the plugin is reachable, which is the check used when the connection is set up.

Three methods of Better BibTeX 9.0.71 write to the library.
`collection.scanAUX` fills a collection with the items that the citekeys of an AUX file name.
`autoexport.add` adds an automatic export, and creates its collection when the collection is missing.
`item.regenerate_key` changes the citekeys of items.

---

Source: a setup of a Zotero connection on 2026-09-17, on Zotero 9.0.6.
The methods that write were read from `content/better-bibtex.js` of Better BibTeX 9.0.71 on 2026-10-08.
Related: [Zotero serves a read-only local API on port 23119 when other applications are allowed to connect](local-api.md), [A Zotero item key and a Better BibTeX citekey name the same item](item-key-and-citekey.md).
