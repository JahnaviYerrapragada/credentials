# Encrypted secrets for HSI.

## Install git-crypt

Installation instructions can be [found here](https://github.com/AGWA/git-crypt/wiki/Installation).

	# Eg on Mac
	> brew install git-crypt

## Install gpg

	# Eg on Mac
	> brew install gpg

Create [your own gpg key](https://help.github.com/articles/generating-a-new-gpg-key/)

	> gpg --full-generate-key    

## Add team member as a trusted user

You need to [exchange and verify public key](https://www.gnupg.org/gph/en/manual/x56.html) and 
then [add it to the vault](https://github.com/AGWA/git-crypt)

	# Export a public key, e.g:
	> gpg --armor --export john.doe@klarna.com > john.doe.gpg

	# Send public key to team member that already have access to this repository

	# Import the public key of new trusted team member on your personal keyring, e.g:
	> gpg --import john.doe.gpg
	gpg: key 0AB12345: public key "John Doe <john.doe@klarna.com>" imported
	gpg: Total number processed: 1
	gpg:               imported: 1  (RSA: 1)

	# Sign and verify, e.g:
	> gpg --sign-key 0AB12345

	# Add user to git crypt using the key id, e.g:
	> git-crypt add-gpg-user 0AB12345

## Usage guide

	# Unlock the repository with your GPG password
	> git-crypt unlock

	# Create and edit files

See [git-crypt](https://github.com/AGWA/git-crypt) for more information on how to use git-crypt.

## Secret folder structure

```
├── jenkins/
      ├── global/
├── misc/
```

## Jenkins credentials convention

Jenkins credentials should be put in `jenkins/<domain>`. 

Each credential should be in a different `yml` file. File names should match 
credential ids.

Example: `secrets/jenkins/global/stash-notifier.yml` will add a new credential 
with id `stash-notifier` in the `global` domain in Jenkins. 

There are 4 types of credentials accepted:

- Basic SSH User Private Key

```
privateKey: |
  -----BEGIN RSA PRIVATE KEY-----
  ...
  -----END RSA PRIVATE KEY-----
description: Credentials description
passphrase: ''
type: BasicSSHUserPrivateKey
username: cloud-user
```

- String Credentials

```
description: Credentials description
secret: secret text
type: StringCredentialsImpl
```

- File Credentials

```
filename: nexus
data: |
  file
  contents
  here
description: Credentials description
type: FileCredentialsImpl
```

- Username/Password credentials

```
password: password-here
description: Credentials description
type: UsernamePasswordCredentialsImpl
username: sys.cloudy
```


