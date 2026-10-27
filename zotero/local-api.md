# Zotero serves a read-only local API on port 23119 when other applications are allowed to connect

Zotero desktop listens on `http://localhost:23119/` while it runs.
Zotero 7 is the first version with the local API.
The local API under `http://localhost:23119/api/users/0/` mirrors the web API of zotero.org and answers GET requests only, for items, collections, tags, saved searches, the full-text index and the locations of attachments.
The local API is off by default.
The setting that turns it on is in Settings, Advanced: "Allow other applications on this computer to communicate with Zotero".
The local API answers a request with 403 while the setting is off and with 200 once it is on.

```sh
curl -s 'http://localhost:23119/api/users/0/items?limit=1' -o /dev/null -w '%{http_code}\n'
curl -s 'http://localhost:23119/api/users/0/items?q=co-design&qmode=everything&limit=10&format=json'
curl -s 'http://localhost:23119/api/users/0/collections?limit=200'
curl -s 'http://localhost:23119/api/users/0/items/<ITEMKEY>/children'
curl -s 'http://localhost:23119/api/users/0/items/<ATTACHMENTKEY>/file/view/url'
```

The last call returns the `file://` URL of an attachment.

The connector endpoints under `http://localhost:23119/connector/` serve the browser connector and are on whenever Zotero runs, whatever the setting.
`/connector/ping` answers "Zotero is running".
`/connector/saveItems`, `/connector/saveSnapshot` and `/connector/import` create items without any key, so a connection that must not write never posts to them.
Zotero 10.0.5 serves more endpoints that change the library without a key: `saveStandaloneAttachment`, `saveSingleFile`, `saveAttachment` and `saveAttachmentFromResolver` save attachments, `updateSession` changes the collection and the tags of the items of a save, and `installStyle` installs a citation style.
Zotero 10 and later can grant a local API key that permits writes through the local API, and without such a key the local API stays read-only.
A post to `/api/local/authorize` asks Zotero for such a key, and Zotero then shows a dialog in which the user may choose "Always Allow".

The data directory is `~/Zotero` by default, and the preference `extensions.zotero.dataDir` sets another.
The attachment of an item with key `<KEY>` is stored under `~/Zotero/storage/<KEY>/`.

---

Source: a setup of a Zotero connection on 2026-09-17, on Zotero 9.0.6.
The endpoints of Zotero 10.0.5 were read from `server_connector.js` and `server_localAPI.js` in its `omni.ja` on 2026-10-08.
Related: [Better BibTeX answers JSON-RPC calls on the Zotero port while Zotero runs](better-bibtex-json-rpc.md), [The zotero-mcp-server reads the library in local mode and holds its write switch in the environment](zotero-mcp-server.md), [A Zotero item key and a Better BibTeX citekey name the same item](item-key-and-citekey.md).
