# How to use VSCode to develop in docker container 

## Why VSCode? 

VSCode is compared to some other editors such as `pycharm`, `clion`, `eclipse` much simpler to configure and use. 
It's also quite easy to install vscode. 

Add Microsoft keys to enable VSCode installation over `apt` package manager. 
```
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
```
Add VSCode repository as follows: 
```
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
```
Run installation command: 
```
sudo apt-get update && sudo apt install code
```

Run VSCode with: 
```
code
```

## Extensions 

Extensions I find useful for development in VSCode are: 
1. Docker 
2. C/C++
3. Python 
4. XML 
5. C/C++ Extension Pack 
6. CMake 
7. CMake Tools 
8. Doxygen Documentation 
9. Remote 

Extensions can be found in upper left corner of the editor as shown in Figure 1. 

![Figure 1.]( {{ BASE_PATH }} /assets/images/remote_dev/plugin_install.png)

On Figure 1. you can find following: 
1. Extensions button 
2. Search bar
3. Extension Name
4. Install/Disable button

**Extensions have to be installed in your container vscode in order to use them inside of a container.**

## How tu run VSCode in container? 

Run vscode locally with: 
```
code
```

When vscode opens, use extensions pack to install [remote containers](https://www.google.com/search?channel=fs&client=ubuntu&q=remote+containers+) plugin.

After installation of remote containers plugin, restart vscode. 

These are the steps required to run VSCode in container: 

1. Open VSCode: 
```
code
```

2. In another terminal, run your docker container: 
```
docker start -i <container_name> 
```

3. To attach VSCode to running container press `F1` and type the name of the container you want to join. 


Whole process is shown in Animation 1. 

![Animation 1.]( {{ BASE_PATH }} /assets/images/remote_dev/how_to_run_vscode.png)

## Configure VSCode in your container

After attaching container to a VSCode, new VSCode window will appear where you can open files. 
For ROS development, open workspace directory as main directory as it will probably be used the most. 

It's advisable to install extensions mentioned before to enable autocomplete.

In order to enable autocompletion for certain ROS packages add ROS package paths to 
`includePath` in `c_cpp properties.json` located in `.vscode` folder. 

```
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**", 
                "/opt/ros/galactic/include"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "gnu17",
            "cppStandard": "gnu++14",
            "intelliSenseMode": "linux-gcc-x64"
        }
    ],
    "version": 4
}
```

## Revision of development pipeline 

Current development pipeline is shown on Figure 2. 

![Figure 2.]( {{ BASE_PATH }} /assets/images/remote_dev/dev_pipeline.png)


## More info 

1. Tutorial for remote containers plugin can be found [here](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

