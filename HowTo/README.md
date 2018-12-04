# HowTo use a git repository to exchange data with a Docker container
## Prerequisites
* a [git](https://git-scm.com/book/de/v1) repository on a web accessible server
    * e.g. [github](https://github.com/): 
        * you need a [github account](https://github.com/join)
        * [fork](https://help.github.com/articles/fork-a-repo/) the github repository of interest e.g. https://github.com/rohanLuigi/JuliaWorkshopDec2018 
* a Docker container with git
## Create a Docker network bridge (optional)
In order to connect to network resources from inside the container we will use a [bridge](https://docs.docker.com/network/bridge/#manage-a-user-defined-bridge), which needs to be created once. 

```bash
docker network ls
# check if there is an existing bridge to use
#if not:
docker network create mybridge
```
## Start the Docker container
```bash
#replace mybridge with whatever name <docker network ls> above listed
docker run -itd -p 8888:8888 --network="mybridge" qtlrocks/jwas-docker
```

## Open a bash shell on the container
```bash
# look for the name of the started container
docker ps -a
#CONTAINER ID        IMAGE                  COMMAND                  CREATED              STATUS              PORTS                    NAMES
#72aff1fddc02        qtlrocks/jwas-docker   "tini -g -- start-no…"   About a minute ago   Up About a minute   0.0.0.0:8888->8888/tcp   youthful_lalande

#execute a bash on the container
docker exec -it youthful_lalande bash
```
## Inside the container (bash)
Using [git](https://git-scm.com/book/de/v1) we clone the repo:
```bash
git clone https://github.com/<YOUR_GITHUB_USER_NAME>/JuliaWorkshopDec2018.git
cd JuliaWorkshopDec2018

#config email and user.name e.g. for me
git config --global user.email daniel.lang@helmholtz-muenchen.de
git config --global user.name "Daniel Lang"

#now the repo is ready to modify
```
## Before you do anything in the repo (when you did not work on it for some time)
```bash
git pull
```

## Commit and push lokal changes back to repository
```bash
#check for changes
git status

#if there are untracked == new files add them
git add <FILENAME>

#commit the changes to the local repo
git commit -a -m '<YOUR COMMENT ON WHAT YOU COMMIT>'

#push local changes to reomte repo
git push
```
