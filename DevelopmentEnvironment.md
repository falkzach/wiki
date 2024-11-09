# Development Environment

## Extending .bashrc

Extend your `.bashrc` file to more easily manage environment variables and profile changes.

Below the existing bash_aliases section, add bash_env and bash_profile

`.bashrc`

```bash
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# Environment variable definitions.

if [ -f ~/.bash_env ]; then
    . ~/.bash_env
fi

# Profile definitions.

if [ -f ~/.bash_profile ]; then
    . ~/.bash_profile
fi
```

`.bash_aliases`

```bash
alias myalias="~/scripts/myscript.sh"
alias d=doctl
alias k=kubectl
alias u="cat /proc/sys/kernel/random/uuid"
```

`.bash_env`

```bash
export MYENVAR=MYENVARVALUE
```

`.bash_profile`

```bash
export PATH="$PATH:/home/falkzach/.local/bin"
```

## Git setup

- [generate ssh key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
- [add gpg key](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)

```bash
git config --global user.name "Zachary Falkner"
git config --global user.email falkzach@gmail.com
git config --global init.defaultBranch main
git config --global commit.gpgsign true
git config --global user.signingkey 2966374CB10156615982E3F49A6378FCB050C64B
git config --global core.autocrlf
```

See [Pgp Certificates](./PgpCertificates.md) for moving certificates to other systems.

## Fira Code

[Fira Code](https://github.com/tonsky/FiraCode): free monospaced font with programming ligatures

```bash
sudo apt install fonts-firacode
```

## VS code

`~/.config/Code/User/settings.json`

```json
{
  "files.trimFinalNewlines": true,
  "files.insertFinalNewline": true,
  "git.autofetch": true,
  "explorer.confirmDragAndDrop": false,
  "git.enableSmartCommit": true,
  "editor.rulers": [80, 120],
  "editor.fontFamily": "Fira Code",
  "editor.fontLigatures": true,
  "terminal.integrated.fontFamily": "monospace"
}
```

## NVM

It is preferable to use [NVM](https://github.com/nvm-sh/nvm) to manage Node versions.

In a project with a `.nvmrc` file

```bash
nvm install
nvm use
```

On a new project be sure to install the latest lts create the `.nvmrc` file

```bash
nvm install --lts
node -v > .nvmrc
```

### Husky Pre Commit Hooks

To run pre-commit hooks setup a `~/.huskyrc` file

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

## Dotnet

See [Install dotnet](https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu) and configured either the ubuntu backports or microsoft sources. Then install your desired dotnet sdk.

```bash
sudo apt update
sudo apt upgrade dotnet-sdk-8.0
```
