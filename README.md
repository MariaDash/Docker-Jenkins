# Jenkins & Docker
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

Now creating yml file:
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
6. 




