[![Build Status](https://travis-ci.com/IllumiDesk/flatiron-stacks.svg?branch=main)](https://travis-ci.com/IllumiDesk/flatiron-stacks)

# Flatiron Stacks

Dockerfiles and related assets for IllumiDesk's workspace images for the `Flatiron School`.

## Pre Requisits

- [Docker](https://docs.docker.com/get-docker/)

## Quickstart

1. Install dependencies

```bash
make venv
```

2. Build images

```bash
make build-all
```

3. Run:

Running the image standalone is helpful for testing:

```bash
docker run -p 8888:8888 illumidesk/flatiron-notebook:latest
```

Then, navigate to `http://localhost:8888` to access your Jupyter Notebook server.

> Refer to [docker's documentation](https://docs.docker.com/engine/reference/run/) for additional `docker run ...` options.

4. Test:

```bash
make test
```

## Image catalogue

| Image | DockerHub Link |
| --- | --- |
| illumidesk/flatiron-base | [![Docker Image](https://img.shields.io/docker/automated/illumidesk/flatiron-base)](https://img.shields.io/docker/automated/illumidesk/flatiron-base?label=flatiron-base) |
| illumidesk/flatiron-notebook | [![Docker Image](https://img.shields.io/docker/automated/illumidesk/flatiron-notebook)](https://hub.docker.com/repository/docker/illumidesk/flatiron-notebook?label=flatiron-notebook) |
| illumidesk/flatiron-grader | [![Docker Image](https://img.shields.io/docker/automated/illumidesk/flatiron-grader)](https://hub.docker.com/repository/docker/illumidesk/flatiron-grader?label=flatiron-grader) |

## Build Mechanism

1. Build and tag the base image or all images at once. Use the `TAG` argument to add your custom tag. The `TAG` argument defaults to `latest` if not specified.

Build all images:

```bash
make build-all
```

Build one image with custom tag:

```bash
make build/flatiron-base TAG=mytag
```

> The base image uses the standard `repo2docker` convention to set dependencies. [Refer to this project's documentaiton](https://repo2docker.readthedocs.io/en/latest/) for additional configuration options.


1. (Optional) Use the base image from step 1 above as a base image for an image compatible with the IllumiDesk stack.

```
FROM jupyter/base-notebook:latest AS base

FROM illumidesk/flatiron-base:latest

USER root

COPY --from=base /usr/local/bin/start* /usr/local/bin/
COPY --from=base /usr/local/bin/fix-permissions /usr/local/bin/
COPY --from=base /etc/jupyter/jupyter_notebook_config.py /etc/jupyter/

RUN ... do stuff

RUN fix-permissions "${HOME} \
 && fix-permissions "${CONDA_DIR}  # make sure you run fix-permissions after doing stuff

USER "${NB_USER}"

```

2. (Optional) Push images to DockerHub

This step requires creating an Organization account in DockerHub or other docker image compatible registry. The `docker push ...`
command will push the image to the DockerHub registry by default. Please refer to the official Docker documentation if you would
like to push another registry.

For example, assuming the DockerHub organization is `illumidesk`, the source files are in the `flatiron-notebook` folder, and the tag is `latest`, then the full namespace for the image would be `illumidesk/flatiron-notebook:latest`. Assuming the image has been built, push the image to DockerHub or any other docker registry with the `docker push ...` command:

```bash
docker push illumidesk/flatiron-notebook:latest
```

## Development and Testing

1. Create your virtual environment and install dev-requirements:

```bash
make venv
```

2. Check Dockerfiles with linter:

```base
make lint-all
```

3. Run tests:

```base
make test
```

## References

These images are based on the `jupyter/docker-stacks` images. [Refer to their documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/) for the full set of configuration options.

## Attributions

- [JupyterHub repo2docker](https://repo2docker.readthedocs.io/en/latest/)
- [jupyter/docker-stacks images](https://github.com/jupyter/docker-stacks)

## License

MIT