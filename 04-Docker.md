# Part IV: Docker

## Objectives:
By the end of this module, the SRE will:
- Describe the directives from a sample dockerfile
- Explain layers and the docker cache
- Start a docker container that runs ubuntu linux
- Describe the following container concepts:
  - base image
  - single pid
  - `CMD` vs `ENTRYPOINT`
  - logs
  - volumes
  - layers and ordering
  - caching
  - build
  - push

## Deliverables
Complete the project described below. Answer the questions, then schedule a time to meet with your mentor and discuss the what you've built.  

### Docker
  1. Setup
  - https://www.youtube.com/watch?v=JprTjTViaEA
  - https://jonnylangefeld.github.io/learning/Docker/How%2Bto%2BDocker.html
  - Install docker
  - login to reactiveops quay and dockerhub via the terminal
    - You will need to be given access to these, reach out to your pod leader.
    - https://hub.docker.com/u/reactiveops/dashboard/
    - `docker login`
    - https://quay.io/organization/reactiveops
    - quay login information https://docs.quay.io/solution/getting-started.html

  2. Create new Dockerfile from `ubuntu:16.04` 
  
     - https://www.youtube.com/watch?v=6Er8MAvTWlI

  3. run  `apt-get update` and `apt-get upgrade` `--yes` on separate lines in the Dockerfile
  4. Build the docker file.  
      - `docker build -t intro_to_docker .`
      - How many steps were there?
  5. Build again
      - Why doesn’t it take as long?
        - The ubuntu image is already downloaded and can be referred to by its id (5e8b97a2a082)
        - Docker cache!
          - `---> Using cache`
          -  `---> 1dcd45e300d7`
        - Every layer checks the Docker cache
          - The docker cache are images that exist on the disk
          - http://kimh.github.io/blog/en/docker/gotchas-in-writing-dockerfile-en/
  6. Use `docker history <image>` to see each step and its layer id
      - Count the number of lines by using `|` `wc -l`
      - `docker history d57d088b85d1 | wc -l`
      - 9 lines!
  7. Replace the two lines with `apt-get update && apt-get upgrade` `--yes` and build again
      - How many steps were there?
      - What is `&&`?
      - Use `wc -l` here
      - Why were there fewer steps?

#### More with Docker
Start a docker container that runs ubuntu linux
  - `docker run -it <image_id> bash`

1.  Using the `ADD` and `RUN` directives, add one of the scripts from before to the image and build the image.
    - https://docs.docker.com/engine/reference/builder/
    - Does the first step get run again or is cache used?

2. Move the `ADD` directive so it is above the `RUN` directive and build the image
    - Is cached used this time?
    - Why not?
3. Use the `CMD` and `ENTRYPOINT` directives in turn the Dockerfile to make the script run by default
    - https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/
4. Rewrite the Dockerfile using `ADD`  and `CMD` to add the script and run the command.  

- Create a container using your latest image
`docker run -it <image_id> bash`
  - How is this different than `docker run -it <image> bash`?
  - Run the docker image as a daemon with `sleep 3600` as the main process
  `docker run -d <image_id> sleep 3600`
  - `exec` into the running container and run `ps -ef` to view the running processes
  - `docker exec -it <container_id> bash`
    - What is the `pid` of the `sleep` process?
    - What happens if you `kill` that pid

## Learning Resources:

- https://medium.com/@jessgreb01/digging-into-docker-layers-c22f948ed612
- https://blog.phusion.nl/2015/01/20/docker-and-the-pid-1-zombie-reaping-problem/
- https://hackernoon.com/my-process-became-pid-1-and-now-signals-behave-strangely-b05c52cc551c
- https://www.boynux.com/containers-fat-thin/
- https://rominirani.com/docker-tutorial-series-part-7-data-volumes-93073a1b5b72
- https://www.youtube.com/watch?v=VOK06Q4QqvE
