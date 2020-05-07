# New Ops Engineer

## Production Engineering 
Welcome to Production Engineering & Operations, where *Reliability is our product*!

Production Engineering & Operations is really focused on two pillars:

1. Product & development velocity ~ empower others to do Ops-like work.
2. *\#BeachOps* ~ We automate so we can kick back and relax from anywhere and let systems manage themselves. We automate and build tools so we can fix while mobile and aren't tied to a desk.


# System Setup
Much of this assumes you're already a skilled Linux/OSX user.

And inspiration for much of this came from this [blog post](https://dev.to/mattstratton/my-brewfile-1pob).

## Bootstrap
Many of this has been codified in a [`Brewfile`](https://github.com/mzeier/dotfiles/blob/master/Brewfile). 

To quickly bootstrap, install [Homebrew](https://brew.sh/):

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

And run "`brew bundle`" in a directory with your `Brewfile`.

## General configurations

### Git

**`git` defaults:**

```
$ git config --global user.name "John Doe"
$ git config --global user.email "john@doe.org"
```


### Python
* `brew install pyenv`
* `pyenv global 3.8.2`

To activate, add the following to `.zshrc`:
```
$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/. zshrc

$ echo export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES >> ~/. zshrc
```

## Python PIP packages
All off my pip packages are in this [`requirements.txt`](https://github.com/mzeier/dotfiles/blob/master/requirements.txt).

The following will install these packages:
```
pip3 install -r requirements.txt
```

### AWS

You'll need IAM accounts for cli usage. 

We rely on the [AWS cli]() and it's support for [Named Profiles]().

#### Configuration File Setup
1. Create a `$HOME/.aws` directory if you don't have one.
2. Create a file named `$HOME/.aws/config` and add the following to it


    ```
    [default]
    region=us-west-2
    ```
    
    
3. Create a file named `$HOME/.aws/credentials` and add the following to it, in preparation for generating your IAM user(s) below

    ```
    [default]
    aws_access_key_id =
    aws_secret_access_key =

    [sunnylabs]
    aws_access_key_id =
    aws_secret_access_key =


    [sunnylabs-dev]
    aws_access_key_id =
    aws_secret_access_key =

    ```


## OSX Must Haves
* [Amphetamine](https://itunes.apple.com/us/app/amphetamine/id937984704?mt=12) - override energy settings
* [1Password](https://1password.com/) - secure password/MFA store
* [MacDown](https://macdown.uranusjr.com/) - Markdown editor
* XCode - prerequisite for most of `brew`

### [Atom](https://atom.io/) - editor
Useful packages:

* `autocomplete-ansible`
* `ansible-snippets`
* `atom-jinja2`
* `indent-toggle-on-paste`
* `language-ansible`
* `tab-to-spaces`
* `trailing-spaces`
* `language-terraform`
* `linter-ansible-linting`
* `linter-ansible-syntax`
* `linter-terraform-syntax`
* `markdown-preview-plux`
* `language-markdown`

`keymap.cson` bindings:

```
'.platform-darwin atom-text-editor:not(.mini)':
   'cmd-shift-v': 'indent-toggle-on-paste:paste'
```

Default themes (UI & Syntax):

* One Dark

### [PyCharm](https://www.jetbrains.com/pycharm/) - commercial editor
A little more complex than an editor like Atom, but a lot of good features
and their Terraform plugin is very solid. You can [grab a trial](https://www.jetbrains.com/pycharm/download/download-thanks.html?platform=mac) and if you like it, buy a license via [Coupa](https://myvmware.workspaceair.com/catalog-portal/ui?isOnPremise=false&isMobile=false&userId=2492128#/bookmarks/details/WORKSPACE-9efde298-9baa-4038-9feb-9d130eb11a94-Web-Saml20?nativenav=Hide)



--

# Quiz Time!

You should be able to answer the following questions:

