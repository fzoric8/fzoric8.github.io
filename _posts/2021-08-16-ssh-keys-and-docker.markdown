# Use SSH whith docker 

From recently, github has depreacated pulling, cloning repositores 
with username and password. Therefore it's neccessary to 
configure personal token or SSH keys in order to easily clone repositories, and
use all available features of docker files. 

Easiest way to solve this is to use SSH keys for cloning, 
pulling and pushing repositories. 

In order to do so, you have to generate SSH key with service 
you use for software versioning. 

Here you can find instructions how to do so for most common SV 
providers: 

1. [Github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)  

2. [BitBucket](https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/)  

3. [GitLab](https://docs.gitlab.com/ee/ssh/)  

After you've created SSH keys, you can pass it in wide 
variety of ways. BuildKit is available from Docker Engine 
since 18.06. In order to use it, you have to define 
build environment variable as follows in your `~/.bashrc` or `~/.zshrc` file: 
```
export DOCKER_BUILDKIT=1
```
After adding that line. you need to source it as follows: 
```
source ~/.bashrc
```

When you've set up your SSH keys and defined BuildKit as 
build engine, you need to make several adjustments to existing 
Dockerfile to support using SSH keys. 

First, define syntax of your Dockerfile in first line as: 
```
# syntax=docker/dockerfile:1.3
```

After that, to all commands that clone git repositories, 
add following in root user before cloning any of the repositories. 
```
RUN mkdir -p -m 0600 ~./ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
```
Command above copies your ssh keys from github to hidden folder inside 
of your docker image, and enables access to those during build. 

```
RUN --mount=type=ssh <git clone command> 
```

Which looks like: 
```
RUN --mount=type=ssh git clone git@github.com:fzoric8/ros_utils.git 
```

In the end, run docker build command as follows: 
```
 docker build --ssh default=$SSH_AUTH_SOCK -t moveit_noetic_img .
```

You can find out more about Buildkit on following [link](https://docs.docker.com/develop/develop-images/build_enhancements/) 

## More about ssh keys

If you want to test your current ssh configuration, run following command: 
```
ssh -vT git@github.com
```

If everything's setup okay, output should be following: 
```
Hi fzoric8! You've successfully authenticated, but GitHub does not provide shell access.
```

If output is : `git@github.com: Permission denied (publickey).` 

Something doesn't work as it should. 

You can check these awesome resources from GitHub:  
1. [About SSH](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/about-ssh)  
2. [Check for existing SSH keys](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)  
3. [Generate new SSH keys](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 
4. [Add new SSH key to your GitHub account](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)  
5. [Test your SSH connection](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/testing-your-ssh-connection)  


# More resources: 
1. [How does ssh work](https://www.hostinger.com/tutorials/ssh-tutorial-how-does-ssh-work) 
2. [SSH protocol](https://www.ssh.com/academy/ssh/protocol]
