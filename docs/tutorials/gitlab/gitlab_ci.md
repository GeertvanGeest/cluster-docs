Gitlab and GitHub CI are extremely useful for running processes that are related to your software development project, like testing, building container images, deploying webpages etc. 

In gitlab, all you need to do is generate a file called `.gitlab-ci.yml`, and specify there what kind of processes need to run. In such a file you specify:

- When someting needs to be run (e.g. after a push to the main branch, or after a release)
- In what kind of environment it needs to run (often an image on dockerhub)
- What needs to be run (e.g. running a test script)

In order to get started with gitlab ci, have a look [here](https://docs.gitlab.com/ee/ci/quick_start/).

## Build and push image example

This example of `.gitlab-ci.yml` does the following:

- Pull and use docker-in-docker (dind) container
- Generate an image tag based on the CI container registry name and a repository tag (i.e. release). 

!!! info "CI container registry"
    By default, the CI container registry is the registry associated to the repo; find it at **Packages & Registries** > **Container Registry**) 

- Login to the CI container registry 
- Build the image using the repository as a context (i.e. using a `Dockerfile` that is in the base directory of the repo)
- Push the built image to the container registry
- Run the CI pipeline only when a tag (i.e. release) is pushed. By default it will with each push. 

!!! info "Predefined variables"
    Many of the variables used in this `yml` are predefined. Have a look at the reference here: [https://docs.gitlab.com/ee/ci/variables/predefined_variables.html](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html). 

```yaml
variables:
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: ""

stages:
  - build

build:
  image: docker:20.10.16
  stage: build
  services:
    - docker:20.10.16-dind
  variables:
    IMAGE_TAG: $CI_REGISTRY_IMAGE:$CI_COMMIT_TAG
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $IMAGE_TAG .
    - docker push $IMAGE_TAG
  only:
    - tags

```

If everything goes well, the image will be pushed to the container registry. In order to pull the image:

=== "Docker"
    ```sh
    docker login gitlab.bioinformatics.unibe.ch:5050

    docker pull \
    gitlab.bioinformatics.unibe.ch:5050/[namespace]/[repo name]/[image name]:[tag name]

    ```

=== "apptainer"
    ```sh
    # use login only the first time
    apptainer remote login --username foo docker://gitlab.bioinformatics.unibe.ch:5050

    apptainer pull \
    docker://gitlab.bioinformatics.unibe.ch:5050/[namespace]/[repo name]/[image name]:[tag name]
    ```

🥳 :tada:


## R-package build example

This pipeline installs a package inside a container, and deploys a documentation website. It assumes the project uses a.o. [`renv`](https://rstudio.github.io/renv/articles/renv.html), and [`pkgdown`](https://pkgdown.r-lib.org/). While building pages, it makes efficient use of the cache. 

=== "`Dockerfile`"
    ```Dockerfile
    FROM rocker/tidyverse:4.2.1

    COPY . /IBURNApipe

    WORKDIR /IBURNApipe

    RUN apt-get -y update -qq \
    && apt-get install -y --no-install-recommends \
        pandoc libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev \
        libtiff5-dev libjpeg-dev libcurl4-openssl-dev libfontconfig1-dev \
        libxml2-dev libssl-dev default-jre default-jdk

    RUN R -e "if (!requireNamespace('renv', quietly = TRUE)) install.packages('renv')"
    RUN R -e "renv::restore()"
    RUN R -e "if (!requireNamespace('devtools', quietly = TRUE)) install.packages('devtools')"
    RUN R -e "devtools::install()"

    RUN Rscript additional_packages_container.R

    ```

=== "`.gitlab-ci.yml`"
    ```yaml
    image: rocker/tidyverse:latest

    variables:
        DOCKER_DRIVER: overlay2
        PKGNAME: IBURNApipe
        CHECK_DIR: ${CI_PROJECT_DIR}/ci/logs
        BUILD_LOGS_DIR: ${CI_PROJECT_DIR}/ci/logs/${PKGNAME}.Rcheck
        RENV_PATHS_CACHE: ${CI_PROJECT_DIR}/cache
        RENV_PATHS_LIBRARY: ${CI_PROJECT_DIR}/renv/library
        DOCKER_TLS_CERTDIR: ""

    cache:
        key: ${CI_JOB_NAME}
        paths:
            - ${RENV_PATHS_CACHE}
            - ${RENV_PATHS_LIBRARY}

    stages:
        - build
        - deploy

    build:
    image: docker:20.10.16
    stage: build
    services:
        - docker:20.10.16-dind
    variables:
        IMAGE_TAG: $CI_REGISTRY_IMAGE:$CI_COMMIT_TAG
    script:
        - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
        - docker build -t $IMAGE_TAG .
        - docker push $IMAGE_TAG
    only:
        - tags

    pages:
        stage: deploy
        script:
            - apt-get update
            - apt-get -y install pandoc libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev libcurl4-openssl-dev libfontconfig1-dev libxml2-dev libssl-dev
            - Rscript -e "if (!requireNamespace('renv', quietly = TRUE)) install.packages('renv')"
            - Rscript -e "renv::restore()"
            - R -e "pkgdown::build_site()"
        artifacts:
            paths:
                - public
            expire_in: 1 day
        rules:
            - if: $CI_COMMIT_REF_NAME == $CI_DEFAULT_BRANCH

    ```
