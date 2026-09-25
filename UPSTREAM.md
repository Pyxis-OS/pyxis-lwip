# lwIP source provenance

Pinned to `STABLE-2_2_1_RELEASE`, commit
`77dcd25a72509eb83f72b033d219b1d40cd8eb95`:
https://github.com/lwip-tcpip/lwip/tree/77dcd25a72509eb83f72b033d219b1d40cd8eb95

`COPYING` contains the upstream BSD license. Source headers retain their notices
and formatting. All imported upstream files are unmodified.

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
in pyxis-os. The initial import establishes only the source dependency;
no routing, loopback, allocation-policy or other upstream patches are applied.
