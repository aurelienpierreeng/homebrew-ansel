# Homebrew tap for Ansel nightly builds

```sh
brew tap aurelienpierreeng/ansel
brew install --cask ansel-nightly
brew upgrade --cask ansel-nightly     # or just: brew upgrade
```

`Casks/ansel-nightly.rb` is **generated** — rewritten after every nightly by the
`Nightly manifest` workflow in [aurelienpierreeng/ansel](https://github.com/aurelienpierreeng/ansel)
(`tools/nightly_manifest.py`), from the same manifest that drives the ansel.photos
download buttons. Do not edit it by hand; a fix belongs in the generator.

The cask carries a real version (`0.0.0+<commits>.g<hash>`) rather than `version :latest`
on purpose: plain `brew upgrade` skips `:latest` casks unless run with `--greedy`, and the
point of a nightly is to be upgraded.

Each architecture pins its own version, sha256 and URL, because the two dmgs are built by
separate runners and one can fail a night the other succeeded.

Nightly builds are **not signed or notarized**. Gatekeeper quarantines the app; the first
launch needs right-click › Open, or `xattr -dr com.apple.quarantine /Applications/Ansel.app`.
See `doc/nightly-distribution.md` in the ansel repository for what signing would take.