---
title: Signing Commits with GPG
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Signing Commits with GPG](#signing-commits-with-gpg)
  - [GPG](#gpg)
    - [Resource](#resource)
    - [Generating a new GPG key by using command line](#generating-a-new-gpg-key-by-using-command-line)
      - [List GPG keys for our use](#list-gpg-keys-for-our-use)
      - [Printing PGP Public key](#printing-pgp-public-key)
      - [Adding a new GPG key to your platform](#adding-a-new-gpg-key-to-your-platform)
    - [Generating GPG key in Client](#generating-gpg-key-in-client)
  - [Configure Git Client](#configure-git-client)
    - [Telling Git about your GPG key](#telling-git-about-your-gpg-key)
    - [Telling Git about your X.509 key](#telling-git-about-your-x509-key)
    - [Configuration in SourceTree](#configuration-in-sourcetree)
  - [Reference](#reference)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Signing Commits with GPG

[TOC]

## GPG

### Resource

- [GnuPG *(Official Site)*](https://www.gnupg.org/)
- [GPGTools *(macOS)*](https://gpgtools.org/)
- [GPG4Win *(Windows)*](https://www.gpg4win.org/)

### Generating a new GPG key by using command line

```shell
gpg --full-generate-key
```

🤪Finish the rest configuration

#### List GPG keys for our use

```shell
gpg --list-secret-keys --keyid-format LONG
```

>We will got some keys. 
>
>Remember the info down below.
>
>\-------------------------------
>
>sec   rsa4096/9ED0436D3E1B835B 2020-09-08 [SC]
>
>​      59AF979FCA73ABE3EDEADFE29ED0436D3E1B835B
>
>uid                 [ultimate] Frank (iMac) <472730949@qq.com>
>
>ssb   rsa4096/62632B2510D1F425 2020-09-08 [E]

`9ED0436D3E1B835B` is our GPG key ID

#### Printing PGP Public key

```shell
gpg --armor --exprot 9ED0436D3E1B835B
```

#### Adding a new GPG key to your platform

Copy your PGP public key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`

### Generating GPG key in Client

🤪Check the resource manual page.

## Configure Git Client

### [Telling Git about your GPG key](https://docs.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key#telling-git-about-your-gpg-key)

If you're using a GPG key that matches your committer identity and your verified email address associated with your GitHub account, then you can begin signing commits and signing tags.

If you don't have a GPG key that matches your committer identity, you need to associate an email with an existing key. For more information, see "[Associating an email with your GPG key](https://docs.github.com/en/articles/associating-an-email-with-your-gpg-key)".

If you have multiple GPG keys, you need to tell Git which one to use.

1. Open Terminal.

2. Use the `gpg --list-secret-keys --keyid-format LONG` command to list GPG keys for which you have both a public and private key. A private key is required for signing commits or tags.

   ```shell
   $ gpg --list-secret-keys --keyid-format LONG
   ```

   **Note:** Some GPG installations on Linux may require you to use `gpg2 --list-keys --keyid-format LONG` to view a list of your existing keys instead. In this case you will also need to configure Git to use `gpg2` by running `git config --global gpg.program gpg2`.

3. From the list of GPG keys, copy the GPG key ID you'd like to use. In this example, the GPG key ID is `3AA5C34371567BD2`:

   ```shell
   $ gpg --list-secret-keys --keyid-format LONG
   /Users/hubot/.gnupg/secring.gpg
   ------------------------------------
   sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
   uid                          Hubot 
   ssb   4096R/42B317FD4BA89E7A 2016-03-10
   ```

4. To set your GPG signing key in Git, paste the text below, substituting in the GPG key ID you'd like to use. In this example, the GPG key ID is `3AA5C34371567BD2`:

   ```shell
   $ git config --global user.signingkey 3AA5C34371567BD2
   ```

5. If you aren't using the GPG suite, paste the text below to add the GPG key to your bash profile:

   ```shell
   $ test -r ~/.bash_profile && echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile
   $ echo 'export GPG_TTY=$(tty)' >> ~/.profile
   ```

   **Note:** If you don't have `.bash_profile`, this command adds your GPG key to `.profile`.

### [Telling Git about your X.509 key](https://docs.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key#telling-git-about-your-x509-key)

You can use [smimesign](https://github.com/github/smimesign) to sign commits and tags using S/MIME instead of GPG.

**Note:** S/MIME signature verification is available in Git 2.19 or later. To update your version of Git, see the [Git](https://git-scm.com/downloads) website.

1. Install [smimesign](https://github.com/github/smimesign#installation).
2. Open Terminal.
3. Configure Git to use S/MIME to sign commits and tags. In Git 2.19 or later, use the `git config gpg.x509.program` and `git config gpg.format` commands:

- To use S/MIME to sign for all repositories:

  ```shell
  $ git config --global gpg.x509.program smimesign
  $ git config --global gpg.format x509
  ```

- To use S/MIME to sign for a single repository:

  ```shell
  $ cd /path/to/my/repository
  $ git config --local gpg.x509.program smimesign
  $ git config --local gpg.format x509
  ```

  In Git 2.18 or earlier, use the

   

  ```
  git config gpg.program
  ```

   

  command:

- To use S/MIME to sign for all repositories:

  ```shell
  $ git config --global gpg.program smimesign
  ```

- To use S/MIME to sign for a single repository:

  ```shell
  $ cd /path/to/my/repository
  $ git config --local gpg.program smimesign
  ```

  If you're using an X.509 key that matches your committer identity, you can begin signing commits and tags.

1. If you're not using an X.509 key that matches your commiter identity, list X.509 keys for which you have both a certificate and private key using the

    

   ```
   smimesign --list-keys
   ```

    

   command.

   ```shell
   $ smimesign --list-keys
   ```

2. From the list of X.509 keys, copy the certificate ID of the X.509 key you'd like to use. In this example, the certificate ID is

    

   ```
   0ff455a2708394633e4bb2f88002e3cd80cbd76f
   ```

   :

   ```shell
   $ smimesign --list-keys
                ID: 0ff455a2708394633e4bb2f88002e3cd80cbd76f
               S/N: a2dfa7e8c9c4d1616f1009c988bb70f
         Algorithm: SHA256-RSA
          Validity: 2017-11-22 00:00:00 +0000 UTC - 2020-11-22 12:00:00 +0000 UTC
            Issuer: CN=DigiCert SHA2 Assured ID CA,OU=www.digicert.com,O=DigiCert Inc,C=US
           Subject: CN=Octocat,O=GitHub\, Inc.,L=San Francisco,ST=California,C=US
            Emails: octocat@github.com
   ```

3. To set your X.509 signing key in Git, paste the text below, substituting in the certificate ID you copied earlier.

- To use your X.509 key to sign for all repositories:

  ```shell
  $ git config --global user.signingkey 0ff455a2708394633e4bb2f88002e3cd80cbd76f
  ```

- To use your X.509 key to sign for a single repository:

  ```shell
  $ cd /path/to/my/repository
  $ git config --local user.signingkey 0ff455a2708394633e4bb2f88002e3cd80cbd76f
  ```

### Configuration in SourceTree

1. Go to `Preferences` -> `Advanced` 
2. Set `GPG Program` Environment (usually in `/usr/local/bin`)
3. Vola 🎉

## Reference

- [git-scm](https://git-scm.com/book/tr/v2/Git-Tools-Signing-Your-Work)
- [github-docs](https://docs.github.com/en/github/authenticating-to-github/signing-commits)