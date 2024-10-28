# Development Environment

## extending .bashrc

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
```

`.bash_env`
```bash
export MYENVAR=MYENVARVALUE
```

`.bash_profile`
```bash
export PATH="$PATH:/home/falkzach/.local/bin"
export PATH="$PATH:/bin/elixir"
```

## git setup

* [generate ssh key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
* [add gpg key](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)

```bash
git config --global user.name "Zachary Falkner"
git config --global user.email falkzach@gmail.com
git config --global init.defaultBranch main
git config --global commit.gpgsign true
git config --global core.autocrlf
```

## vs code

`~/.config/Code/User/settings.json`
```json
{
    "files.trimFinalNewlines": true,
    "files.insertFinalNewline": true,
    "git.autofetch": true,
    "explorer.confirmDragAndDrop": false,
    "git.enableSmartCommit": true,
    "editor.rulers": [
        80, 120
    ]
}

```
