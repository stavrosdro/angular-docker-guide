# Docker Development Environment: Angular - Full Guide

Let's assume that you just formatted your windows laptop and you enjoy your fresh installation just before the download of the Nodejs LTS version. Another interesting journey is to install Docker, VSCode, enable WSL in your local machine. Of course the fact that we use Windows instead of an Unix alternative will "cost" a couple of more steps but in the end the look and feel will be perfect and the laptop cleaner than ever.

The steps are the following:

- [x] Fresh Windows installation
- [x] Coffee
- [ ] Download & Ιnstall Docker (enable WSL 2), Git, WSL, VSCode
- [ ] Configure WSL, Docker, Git, VSCode
- [ ] FINAL STEPS & BONUS

*Let's get started!*

## Download & Installation Tips

### Docker
- During the Docker installation you should check the *Install required Windows components for WSL 2*
- Make sure that in *Docker > Settings > General* the option **Use the WSL 2 based engine** is checked

### Git
- Install Git for windows and set up the needed credentials in order to clone/pull/push from/to your repo. Use **Personal Access Token** and store it in the **Windows Credentials**

### VSCode
- Add VSCode to Windows path
- Open VSCode install the ***Remote - WSL extension (by Microsoft)**

### WSL
- Run CMD or PowerShell as admin and type `wsl -l -o`. You will see the available distros. 
- Install one e.g. Ubuntu-20.04, by running `wsl --install -d Ubuntu-20.04`.

## Configurations

### WSL

- Run `wsl -l -v` and you will notice that Ubuntu distro's version is 1 and the default subsystem is the docker-desktop.
- Upgrade the distro in v2 with `wsl --set-version Ubuntu-20.04 2`
- Set v2 as default for furure installations `wsl --set-default-version 2`
- Set the detault distro for Docker `wsl --set-default Ubuntu-20.04`

### Docker

- Navigate to *Docker > Settings > Resources > WSL INTEGRATION* and enable your new distro by clicking the toggle switch and click *Apply & Restart*
- Create a Docker Image with Angular by running `docker build -t angular12 .` This is my Dockerfile:
```
FROM node:14-alpine

RUN npm install -g @angular/cli

WORKDIR /app

EXPOSE 4200 49153

CMD npm start
```
- Open cmd and run `wsl`. You can create a folder for your projects in the home directory
- Now create a Docker Container with `docker run -it -v $(pwd):/app -p 4200:4200 -p 49153:49153 --name ng angular12 sh` (inside the linux distro in the projects' folder)
- Run `ng new my-app --skip-git` and `chmod -R 777 ./*`
- Open a new cmd, type `wsl`, navigate to your new project's folder and run `code .` (If you don't have VSCode in your path follow the steps from the Remote - WSL extension in order to open it in the above location)

## FINAL STEPS & BONUS

- Open **package.json** file and replace ~~"start": "ng serve",~~ with **"start": "ng serve --host 0.0.0.0 --poll",**
- From inside the Container run `npm start` and navigate to `http://localhost:4200`
- Configure the git in WSL by running:
```
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager-core.exe"
```
- Run `git init`, **commit** the changes in your project and **push** to the remote

### Bonus
- When WSL is running you can navigate using the windows explorer in the location `\\WSL$` 
- You need to run `chmod -R 777 ./` after you generate files with docker, otherwise you will not be able to edit files with VSCode. I will update the guide if I find a workaround for this.
- Find `[*]` and add a new rule below it: `end_of_line = crlf`. Also include a new category `[*.sh]` and put `end_of_line = lf` below it

*Enjoy!!!*