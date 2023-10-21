# Docker & Jenkins 
## Docker
Install Docker, check version:
```
Admin@DESKTOP-V6V9F0T MINGW64 /
$docker --version
```
Creating project folder:
```
Admin@DESKTOP-V6V9F0T MINGW64 /
$ mkdir project1
```
Creating folder api:
```
Admin@DESKTOP-V6V9F0T MINGW64 /
$cd project 1
Admin@DESKTOP-V6V9F0T MINGW64 /project1/
$mkdir api
Admin@DESKTOP-V6V9F0T MINGW64 /
$ cd ..
```
Coping files from another project:
```
Admin@DESKTOP-V6V9F0T MINGW64 /$ cp project2/api/requirements.txt project1/api/requirements.txt
Admin@DESKTOP-V6V9F0T MINGW64 /$ cp project2/api/app.py project1/api/app.py
Admin@DESKTOP-V6V9F0T MINGW64 /$ cd project1
Admin@DESKTOP-V6V9F0T MINGW64 /$ ls -la api
```
We checked that files were copied.

Now creating yml file (better in PyCharm there will be no mistakes):
```
Admin@DESKTOP-V6V9F0T MINGW64 /$ vim docker-compose.yml
```
docker-compose.yml
```
version: "3"
services:
api:
  build: ./api
```
creating docker file:
```
Admin@DESKTOP-V6V9F0T MINGW64 /$ cd api
Admin@DESKTOP-V6V9F0T MINGW64 /$ vim Dockerfile
```
Dockerfile
```
FROM python3.9-slim
WORKDIR /python_flask_d
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=5001"]
```
What we did here:
1. We install python 3.9 slim from docker storage
2. Creating working directory for the server to run in
3. In this directory copy requirements
4. Install requirements
5. Copying all work files from local machine to the virtual machine

Starting the project (image) - building it
```
Admin@DESKTOP-V6V9F0T MINGW64 /$docker-compose up --build
[+] Building FINISHED
Admin@DESKTOP-V6V9F0T MINGW64 /$
```
## Jenkins
1. We go to gitHub and create file new_1.py (on Python)
2. We open PowerShell as Administrator and run the command
```
PS C:\WINDOWS\system32> docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```
3. To see process ID:
```
PS C:\WINDOWS\system32> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS C:\WINDOWS\system32>
```
4. Go to browser and input ip:port: 127.0.0.1:8080

Started first jenkins page where you need to input administrator password 

5. To find it we need to run the command in PowerShell and use Container ID that is running:
```
PS C:\WINDOWS\system32>docker logs <containerid>
```
6. Push "Continue" button and then install suggested pluggins.
Waiting for Jenkins to install all plugins
7. We need to fill in fields: name, password, full name, email then push "Save and Continue" button
8. Jenkins setup is complited - start using Jenkins.
9. Fill in Item's name ( name of the project) - Project_1
10. Push "Pipeline" button then OK
11. In the bottom of the page Pipeline Script choose  type "Hello World"
12. Change Stage name to `("checkout")`
13. Change syntax with Pipeline Sintax
14. We need to add checking git access: Choose option `Checkout from version control`
15. Then choose `GIT`, `Repository URL` ( by this we are cloning GIT Repository to Jenkins)
16. Change name of the branch from `master` to `main`
17. Generate Pipeline Script
18. Copy it and insert instead of `echo "Hello World"` and save
19. Push " Build Now"
`build now`
To see logs hover to number of seconds under `build`
### To Build and run project
1. We go to settings and open pipeline script/ We need to add script to run in Shell/ So we wind it in Pipeline Syntax: `Shell Script`
2. in `Shell script` we insert `python3 new_1.py` and generate script and then copy it
3. Also before it we need to add script to clone repository GIT:
   3.1. We again use Pipeline syntax and choose option `GIT`
   3.2. We insert URL , change name `master` to `main`, generate script and copy it
4. Insert it before sh script in the pipeline
5. Apply and save and then `Build now`
6. We have first issue: error python3 not found- so we need to install python3 in the container
7. We go to PowerShell and find container ID
```
PS C:\WINDOWS\system32> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS C:\WINDOWS\system32>
```
Then we install python ( this command enable to go to Bash terminal of the container and install there what we need:
```
PS C:\WINDOWS\system32>docker exec -it -u 0 containerID /bin/bash
root@containerID:/#
```
exec - execute
it - interactive
u - user
8. Updating package manager:
```
root@containerID:/#apt-get update
root@containerID:/#
```
9. Installing Python:
```
root@containerID:/#apt-get install python3
root@containerID:/#
```
10. Trying to `build no` in jenkins again - everything working
### By now we configurated Jenkins, created a project, add GitHub ( from GitHub project is building in Jenkins) and configure correct build.
## Testing with Postman
1. Go to PowerShell and install node js and then check it  
```
root@containerID:/#npm install nodejs
root@containerID:/#node -v
v12.22.12
```
2. Installing Newman
```
root@containerID:/#npm install -g newman
root@containerID:/#
v12.22.12
```
If you have error `npm not installed` do command `apt  install -y npm`

!!! We will use Postman collection to run it. So we add the collection to our GitHub
3. We create in Jenkins another project for Postman tests:
3.1. Item:pm_api_tests 
3.2. Creating task with free configuration --> OK
3.3. Managing with source code --> choose GIT and input URL and change `master` to `main`--> Apply --> Save
4. Settings. Configure. General Settings
4.1. Build trigger --> run after other projects builds
4.2. Insert another project: Project_1
4.3. Run if build is stable
4.4. Build steps ( run Shell command):
```
newman run Postman 1.postman_collection.json
```
Apply --> Save --> Build now

Tests are running - to see the results: build --> output to the console



