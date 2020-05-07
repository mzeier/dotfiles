# New Ops Engineer

## Ops @ Wavefront
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

### Repositories to Checkout

- ansible: https://phabricator.corp.wavefront.com/source/ansible/ - `git clone ssh://git@phabricator.corp.wavefront.com/source/ansible.git`
- postboot: https://phabricator.corp.wavefront.com/source/postboot/ - `git clone ssh://git@phabricator.corp.wavefront.com/source/postboot.git`
- terraform: https://phabricator.corp.wavefront.com/source/terraform/ - `git clone ssh://git@phabricator.corp.wavefront.com/source/terraform.git`
- packer: https://phabricator.corp.wavefront.com/source/packer/ - `git clone ssh://git@phabricator.corp.wavefront.com/source/packer.git`
- devops (tools): https://phabricator.corp.wavefront.com/source/devops/ - `ssh://git@phabricator.corp.wavefront.com/source/devops.git`
- ops (tools and configs): https://phabricator.corp.wavefront.com/source/ops/ - `git clone ssh://git@phabricator.corp.wavefront.com/source/ops.git`
- k8s (kubernetes manifests): https://phabricator.corp.wavefront.com/source/k8s/ - `git clone ssh://git@phabricator.corp.wavefront.com/source/k8s.git`
- docs: https://phabricator.corp.wavefront.com/source/docs - `git clone ssh://git@phabricator.corp.wavefront.com/source/docs.git`

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

#### Generating your IAM user for AWS - Sunnylabs (prod account)
1. Using Okta, log into the AWS-Sunnylabs account and create yourself a programmatic user
2. Under the IAM, use the Add User button to create the account, then choose a username, access type (Programmatic access)
3. next, then add yourself to these groups: Administrators and credstash-access groups.
4. Review and finish
5. You will be provided with an access ID and an access key
6. Save these and add them to your ~/.aws/credentials file on your workstation.
  * **Note:** You'll set your sunnylabs IAM user as your default Named Profile.

#### Generating your IAM user for other AWS accounts
1. Using Okta, log into the AWS - Wavefront-dev account and create yourself a programmatic user
2. Under the IAM, use the Add User button to create the account, then choose a username, access type (Programmatic access)
3. next, then add yourself to these groups: Administrators and credit-stash-access groups.
4. Review and finish
5. You will be provided with an access ID and an access key
6. Save these and add them to your ~/.aws/credentials file on your workstation.

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

### Get setup with phabricator and install `arc`

* Create account on phacility ?
* Get invited to org
* Install `arc`, https://secure.phabricator.com/book/phabricator/article/arcanist_quick_start/
```
mkdir ~/code/tools
cd ~/code/tools
git clone ...
```


### Import gpg private

* Import gpg private
```
AWS_PROFILE=sunnylabs credstash -r us-west-2 get keybase.wfops.private.encoded | base64 -D | gpg --import
```

### OpenVPN
Visit this site in Confluence (https://wavefront.atlassian.net/wiki/spaces/OBR/pages/108006545/Connect+to+OpenVPN) pay particular attention to completing steps 3 and 4.
- Consider buying and expensing [Viscocity](https://www.sparklabs.com/viscosity/)

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

# Day One
On your first day, you'll want to quickly come up to speed on all things Ops @ Wavefront.

Everything here is geared towards getting you familiar with the Wavefront environment and systems and with ensuring your workstation - as well as any access or credentials - are setup.

## phabricator workflow for code review

1. `arc diff`
2. filling out template for diff
3. code review
4. `arc land` (merges to master)

## New cluster spinup

* Build a new cluster
* Upgrade the cluster version
* Upsize & contract fdb, including rolling YAML updates
* Destroy cluster

Things you will learn and configure:

* Phabricator & GitHub
* AWS credentials & `~/.aws/credentials`
* VPN access
* Current release / deployment process
* Working Terraform & Ansible
* Jenkins access

## Quiz Time!

You should be able to answer the following questions:

* What happens at Wavefront when an instance boots up?
* What is LandingParty and what does it do? ExtractionParty?
* Difference between Reported Points and Collector Points.
* What FDB Flow Control is and what happens when it's too high (bonus points, what measures flow control)?
* What is pushback and what is the customer impact?
* what types of pushback are there and what do they do?

### Basics
* In your own words, what is Wavefront's architecture? What are all the tiers & AWS components involved?
* What do all the Jars do?
* What is `webconfig` and what does it do on the HA tier? What does "*flapping webconfig*" mean?
* What does `webconfig` do on the App Tier?
* Life cycle of a single telemetry point. What happens once it leaves `telegraf`?
* What are the three common Ansible playbooks we use for each tier and some of the most common tags used?
* In your own words, what is the process to upsize or downsize a cluster? Where is the runbook for this? Where are the Ansible Playbooks?
* When can you restart DI with 0 second pause? When or why wouldn't you?
* Why and how do config changes take effect?

### FoundationDB
* What are storage/log queues?
* What does "*cluster operating space*" or "*op space*" mean? What happens if it becomes too low and what are the steps to resolve?
* What are the inputs into SSD performance issues? What knobs or tools can you use to relieve pressure?
* What are the inputs into Mem tier performance issues and what knobs or tools can you use to relieve pressure?
* What does "`unreachable processes`" mean and how do you troubleshoot them? How is this reflected in [fdbhealh](https://mon.wavefront.com/dashboard/fdbhealth)?
* Where is the Fdb troubleshooting documenet?
* What is `loghead` and what does it do?
* What is EnhanceIO and how do we use it and why?
* What is `killFdbProcesses.sh`? Where is it located, what does it do and what is the proper way to use it?

### Tech Talk Videos
* List: https://wavefront.atlassian.net/wiki/spaces/ENG/pages/17072169/Tech+Talks
* BCS Recording: https://vmware.zoom.us/recording/play/birre_QtYt18ZCCIvX939hQyRFsV9Ty_na5fz3QE_CJ2m53yO_wglre94TsND9Mj
  * Quiz: https://github.com/sunnylabs/docs/tree/master/quiz/bcs.md
  * Answers: https://github.com/sunnylabs/docs/tree/master/quiz/bcs-answers.md
