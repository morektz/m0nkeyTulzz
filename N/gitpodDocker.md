# Gitpod Docker Information 

When you need to constantly install stuff for your repo and it takes really long, then there is an option to make a custom docker image, that gets stored when you make a project. 

See these links 

https://www.gitpod.io/docs/config-docker 
- This talks about making a docker file 

Steps 
1. Make .gitpod.Dockerfile 
2. At the top of [.gitpod.yml](https://www.gitpod.io/docs/config-start-tasks) - add
```yaml
image:
  file: .gitpod.Dockerfile
```
so that example .gitpod.yml file from my other repo

```yaml
# List the start up tasks. Learn more https://www.gitpod.io/docs/config-start-tasks/
image:
  file: .gitpod.Dockerfile

tasks :
  
  - name: Echo Test
    command: echo "hello"

# List the ports to expose. Learn more https://www.gitpod.io/docs/config-ports/
ports:
  - port: 8000-9000
    onOpen: open-preview
    visibility: public

```

Next step is to make the `.gitpod.Dockerfile`

It should look like this 

```yaml
FROM gitpod/workspace-full

RUN brew install plantuml
RUN npm update -g
RUN npm install --global http-server 

```
This isa a standard docker configuration file , use `run` for every line. The actual doc suggests another format, but this is way easier.

If you made this docker file and then also include the npm... statemetns in the yaml, file for some reason the npm packages are not installed. This is why I recommend you put all the package installs in this docker file, and not in the yaml file.
>Ideally avoid having having both in the same repo, and only use this method when something takes way too long to install. This is the same reasoning I had done after making mistakes


> Making both `.gitpod.yml` & `.gitpod.Dockerfile` , is problematic and buggy. I burned way too much time on this 


After making the files, do the tests. The test commands are 

``` bash
docker build -f .gitpod.Dockerfile -t gitpod-dockerfile-test . 
``` 
This builds the actual docker image in your instance. Very important step, if you dont do this , then the prebuild will take an hour each time you run the test.

```bash
docker run -it gitpod-dockerfile-test bash
```
This builds a gitpod-dockerfile-test image and starts a new container based on that image. At this point, you are connected to the Docker container that will be available as the foundation for your Gitpod workspace. You can inspect the container and make sure the necessary tools & libraries are installed.

To exit the container and return back to your Gitpod workspace, type `exit`.

> Once you have the prebuilds done, dont deleted the project, instead revert back to a snapshot and use that, when u use a snapshot its workspace gets created, and you can work on that. I made several mistakes trying to delete all the projects and snapshots, which again burned a lot of time. 