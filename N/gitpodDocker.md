# Gitpod Docker Information 

When you need to constantly install stuff for your repo and it takes really long, then there is an option to make a custom docker image, that gets stored when you make a project. 

See these links 

https://www.gitpod.io/docs/config-docker 
- This talks about making a docker file 

Steps 
1. Make .gitpod.Dockerfile 
2. At the top of [].gitpod.yml](https://www.gitpod.io/docs/config-start-tasks) - add
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
Whats important is the image:line