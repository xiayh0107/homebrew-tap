# xiayh0107/tap

Homebrew formulae for [Hoploom](https://github.com/xiayh0107/hoploom)
(`serverctl`): a local-first SSH manager secured by the OS keychain and safely
drivable by AI agents.

## Install

```bash
brew install xiayh0107/tap/hoploom
hoploom --version
```

Or `brew tap xiayh0107/tap` and then `brew install hoploom`. Existing
`ssh-server-manager` installations migrate through the tap rename mapping;
`serverctl` remains a compatible command. Inventory and OS-vault namespaces stay
unchanged.

In a `Brewfile`:

```ruby
tap "xiayh0107/tap"
brew "hoploom"
```

`pipx install hoploom` installs the same release from PyPI. To add the
Agent Skill for Claude Code or Codex as well, see the project's
[AI agents guide](https://github.com/xiayh0107/hoploom/blob/main/ssh-server-manager/docs/ai-agents.md).

## Updating the formula

The formula is rendered from the sdist PyPI serves, so its hash is the one
Homebrew downloads. In a checkout of `hoploom`, after a release has been
published to PyPI:

```bash
hoploom_version=X.Y.Z
sdist="$(mktemp -d)/hoploom-$hoploom_version.tar.gz"
pypi_url="https://files.pythonhosted.org/packages/source/h/hoploom"
curl -fsSL -o "$sdist" "$pypi_url/hoploom-$hoploom_version.tar.gz"
ssh-server-manager/scripts/prepare_packaging "$sdist"
```

`prepare_packaging` checks the sdist hash against PyPI. Download the published
file before rendering; the release workflow builds its own archive.

Copy `dist/packaging/homebrew/hoploom.rb` into `Formula/`, retaining the
tap's `pypi_packages` configuration. Run
`brew update-python-resources xiayh0107/tap/hoploom` and open a pull request. `brew test-bot` builds it on macOS and Linux; once it is green, run the
**brew pr-pull** workflow with the pull request number to publish the bottles.
For a root release uploaded less than a day ago, add
`--ignore-main-package-cooldown`; keep the dependency upload cooldown in place.
The formula records maturin and the Linux keyring packages in `pypi_packages`.
Keep the `jeepney` and `secretstorage` resource blocks inside `on_linux` after
regeneration.

Hoploom includes its native executor. Formula builds require Rust and
maturin; the formula test executes the installed native extension without
accessing the OS vault or contacting SSH hosts. Render hashes from PyPI's
actual sdist, including after a workflow rebuild.

## Documentation

`brew help`, `man brew` or check [Homebrew's documentation](https://docs.brew.sh).
