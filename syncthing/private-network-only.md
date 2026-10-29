# Syncthing keeps its traffic on a private network such as a Tailscale tailnet once discovery, relays and NAT traversal are off and each device has its address on that network

Syncthing finds the address of another device in four ways besides the addresses that the configuration lists.
Global discovery asks the public discovery servers, local discovery broadcasts on the local network, relays carry the traffic of two devices that cannot connect directly, and NAT traversal opens a port on the router.
The four options below turn them off on a running Syncthing 2.1.7, and take effect without a restart:

```sh
syncthing cli config options global-ann-enabled set false
syncthing cli config options local-ann-enabled set false
syncthing cli config options relays-enabled set false
syncthing cli config options natenabled set false
```

A device then connects only to the addresses that its configuration lists for the other device.
The address of a device on a tailnet is its MagicDNS name or its Tailscale address, with the port 22000 on which Syncthing listens by default:

```sh
syncthing cli config devices add --device-id <device ID> --name <name> --addresses tcp://<MagicDNS name>:22000
```

Syncthing 2.1.7 on macOS lists the Tailscale IPv6 address of the machine among its own addresses, and leaves out its Tailscale IPv4 address in `100.64.0.0/10`.
A device therefore learns the IPv4 address of a tailnet peer only from its configuration.
`syncthing device-id` prints the identifier of the device on which it runs.

Two devices share a folder when each lists the other and one adds the other to the folder:

```sh
syncthing cli config folders <folder ID> devices add --device-id <device ID>
```

The other device then shows the folder under `syncthing cli show pending folders`, until a folder with the same folder ID is added there with `syncthing cli config folders add --id <folder ID> --path <path>` and shared back.
The web interface listens on `127.0.0.1:8384` by default, so only the machine itself reaches it.

---

Source: the output of `syncthing cli config ... --help` and of the REST endpoint `/rest/system/status` of Syncthing 2.1.7 on macOS on 2026-10-28, and the thread "Syncthing + Tailscale" at https://forum.syncthing.net/t/syncthing-tailscale/15293, read on 2026-10-28.
Related: knowledge item `public-knowledge-base/syncthing/change-detection-and-conflicts.md`, knowledge item `public-knowledge-base/syncthing/ignore-files.md`.
