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
## Installing an older nightly, or staying on one

Every night's cask is a commit in this tap, so the git history is the archive. Homebrew
has no `@version` lookup for casks (only for formulae), but installing from a past commit
is two commands:

```sh
git -C "$(brew --repository aurelienpierreeng/ansel)" log --oneline -- Casks/ansel-nightly.rb   # pick a night
git -C "$(brew --repository aurelienpierreeng/ansel)" show <commit>:Casks/ansel-nightly.rb > /tmp/ansel-nightly.rb
brew install --cask /tmp/ansel-nightly.rb
```

To stay on the version you have, just don't `brew upgrade --cask ansel-nightly` — `brew pin`
does not apply to casks. This works because the cask carries a real version string; a
`version :latest` cask would have nothing to find in history.

## Stable releases

A separate `ansel` cask will appear here with the first tagged release, so the stable and
nightly channels can be installed side by side.
