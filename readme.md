# Tutorials of carya on university of Houston

## Carya

### Step 1: Request for an account and fill time from UH HPC center
See the details with the following link
https://uh.edu/rcdc/getting-started/

### Step 2: Basical operations
There two positions for your operation,Your local dictionary and the PI's dictionary.Typically, put your code into your local dict and the Data,virtual files, ex. conda envs, into your PI's dict.

Your local dictionray (Disk space:only 10G):
```Bash
cd ~
```
Your Pi's dictionray (By your request, 4T):
```Bash
cd /project/<PI name>/<your user name>/
```
If there is no folder in with your user name, make it by yourself.
```Bash
mkdir /project/<PI name>/<your user name>
```
Here are some common commands, skip it if your are familiar with it.

To remove a file or a foler recursively:
```Bash
rm <file path>
```
```Bash
rm -r <fold path>
```
other commands: move, compress and extract files:
```Bash
mv <file path> <destination path>
```
```Bash
tar -cvf <file name.tar.gz> <fold path>
```
```Bash
tar -xvf <file name.tar.gz> 
```
To combine the csv files with same head,Go to the directory containing the CSV files
```Bash
cd /path/to/csv/files
```
Concatenate the files, keeping the header from only the first file
```Bash
head -1 one_of_your_files.csv > merged.csv
tail -n +2 -q *.csv >> merged.csv
```
To kill some of the ps, check the ps list first and then kill with the pid:

```Bash
lsof +D /path 
```
```Bash
kill -9 or -15 pid 
```

### some git operations:
```Bash
git clone -b main --single-branch https://<token>@github.com/<repository>
```
After edit the files, 
```Bash
git commit -m "<your commit description>"
```
It will tell you which files need to be add
```Bash
git add <files>
```
commit again
```Bash
git commit -m "<your commit description>"
```
```Bash
git push
```
To stash the local changes then pull:
```Bash
git stash
```
```Bash
git pull
```
and delete the stash if wanted or keep the stash:
```Bash
git stash drop
```
```Bash
git stash pop
```
***
### Step 3: Please follow the operations with the official operation from UH HPC center
https://uh.edu/rcdc/support-services/user-guide/getting-started-clusters/

Quick command for activate the conda env:
```Bash
module add Miniconda3/py310
source  $(dirname `which python`)/../etc/profile.d/conda.sh
conda activate /project/chen/envs/ocp-models
```
Here are some operations that usefull and not in this tutorial.

To show the size of the dictionary
```Bash
du -h <Path> 
```
To show the file counts of the dictionary
```Bash
find <Path> -type f | wc -l 
```
To kill the squeue:
```Bash
scancel <pid> 
```
to catch up a task
```Bash
cat myjob.o<123456> 
```

### Jupyter notebook set up

It's not convinient to build the code with the linux system without GUI, jupyter notebook is recomanded.

Here are the method to set up:
* Set up conda env and activate it, following the UH tutorial
* Install jupyter
```Bash
conda install notebook
```
* Set up server
```Bash 
jupyter notebook --no-browser --ip=127.0.0.1 --port=88<xx> 
```
you will see the address like this, but can't open it

http://127.0.0.1:8890/?token=gcf45711b1540d5004eff093e7fe8a511d339b28d6171012

* Open a new teminal on your local machine, type
```Bash 
ssh -l <username> carya.rcdc.uh.edu -L 88<xx>:127.0.0.1:88<xx>
```
* The address above can be a 4-digit number like 8888, replace and change the number by yourself if the port cannot be listened.
* Afterwhile you can login the address and start to use the jupyter on your local machine.
* To check if the port entry is occupied on your local machine, type
```Bash 
lsof -i :88<xx>
```  
### Tensorboard remote set up
1. on local:
   `ssh -L 16006:127.0.0.1:6006 user@server`
2. on server:
   `tensorboard --logdir=<profile-logs> --port=6006 --bind_all`
3. on local browser:
   login `127.0.0.1:16006` or `localhost:16006`

## Set up remote vscode in compute node rather than login node
ref: https://github.com/microsoft/vscode-remote-release/issues/1722#issuecomment-1483162486
ref: https://code.visualstudio.com/docs/remote/tunnels
1. Install Code CLI `curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' --output vscode_cli.tar.gz` then `tar -xf vscode_cli.tar.gz`
2. Qequest a node` salloc -t 1:00:00 -n 24 --gpus=1 -N 1`
3. Create a tunnel by `./code tunnel `, select the github and use the code output to connect the account on https://github.com/login/device
4. Open VSCode on local machine, ctrl+shift+p and type "tunnel" to find the "Remote-Tunnels: Connect to Tunnel..." command. The tunnel I created shows up in the list, click it.

# `Do not` need to see the following:
***
***
***
If you do not want to change the contents of the dockerfile, you could use such command to build the image:

```Bash
docker build -t xubuntu:1.8 https://github.com/moxx799/Docker-file.git
```


* Start from `pytorch 1.14.0a` image:


There are 3 available options:

| Option  | Description | Default |
| :-----: | ----------- | ------- |
| `BASE_IMAGE` | The base image for building this desktop image. | `nvcr.io/nvidia/pytorch:22.12-py3` |
| `BASE_LAUNCH` | The entrypoint script from the base image. If there is no entry script, please use`""`. | `/opt/nvidia/nvidia_entrypoint.sh` |
| `WITH_CHINESE` | If set, the image would be built with Chinese support for vscode, sublime and codeblocks. | `true` |
| `WITH_EXTRA_APPS` | The installed extra applications. Each character represents an app or several apps. For example,`cgo` represents fully installing `Cloudreve`, `GIMP`, `LibreOffice` and `Thunderbird`. More details could be referred in the following table. | `cgo` |
| `ADDR_PROXY` | Set the proxy address pointing to `localhost`. If specified, this value should be a full address. (Experimental feature ::) | `unset` |

Here we show the list of extra apps:

|  Code   | Description |
| :-----: | ----------- |
| `c` | [`Cloudreve`][link-cloudreve] |
| `p` | [`PyCharm`][link-pycharm] |
| `g` | [`GIMP`](https://www.gimp.org/downloads) |
| `k` | [`GitKraken`][link-gitkraken] |
| `m` | [`Sublime Text 4`][link-sublime-text] |
| `x` | [`TeXLive`](https://tug.org/texlive) + [`TeXstudio`](https://www.texstudio.org) |
| `n` | [`Nautilus`](https://github.com/GNOME/nautilus) + [`Nemo`](https://github.com/linuxmint/nemo) |
| `o` | [`LibreOffice`](https://www.libreoffice.org) + [`Thunderbird`](https://www.thunderbird.net) |
| `e` | [`GNU Emacs`](https://www.gnu.org/software/emacs/download.html#gnu-linux) |

To find your launch script of your base image, use

```bash
docker inspect <your-base-image>:<tag>
```




* Switch the VNCServer to `XTigerVNC` (experimental): Add the option `--xvnc` will make the desktop hosted by the `Xvnc` program. Everything will be run in the same process. There will be no sub-process manager like `tigervncserver` to manage desktop related programs. A good thing is that, users do not need to run `tigervncserver -kill :1` before saving the image. However, currently these desktop related programs are not guaranteed to be closed if hitting <kbd>Ctrl</kbd>+<kbd>C</kbd>. Therefore, we suggest the users to use `ps -aux` to validate the running processes before saving the image.



  After using <kbd>Ctrl</kbd>+<kbd>C</kbd> to kill the `Xvnc` program, users can use the following command to relaunch the `Xvnc` and `noVNC` services:

  ```bash
  xvnc-launch [--root]
  ```


* With [`Cloudreve` :link:][link-cloudreve]: We recommend users to launch `Cloudreve` by opening a new terminal on the desktop, and using the following command:

  ```bash
  crpasswd  # only used for checking the INITIAL admin password.
  cloudreve  # launch Cloudreve service, requires users to expose 5212 port.
  ```

  > :warning: Using `Cloudreve` requires users to add the extra app `c` in the option `WITH_EXTRA_APPS` when building the image.

  > :warning: We **STRONGLY** recommend users to change their admin password, and create a non-admin user for using `Cloudreve`. You can also configure your data exchanging folder.

  

## Features

This is the minimal desktop test based on `ubuntu` `16.04`, `18.04` or `20.04` image, it has:


* [**Cloudreve Service (Chinese only)** :link:][link-cloudreve]: a private cloud storage service, allowing users to expose their personal folder as an "online drive" available on LAN. If users are interested, they can dig into the configurations and enable more features (like WebDAV and offline downloading). Currently this feature is designed for using a browser-based app to replace the WinSCP client.


## Update records

### ver 1.0 @ 08/18/2023

#### :Initial build

1. Initial build.



[link-pycharm]:https://www.jetbrains.com/help/pycharm/installation-guide.html
[link-gitkraken]:https://www.gitkraken.com
[link-sublime-text]:https://www.sublimetext.com/
[link-cloudreve]:https://cloudreve.org/
[link-filebrowser]:https://filebrowser.org/
