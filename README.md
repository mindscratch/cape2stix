# Overview

Clone the repository and fetch submodules:

```
git clone git@github.com:mindscratch/cape2stix.git
cd cape2stix
git submodule update --init --recursive
```

## local dev

```
python3 -m venv .venv
source .venv/bin/activate
poetry install
```

## Build

The instructions below explain how to build the whl file. This was required because I kept having problems building on Mac, if you're on linux you likely won't need the docker image.

From the top-level direectory:

```bash
docker build -t build -f Dockerfile.build .

docker run --rm -it -v $(pwd):/app build

# once in the container
python3 -m pip install poetry && poetry install && poetry build
```

This produces:

* cape2stix-0.1.0-py3-none-any.whl
* cape2stix-0.1.0.tar.gz

