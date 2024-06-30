# Git Credential Manager And Automatic SSH Signing
![](./gcm.png)
## Git Credential Manager
Git Credential Manager (GCM) is another way to store your credentials securely and connect to GitHub over HTTPS. With GCM, you don't have to manually create and store a personal access token, as GCM manages authentication on your behalf, including 2FA (two-factor authentication).

## Installation
### MacOS

**Homebrew**
    If you have an existing installation of the 'Java GCM' on macOS and you have installed this using Homebrew, this installation will be unlinked (```brew unlink git-credential-manager```) when GCM is installed.

#### Install
```sh
brew install --cask git-credential-manager
```

After installing you can stay up-to-date with new releases by running
```sh
brew upgrade --cask git-credential-manager
```

Or you can directly install it from its installation package found [here](https://github.com/git-ecosystem/git-credential-manager/releases/tag/v2.5.0)

#### Uninstall
To uninstall, run the following command
```sh
brew uninstall --cask git-credential-manager
```

To uninstall installation package use
```sh
sudo /usr/local/share/gcm-core/uninstall.sh
```

### Linux
Download the latest [.deb package*](https://github.com/git-ecosystem/git-credential-manager/releases/tag/v2.5.0), and run the following command
```sh
sudo dpkg -i <path-to-package>
```
```sh
git-credential-manager configure
```
The default credential stores on macOS and Windows are the macOS Keychain and the Windows Credential Manager, respectively.
GCM comes without a default store on Linux distributions.
Some extra configurations are required for all linux distributions.<br>
Here is a list of all credential stores from which you can pick one to use.

- Windows Credential Manager
- DPAPI protected files
- macOS Keychain
- freedesktop.org Secret Service API
- GPG/pass compatible files
- Git's built-in credential cache
- Plaintext files
  
You can select which credential store to use by setting the ```GCM_CREDENTIAL_STORE``` environment variable, or the ```credential.credentialStore``` Git configuration setting. For example
```sh
git config --global credential.credentialStore gpg
```
### Uninstall
```sh
git-credential-manager unconfigure
```
```
sudo dpkg -r gcm
```
## Windows
GCM is included with Git for Windows. During installation you will be asked to select a credential helper, with GCM listed as the default.
Or you can still download the standalone for windows [here](https://github.com/git-ecosystem/git-credential-manager/releases/tag/v2.5.0)

## .Net tools
GCM is available to install as a cross-platform .NET tool. This is the preferred install method for Linux because you can use it to install on any .NET-supported distribution. You can also use this method on macOS if you so choose.

**Note**: Make sure you have installed **version 7.0** of the .NET SDK before attempting to run the following dotnet tool commands. After installing, you will also need to follow the output instructions to add the tools directory to your PATH.

## Usage
## Automatic SSH Signing
To have git automatically sign stuffs such as commits, here are the steps you will have to follow
- Create and SSH key pair
- Add it to your github account or any other hosted git account.
- Configure Git to begin using the key for verifying signatures

* But before that you should git install, which not in the scope of this article you may check it [here](https://github.com/git-guides/install-git)   
#### Step 1: Creating and SSH key pair
Run ```ssh-keygen -t``` followed by the key type and an optional comment. This comment is included in the .pub file that’s created. You may want to use an email address for the comment. 
For example, for ED25519: 
```ssh-keygen -t ed25519 -C "<comment>"```
For 2048-bit RSA: 
```ssh-keygen -t rsa -b 2048 -C "<comment>"```

Then press ```Enter``` to the following prompts
![](./Pasted%20image.png)
you can also associate a password to your keys or just press Enter for no password
![](./Pasted%20image%201.png)

#### Step 2: Adding it to your github account
![](./add_key_github.png)
![](./adding_key_2.png)
![](./adding_key_3.png)

Now that we have added our key to github we can now configure git on our local machine for automatic signing

#### Step 3: Configure git to use the SSH keys for automatic signing
To configure Git to use your key.
```sh
git config --global gpg.format ssh
```
Specify which public SSH key to use as the signing key and change the filename (```~/.ssh/examplekey.pub```) to the location of your key. The filename might differ, depending on how you generated your key: 
```sh
git config --global user.signingkey ~/.ssh/examplekey.pub
```
#### Example try signing a commit if it works
1. Use the -S flag when signing your commits: 
```sh
    git commit -S -m "My commit msg"   
```
2. Optional. If you don’t want to type the -S flag every time you commit, tell Git to sign your commits automatically.
```sh
   git config --global commit.gpgsign true
```
3. If your SSH key is protected, Git prompts you to enter your passphrase. 
4. Push to gitlab or any hosted platform where you added your keys. 
5. Check that your commits are verified. Signature verification uses the allowed_signers file to associate emails and SSH keys. 
   
And your done!, Congrats you just signed your first commit