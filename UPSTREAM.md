# lwIP source provenance

Pinned to `STABLE-2_2_1_RELEASE`, commit
`77dcd25a72509eb83f72b033d219b1d40cd8eb95`:
https://github.com/lwip-tcpip/lwip/tree/77dcd25a72509eb83f72b033d219b1d40cd8eb95

`COPYING` contains the upstream BSD license. Source headers retain their notices
and formatting. Imported files retain the pinned upstream contents except for the local changes below.

The import contains sixteen core C files: `init`, `def`, `inet_chksum`, `ip`,
`mem`, `memp`, `netif`, `pbuf`, `stats`, `sys`, `tcp`, `tcp_in`, `tcp_out`,
`timeouts`, `ipv4/ip4` and `ipv4/ip4_addr`, under `src/core`.
It also contains the 61 headers consumed by Caelum's initial build profile,
plus upstream `README` and `COPYING`. Header selection follows the
compiler-generated dependency files, including upstream's unconditional
cross-protocol includes. Application, socket/netconn, driver, test and upstream
build-system sources are not imported. A later profile change may require more
files from this same pin; do not substitute locally invented upstream headers.

This repository owns the upstream source selection and any future local patches.
Record each adaptation here, preserving upstream license notices and formatting.
Caelum-specific configuration, port hooks and integration documentation belong
in pyxis-os. The kernel uses the native build rules rather than an upstream build system.

## Local changes

- `src/core/ipv4/ip4.c`: `LWIP_HOOK_IP4_ROUTE_SRC` is authoritative, including
  a NULL result and a NULL source. Neither entry point falls back to lwIP's
  netif/default-route search when the hook is defined. New
  `LWIP_HOOK_IP4_INPUT_ACCEPT(netif, destination)` decides local-address
  acceptance, including Caelum's full 127/8 range. Ordinary IP/TCP validation
  remains in lwIP; Caelum validates ingress before calling it as well.
- `src/core/tcp.c`: allocation failure returns NULL without evicting another
  PCB, attempting unrelated pending closes or shortening TIME_WAIT. The
  associated eviction helpers are removed. New
  `LWIP_HOOK_TCP_PCB_ALLOCATED(pcb)` runs after PCB initialization and before
  publication; a non-ERR_OK result frees the PCB through its normal destructor
  and fails allocation. Caelum attaches a packet-generation record through the
  existing extension-argument API and its destruction callback. When converting
  to a listener, extension ownership moves to the new PCB: clear the old copy
  before freeing it to avoid destroying the transferred state twice. Caelum
  does not yet expose listening.
- `src/core/tcp_out.c`: new
  `LWIP_HOOK_TCP_OUTPUT(pcb, pbuf, source, destination, ttl, tos, netif)` wraps
  both data and control IP submissions. The PCB is borrowed and may be NULL for
  stateless control output. The hook must return an lwIP error, preserve pbuf
  ownership, and call IP output to construct the normal IPv4 header. Caelum
  scopes the connection generation to that synchronous call; no PCB pointer
  escapes into its packet queues.

The port retains upstream packet/state/congestion-control algorithms. No host
library, socket API, driver or alternative allocator is added here.
