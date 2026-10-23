# xiayh0107/tap

Homebrew formulae for [SSH Server Manager](https://github.com/xiayh0107/servers-connect)
(`serverctl`): a local-first SSH manager secured by the OS keychain and safely
drivable by AI agents.

## Install

```bash
brew install xiayh0107/tap/ssh-server-manager
serverctl --version
```

Or `brew tap xiayh0107/tap` and then `brew install ssh-server-manager`.

In a `Brewfile`:

```ruby
tap "xiayh0107/tap"
brew "ssh-server-manager"
```

`pipx install ssh-server-manager` installs the same release from PyPI. To add the
Agent Skill for Claude Code or Codex as well, see the project's
[AI agents guide](https://github.com/xiayh0107/servers-connect/blob/main/ssh-server-manager/docs/ai-agents.md).

## Updating the formula

The formula is rendered from the sdist PyPI serves, so its hash is the one
Homebrew downloads. In a checkout of `servers-connect`, after a release has been
published to PyPI:

```bash
sdist="$(mktemp -d)/ssh_server_manager-X.Y.Z.tar.gz"
curl -fsSL -o "$sdist" https://files.pythonhosted.org/packages/source/s/ssh-server-manager/ssh_server_manager-X.Y.Z.tar.gz
ssh-server-manager/scripts/prepare_packaging "$sdist"
```

`prepare_packaging` refuses a file whose hash is not the one PyPI serves (a local
`dist/` build never is: the release workflow builds its own).

Copy `dist/packaging/homebrew/ssh-server-manager.rb` into `Formula/`, run
`brew update-python-resources xiayh0107/tap/ssh-server-manager`, and open a pull
request. `brew test-bot` builds it on macOS and Linux; once it is green, run the
**brew pr-pull** workflow with the pull request number to publish the bottles.

## Documentation

`brew help`, `man brew` or check [Homebrew's documentation](https://docs.brew.sh).
