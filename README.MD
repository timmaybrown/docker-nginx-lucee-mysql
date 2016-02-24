# Lucee Docker Seed
Super basic instructions for getting setup and running the image. Will expand later to have a `db` service that installs and mounts the `MySQL` etc..

#### Create the Local Docker VM
```
docker-machine create -d virtualbox dev
```
#### Make sure the machine is active
```
docker-machine env dev
```
Take note of the IP address of the VM since this how you will hit the containers running on that VM. Since this is the first app running on this `docker-machine` our `docker-compose.yml` file just exposed port `80:80` of the container to the `machine` port of `80` If you planned to run multiple web apps on this one docker VM then you could adjust your `web` service to expose on a different port. ie: `8090:80` then you could hit your app at `http://your-vm-ip-here:8090/`

#### Set the proper environment variables
Make sure you run this command to set the docker `ENV` vars for this terminal so it knows how to communicate with the docker client. This is slightly different on a Mac, but the command above will output what you should run. Windows it is the command below minus the `REM` command from the above output
```
FOR /f "tokens=*" %i IN ('docker-machine env dev --shell=cmd') DO %i
```
#### Change directories to where you cloned this repo
Then run the following command to create the `services` in this case we just have 1 service called `web` that will run the `lucee4-nginx` image. Simply swap the base image in the `docker-compose.yml` to change this out for `lucee5-nginx` or your own customized image. The following command will create and start the commands in a `detached` mode so your terminal is still available
```
docker-compose up -d
```
#### Confirm the container(s) are running
You can take note of the `CONTAINER ID` or the `NAMES` as these can be used in commands to interact with the containers.
```
docker-ps
```

#### Now Hit the App
When you created the `docker-machine` it gave that VM an IP address. Since this is the `default_server` for NGINX it will respond to any host name. You can just make a host file entry of `myapp.local  192.168.99.100` or whatever your VM IP address is given. Using the IP or the host name should now work.
```
http://myapp.local/
http://192.168.99.100/
```
Now just simply change the code in the `./services/web/code/` folder, `save, refresh and repeat` . Since the files are mounted from your local computer to the VM, when the files change locally the VM file system stays in-sync since it's just referencing the local `VOLUME`.

#### TURNING IT OFF
Since the the application code and eventually the mysql data will be stored on your computer outside the container and just mounted in via the docker-compose.yml definition you can simply just run the following command to stop all `containers` that were created by the `docker-compose up -d` command. Just run:
```
docker-compose stop
```
If you want to make changes to the `docker-compose.yml` file you can remove all containers that were created completely by running 
```
docker-compose rm
```
The above command will clean up all containers that were created by it's `up` command. Make your changes to the `docker-compose.yml` file and then re-run the above `up` command and it'll recreate the containers.

## TODO
- Add `MySQL` service to the `docker-compose.yml`
- Figure out where the db file lives so MySQL container can just run MySQL but the `data` is persisted to your local disk.
- Figure out how to make this all work in production with something like `Tutum` or `Digital Ocean`
 
#### Cheers!
