# The zotero-mcp-server reads the library in local mode and holds its write switch in the environment

`zotero-mcp-server` is a Python package that serves the Zotero library to an agent through the Model Context Protocol, and it needs Python 3.10 or newer.
The package is installed as a tool with `uv tool install --python <python 3.10 or newer> zotero-mcp-server`, which puts the commands `zotero-mcp`, `zotero-mcp-server` and `zotero-cli` in `~/.local/bin`.
When that folder is not on the PATH, a configuration names the command by its full path.
A configuration that serves several machines writes the path from the home folder, such as `${HOME}/.local/bin/zotero-mcp` on a platform that expands variables in its configuration.
`zotero-mcp serve` starts the server, and `zotero-mcp version` prints the installed version.
`zotero-mcp-server` is a second name of `zotero-mcp`.
`zotero-cli` reads and writes the library from a shell without the server, with commands such as `add`, `edit`, `delete`, `notes` and `collections`, and `zotero-cli db update` builds the vector index.
A shell leaves `ZOTERO_LOCAL_WRITE` unset, and the package reads an unset switch as on, so the switch of the server does not hold for `zotero-cli`.
The folder of the tool also holds the command `pyzotero`, whose commands `createitem`, `createcollection`, `addtocollection`, `movetocollection` and `removefromcollection` write with a local API key, and whose command `authorize` asks Zotero for such a key.

The environment of the server selects its mode, as the table gives.

| Variable | Value | Effect |
|---|---|---|
| `ZOTERO_LOCAL` | `true` | local mode, which reads `zotero.sqlite` and the local API and needs no zotero.org account or key |
| `ZOTERO_LOCAL_WRITE` | `1` | local writes switched on, the server's own switch, which `0` switches off |
| `ZOTERO_MCP_TOOLSETS` | `none` | the core tools only, 35 instead of about 50, without the groups libraries, search-admin and pdf-geometry, where the value `pdf-geometry` adds the page layout and outline tools |
| `ZOTERO_MCP_DB_SNAPSHOT` | `0` | no temporary copy of the database, while an unset variable makes the server copy it |
| `ZOTERO_API_KEY`, `ZOTERO_LIBRARY_ID` | unset | the web API of zotero.org, which permits writes, stays unused |

In local mode the server keeps a temporary snapshot of `zotero.sqlite` in the system temp directory while it runs, and removes the snapshot afterwards.
Edits made in Zotero on the same day are therefore visible.
The tool `zotero_write_capabilities` reports the mode, and "Mode: none" with "Local writes are switched off by ZOTERO_LOCAL_WRITE" and no web credentials means that no write reaches the library through the local API or the web API.
The note tool `zotero_manage_note` still creates a standalone note in that mode, because it falls back to the connector endpoint `/connector/saveItems` when no client can write, as `tools/annotations.py` of 0.12.4 shows.

Five commands of `zotero-mcp` change more than the server.
`zotero-mcp authorize-local` obtains a local API key from Zotero 10 or later, once the user chooses "Always Allow" in the dialog that Zotero shows.
That key permits writes.
The command stores the key under `local_api` in `~/.config/zotero-mcp/config.json`, readable by the owner alone.
"Allow" in place of "Always Allow" gives a key for one write, which the command does not store.
The server takes the key first from a grant in its own process, then from `ZOTERO_LOCAL_API_KEY`, then from that file.
`zotero-mcp setup` writes the configuration of Claude Desktop and may store API keys.
`zotero-mcp install-skill` installs an agent skill into each agent platform that it finds on the machine.
`zotero-mcp update-db` builds a vector index under `~/.config/zotero-mcp/chroma_db` with calls to an embedding API, and the tool `zotero_semantic_search` works only with that index.
`zotero-mcp batch-import`, with its alias `openai-batch-import`, imports the embeddings of a batch into the same index.
`zotero-mcp --help` lists every command.
The optional extra `semantic` of the package installs the dependencies of the index, `chromadb` and `sentence-transformers`.
The extras `all` and `dev` install the same dependencies.

The read tools of the core set and their main arguments are the following.

| Task | Tool | Main arguments |
|---|---|---|
| Find items | `zotero_search_items` | `query`, `qmode` with `titleCreatorYear` or `everything`, `item_type`, `tag`, `collection_key`, `limit` |
| Search with conditions | `zotero_advanced_search` | conditions |
| Citekey to item | `zotero_search_by_citation_key` | `citekey` |
| Items by tag | `zotero_search_by_tag` | tag |
| Collection tree | `zotero_get_collections`, `zotero_search_collections`, `zotero_get_collection_items` | `limit`, `collection_key` |
| Item details | `zotero_get_item_metadata`, `zotero_get_item_children` | `item_key` |
| Full text | `zotero_get_item_fulltext`, from Zotero's index, and `zotero_read_pdf_pages` | `item_key`, `start_page`, `end_page` |
| PDF path | `zotero_get_attachment_path` | `item_key` of the parent or of the attachment |
| Notes and annotations | `zotero_get_notes`, `zotero_get_annotations`, `zotero_synthesize_annotations` | `item_key` |
| Bibliography text | `zotero_export_bibliography` | `item_keys` or `collection_key`, `style`, `export_format` with `bib`, `citation` or `bibtex` |
| Recent items and tags | `zotero_get_recent`, `zotero_get_tags` | |
| Mode of the server | `zotero_write_capabilities` | |

BibLaTeX and CSL JSON are not among the export formats of `zotero_export_bibliography` and come from the `item.export` method of Better BibTeX.
With the default `limit`, `zotero_get_collections` returns the first 100 collections and cuts the list there silently.
The tool accepts a `limit` up to 5000.
A library of more than 100 collections lists only part of its tree with the default, so a full tree needs a larger `limit` or the endpoint `collections` of the local API.
`uv tool upgrade zotero-mcp-server` updates the server, and the names of the tools and the prompts of the installed version are listed with

```sh
grep -rho 'name="zotero_[a-z_]*"' ~/.local/share/uv/tools/zotero-mcp-server/lib/python3.*/site-packages/zotero_mcp | sort -u
```

---

Source: zotero-mcp-server 0.12.4, installed with uv 0.12.15 and Python 3.11 in a setup of a Zotero connection on 2026-09-17.
The facts on the commands, the extras and the note tool were read from the help, the metadata and the source of 0.12.4 on 2026-10-08.
The default and the maximum of `limit` of `zotero_get_collections` were read from `zotero_mcp/tools/retrieval.py` of 0.12.4 on 2026-10-11, after the default hid a collection of a library of more than 100 collections.
The storage of the key and the order in which the server reads it were read from `zotero_mcp/client.py` and `zotero_mcp/cli.py` of the installed version on 2026-10-24, after Yujun Huang's request of 2026-10-23 to allow writes to Zotero after a prompt.
Related: [Zotero serves a read-only local API on port 23119 when other applications are allowed to connect](local-api.md), [Better BibTeX answers JSON-RPC calls on the Zotero port while Zotero runs](better-bibtex-json-rpc.md).
