# cosienv: Docker Environment for cosipy

This project provides a Docker-based environment for `cosipy`, a tool used for simulating cosmic ray showers. The environment is set up with `conda` to manage dependencies, including the installation of `cosipy`.

> **Note:** Currently, `cosipy` is not compatible with Python 3.11 and 3.12, mainly due to installation issues with a dependency (`astromodels`, see issues [#201](https://github.com/threeML/astromodels/issues/201) and [#204](https://github.com/threeML/astromodels/issues/204)). Therefore, this environment uses Python 3.10.

## Prerequisites

Make sure you have Docker installed on your system. You can download it from [Docker's official website](https://www.docker.com/get-started).

## Building the Docker Image

1. Clone the repository or place the provided `Dockerfile` in your working directory.

If needed

   ```bash
   docker system prune
   ```

2. Run the following command to build the Docker image:

   ```bash
   
   For Mac/M1:
   
   docker build --platform linux/arm64 -t cosipyenv:1.0.0 -f Dockerfile.arm.2024 .

   or for AMD/Intel
   
   docker build --platform linux/amd64 -t cosipyenv:1.0.0 -f Dockerfile.amd.2024 .
   ```

   This will create a Docker image tagged as cosipyenv:1.0.0.

## Running the Container

Once the Docker image is built, follow these steps to run the container:

1. Start the container and open a bash session inside it:

   ```bash
   
   docker run --rm -t -d -p 8100:8888 --name cosipy1 -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:rw -v $HOME/mycosidir:/shared_dir  cosipyenv:1.0.0
   
   ```

2. Inside the container, initialize conda:

    ```bash
     docker exec -it cosipy1 bash
     
     source activate cosi

     nohup jupyter-lab --ip="*" --port 8888 --no-browser --autoreload --NotebookApp.token='XXX'  --notebook-dir=/shared_dir --allow-root > jupyterlab_start.log 2>&1 &
     ```

   This will set up the necessary shell configuration for conda.

3. To re-enter the container, use one of the following commands:

   * If the container is still running:

     ```bash
     docker exec -it cosipy1 bash
     ```

   * If the container is stopped, first start it and then re-enter:

     ```bash
     docker start cosipy1
     docker exec -it cosipy1 /bin/bash
     ```

## Verifying the Installation

Once inside the container, you can check the installation of cosipy by running the Python interpreter and importing the module:

```bash
python
```

```python
import cosipy
```

If there are no errors, the environment is set up correctly (Warnings are possible).

## Additional Notes

The image is based on Python 3.10 to avoid compatibility issues with cosipy and its dependencies.
Feel free to modify the Dockerfile to suit your specific needs or to add additional dependencies.
