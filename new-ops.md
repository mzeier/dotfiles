# New Ops Engineer

## Production Engineering 
Welcome to Wavefront and welcome to Operations, where *Reliability is our product*!

Operations @ Wavefront is really focused on two pillars:

1. Product & development velocity ~ empower others to do Ops-like work.
2. *\#BeachOps* ~ We automate so we can kick back and relax from anywhere and let systems manage themselves. We automate and build tools so we can fix while mobile and aren't tied to a desk.

This document has two purposes.

1. help you get your computers configured with tooling you'll need at Wavefront.
2. get you on an expert path to familiarity with Wavefront (Day One)

# System Setup
Much of this assumes you're already a skilled Linux/OSX user.

## General configurations

## brew-based packages
Many OSX tools come from Homebrew. You'll want to install `brew` as one of your very first steps:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Git
From a terminal

* ` brew install git `

**`git` defaults:**

```
$ git config --global user.name "John Doe"
$ git config --global user.email "john@doe.org"
```

### brew packages
* `brew install packer`
* `brew install terragrunt@0.19.24`
* `brew install jq`
* `brew install gnu-sed --with-default-names`
* `brew install gpg`
* `brew cask install keepassxc`

### Python
Most current version of `python` tends to crash on OSX. 2.7.14 is known to work. Following the steps [here](https://gist.github.com/Bouke/11261620), do the following:

* `brew install readline xz openssl zlib sqlite`
* `brew install pyenv`
* `CFLAGS="-I$(brew --prefix openssl)/include -I/usr/local/opt/zlib/include -I/usr/local/opt/sqlite/include" LDFLAGS="-L$(brew --prefix openssl)/lib -L/usr/local/opt/zlib/lib -L/usr/local/opt/sqlite/lib" pyenv install -v 2.7.15`
* `pyenv global 2.7.15`

To activate, add the following to `.bashrc`:

```
export PATH="/Users/username/.pyenv:$PATH"
eval "$(pyenv init -)"

export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```

## Python PIP packages
* `pip install ansible==2.8.5`
* `pip install credstash==1.15.0`
* `pip install boto`
* `pip install boto3`
* `pip install awscli`
## Install ARA module to send output [ARA](http://ara.corp.wavefront.com) official link [here](https://ara.readthedocs.io/en/stable/installation.html)
* `pip install tox`
* `pip install psycopg2-binary`
* `pip install ara==0.16.6`   ## Version 1.x is for python3 get this version since we are still  python2

## Configure ansible.cfg to use [ARA](http://ara.corp.wavefront.com)
* `python -m ara.setup.ansible >> ~/.ansible.cfg`
* add the following to ~/.ansible.cfg
  ```
  [ara]

  database = credstash -r us-west-2 get ara.database.credentials

  ignore_parameters  =
  ```


### AWS

You'll need IAM accounts for cli usage for (at least) the following Wavefront AWS Accounts (as named in Okta):
- AWS - Sunnylabs
- AWS - Wavefront-dev

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


    [wavefront-sunnylabs]
    aws_access_key_id =
    aws_secret_access_key =

    ```

### `terraform` & `chtf`
Since `terraform` changes versions frequently, you may want to use the [Terraform version switcher](https://github.com/Yleisradio/homebrew-terraforms).

* `brew tap Yleisradio/terraforms`
* `brew install chtf`

Add the following path to allow python to work properly(since brew doesn't do this for you): /usr/local/opt/python/libexec/bin

Add the following to the `~/.bashrc` or `~/.zshrc` or `~/.bash_profile`(OSX) file:

```bash
# Source chtf
if [[ -f /usr/local/share/chtf/chtf.sh ]]; then
    source "/usr/local/share/chtf/chtf.sh"
fi

# making sure that all terminal sessions are running with terraform v0.12.8
chtf 0.12.8
```

Or select the wanted Terraform version to use with `chtf`.

    chtf 0.12.8

You can also just install a specific Terraform version (but you'll need to use `chtf` or adjust `PATH` yourself to use it):

    brew cask install terraform-0.12.8

#### Installing TF Providers (you must complete all of the python steps before finishing this step)
after checking out the terraform repository, navigate to terraform/providers
```bash
# install the providers
./link-providers.sh
```

This will install the appropriate provider for your platform into the proper directory.

Additional instructions per provider will be emitted by the script as well


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

