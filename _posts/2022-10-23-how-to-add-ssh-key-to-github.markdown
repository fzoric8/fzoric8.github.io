## How to add SSH key to github account? 

Since August 13th 2021 Github has removed support for using account password for authenticating 
Git operations. You can read more about that [here](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/). 

#### To authenticate git operations you can now use: 
1. [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) 
2. [ssh key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 

_Easiest and simplest way to authenticate git operations is using SSH key._

#### In order to generate SSH key, run following command: 
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

After that, press <Enter> three times in a row: 
1. To keep default file for saving ssh key 
2. Passphrase 
3. Repeat passphrase

After SSH key generation command, output will be: 
```
Your public key has been saved in /home/user/.ssh/id_rsa.pub

```

#### Next step is to copy public SSH key to your github account. 

You can get contents of the `id_rsa.pub` file as follows: 
```
cat /home/user/.ssh/id_rsa.pub
```

Bear in mind that path to `id_rsa.pub` may differ and you have to change 
it accordingly. 

Output of the `cat` command is something like: 
```
ssh-rsa lot_of_random_letters_and_numbers...==<my_email>@gmail.com
```

#### Copy contents of `id_rsa.pub` and do following: 
1. Log-in to your github account 
2. Press white downwards pointing arrow in the upper-right corner beside your profile picture to open drop-down menu
3. Press **Settings** in drop-down menu
4. After opening **Settings** on the left side of the page click on `SSH and GPG keys`
5. After opening `SSH and GPG keys` click on the green button that says **New SSH key**
6. Type in title for your SSH key (usually PC or container name) 
7. Paste contents of `id_rsa.pub` (SSH key) bellow tititle in **Key** window
8. Press Add SSH key 

#### Verify that you've added ssh key as follows: 
```
ssh -T git@github.com
```

If you've added SSH key you will get output like: 
```
> Hi USERNAME! You've successfully authenticated, but GitHub does not
> provide shell access.
```

More about SSH you can read [here](https://en.wikipedia.org/wiki/Secure_Shell). 



