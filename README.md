# :ramen: ghc-in-browser

See [live](https://try.haskell-miso.org)

<a href="https://try.haskell-miso.org">
  <img width="463" alt="Try miso — Haskell web framework running in WebAssembly" src="https://github.com/user-attachments/assets/671e3140-4dc6-4151-8a3f-fa64066e8582" />
</a>

An interactive [miso](https://haskell-miso.org) playground running entirely in the browser. Write Haskell, click **Run**, and see your miso app rendered live — no install required. Powered by GHC compiled to WebAssembly.

## How it works

- `rootfs.tar.zst` bundles a full wasm32-wasi GHC boot-lib tree plus miso, built from a single pinned [ghc-wasm-meta](https://gitlab.haskell.org/ghc/ghc-wasm-meta) toolchain so every `.so` is ABI-consistent.
- On page load the browser extracts the rootfs in-memory (via [bsdtar.wasm](https://haskell-wasm.github.io/bsdtar-wasm/bsdtar.wasm)) and initialises a WASI filesystem.
- Clicking **Run** passes the editor contents to `libplayground001.so` — a shared-library build of the GHC API — which compiles and links the code entirely in-browser, then mounts the resulting miso app into `#app`.

## Credits

Made possible by the [GHC API browser project](https://github.com/ghc/ghc/blob/master/testsuite/tests/ghc-api-browser/playground001.hs) — thanks to [@terrorjack](https://github.com/terrorjack) for the foundational work on running GHC in the browser via WebAssembly.

## Build and run

Install [Nix](https://nixos.wiki/wiki/Flakes) (flakes enabled), then:

```
./build-root.sh
```

This rebuilds `rootfs.tar.zst` and updates `index.html` to match the new GHC. Serve locally with any static file server, e.g.:

```
python3 -m http.server
```
