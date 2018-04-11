# infrastructure_submodules
Components of the SDAL infrastructure (in submodule form)

# Docker Containers

## R

### rpkgs

#### Installing package for all users simultaneously

This allows a package to be accessable to all users without separate compilling and installations.

1. Go into the rpkgs container: `docker exec -it rpkgs bash`
2. Launch R: `R`
3. Install the package you want for everyone, e.g., `install.packages('tidyverse')`

#### Installing package that needs a system library for all users

Some R packages need a system library installed, e.g, `geojsonio` needs the `libjq` system library installed.
In this example, CENTOS needs the `jq-devel` library installed.

##### Install it in container directly

1. Go into the user's container, e.g., `docker exec -it rstudio_chend bash`
2. Install the system library, e.g., `yum install jq-devel`
3. Install the package of interest in R, e.g., `install.packages('geojsonio')`

##### Installing the system library and R package for everyone

If the above works, you can install the package in the image itself

###### Installing the system library for all docker images

1. Do this on `snowmane`, the test server
    1. Add the installation in the `mro` `Dockerfile`, e.g., `yum install -y jq-devel && \`
    2. Build the base `mro`, `rpkgs`, `rstudio`, and `shiny` images (you can use the `dbuild` command from the dockerimages setup)
    3. Push the images to dockerhub, `docker push`
    ```bash
    docker push sdal/mro-c7sd_auth
    docker push sdal/rss-mro-c7sd_auth
    docker push sdal/rpkgs-mro-c7sd_auth
    docker push sdal/shy-mro-c7sd_auth
    ```
2. Do this on `lightfoot`, the production server
    1. Pull the images from dockerhub, `docker pull`
    ```bash
    docker pull sdal/mro-c7sd_auth
    docker pull sdal/rss-mro-c7sd_auth
    docker pull sdal/rpkgs-mro-c7sd_auth
    docker pull sdal/shy-mro-c7sd_auth
    ```
    2. Start up the rstudio containers, `docker-compose -f rstudio-compose.yml up -d --no-recreate`
