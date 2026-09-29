# pyxis-lwip

Pinned lwIP sources for Caelum's TCP integration. The source selection, upstream
revision and local changes are recorded in [UPSTREAM.md](UPSTREAM.md); see
[COPYING](COPYING) for the upstream license.

Pyxis OS consumes this repository as `third_party/lwip`. Its configuration,
freestanding headers, allocator/clock hooks and build rules live in
`kernel/net/lwip` in [pyxis-os](https://git.internal/PyxisOS/pyxis-os).
Build from that parent checkout with `make kernel` or `make image`.

## License

lwIP sources and local source adaptations retain their [BSD license](COPYING).
Original Pyxis repository documentation uses [MPL-2.0](LICENSE); see
[LICENSING.md](LICENSING.md) for the boundary.
