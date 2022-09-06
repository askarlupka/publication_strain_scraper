# Getting Started

Python's package management system is.... challenging.  To trim down the confusion, install packages with `pip` or `conda`.  Both systems try to solve the same challenge:  dependencies.  `conda` was designed for scientists and it tries to build the most stable versions of packages it can.   Mixing these 2 commands is perfectly fine but may, in rare instances, cause problems.  

## Requirements:

* [Anaconda](https://www.anaconda.com/)

## Virtual Environments

A virtual environment is a way of sandboxing your packages so that you always know what versions of things are installed. When using Python's default configuration all packages are installed to the same place.  The problem with this approach is you might want to try an updated version of some package which replaces the one installed.  If you then go back to old code it may no longer work.  Virtual environments guarantee stability.  

## First Time Setup

The following steps only need to be once during our initial setup.  

### Conda Virtual Environment

Creating a new conda environment is simple (say yes when prompted):

```
conda create -n difz_training -f difz_environment.yml
```

This will load all the depedencies from `difz_environment.yml` and install them in to your new **difz_training** environment.  Now we need to make sure out environment is visible to Jupyter.  Jupyter uses something called Kernels.  Kernels are just virtual Python environments in to which Jupyter runs your code.  We need **difz_training** to be available.

```
conda activate difz_training
python -m ipykernel install --user --name difz_training --display-name "difz_training"
```

> NOTE:  Make sure you run `conda activate difz_training`.  The command after that assumes we're in the environment we want added to Jupyter.  In theory `--name` should work without needing to be in the actual environment but it has been known to fail.  


### Setting up spacy

[spacy](https://spacy.io/) is one of the more modern NLP libraries.  The other being [NLTK](https://www.nltk.org/).  Both serve a purpose but **spacy** is more robust and arguably friendlier.  Both libraries have lite installers meaning they do not come with word corpuses.  This is by design to keep them from being insanely sized.  You need to run the command below once time to download a [language model](https://spacy.io/models/en).

```
conda activate difz_training
python -m spacy download en_core_web_sm
```

Usage of **spacy** can be seen in the LSH notebook.

### Setting up PyTorch

The following will install [PyTorch](https://pytorch.org/) for CPU.  
```
conda install pytorch torchvision torchaudio -c pytorch
```


## Subsequent Runs

Always make sure to activate your conda environment:

```
conda activate difz_training
```


When in doubt, a list of conda environments can be found here:

```
conda env list
```

# Docker

To build a Docker Image you need a `Dockerfile`, here is a simple example:

```dockerfile
FROM python:3.9

CMD ["python","--version"]
```

* **FROM** - Every Docker 

## Terminology

* **Dockerfile** - A blueprint that tells Docker how to create an image.
* **Image** - If Dockerfiles are blueprints, think of Images as akin to demonstration homes.  They aren't directly usable but can be shared with prospective owners who may wish to buy a version of that home (a Container).
* **Containers** - Containers are Images that have been instantiatied.  In other words you saw the demo home, wanted your own, so they built you a copy and you live there now.  

Docker creates an Image from a Dockerfile -> Docker instantiates an Image to create a live Container -> Live Container runs your code.

## Building a Dockerfile

```
docker build -t example .
```

`build` tells Docker to build a new Image.  `-t example` tells Docker to name this new Image `example`.  The `.` at the end tells Docker where to look for the Dockerfile.

## Running a Dockerfile Once and Close

```
docker run example
```
`run` tells Docker to run a specific Image which creates a Container.  Here we're running our `example` Image.  

## Running a Dockerfile Once and Leave Open

Some Dockerfiles are meant to run as services meaning they do not shutdown automatically.  The example above will always shutdown.

The nice thing about Docker is we can override the default behaviour of a container to keep it open:

```
docker run -dit --entrypoint /bin/bash example
```

Like the last example only we specifiy `-dit`.  `-dit` is shorthand for `-d -i -t`.  `-d` tells docker to run in detached mode meaning start the container but then return.  `-i` tells Docker to make the container interactive, this would let us connect to it if we chose.  `-t` tells Docker to connect our terminal/shell to the container.  

`--entrypoint /bin/bash` is the key here.  What it says is to ignore the last line of the docker file and instead run `/bin/bash`.  This essentially tells your container to run a terminal which is a non-terminating operation.  

Now you can connect to that container.  First, get a list of all active Docker containers:

```
docker ps
```

Find the one labelled `example` and copy the **CONTAINER ID**, an alphanumeric string.  Replace `<CONTAINER ID>` in the following with that value

```
docker exec -it <CONTAINER ID> /bin/bash
```

Now you're inside your container!!!  Woo.  Run `python --version` to confirm.  To exit type `exit`

## Moving data in and out of your container

This one is easier than it seems.  Your container is basically a second computer living inside your computer.  What we need to do is tell docker that it's ok to connect a folder from our host (your main computer) to our guest (the container):

```
docker run -v $(pwd):/home/host -dit --entrypoint /bin/bash example 
```

`-v $(pwd):/home/host` is the new piece.  `-v` tells docker to create a passthrough from a folder on your main computer `$(pwd)` to a folder inside the container `/home/host`.  `/home/host` is not a required name.  I like to explicitly call folders I mount `host` so that I know they come from the host.

`$(pwd)` is just shorthand for 'get my current directory'.  You should always use the full path.  `-v $(pwd):/home/host` is equivalent to `-v .:/home/host` but I've explicitly given the full path to the current folder.  You can test this like so:

```
docker exec -it <CONTAINER ID> /bin/bash
```

```
python /home/host/sample_docker_script2.py
```

Or do this to see all the folders of your main machine inside the container:

```
ls /home/host
```

> `-v` is a direct connection between host and guest.  These aren't copies of your files, these are the originals.  

### Docker cheatsheet

Build a docker image

```
docker built -t <NAME> .
```

List Docker images:

```
docker images
```

List active Docker containers:

```
docker ps
```

Kill an active Docker container:

```
docker kill <CONTAINER ID>
```
