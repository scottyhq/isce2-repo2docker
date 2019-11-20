# isce2-repo2docker

build [isce2](https://github.com/isce-framework/isce2) jupyterhub-compatible image with github actions

![Action Status](https://github.com/scottyhq/isce2-repo2docker/workflows/Repo2Docker/badge.svg)

* commits to master branch trigger re-building 'latest' image
* pushing a tag results in a docker image with the same tag:
```
git tag -am "test v2.3.2" v2.3.2
git push --tags
```

images stored on dockerhub public repo:
https://hub.docker.com/repository/docker/scottyhq/isce2-repo2docker


## instructions for building locally
```
git clone https://github.com/scottyhq/isce2-repo2docker.git
cd isce2-repo2docker
conda create repo2docker
conda activate repo2docker
pip install -r requirements.txt
export IMAGE=isce2-repo2docker
export TAG=latest
jupyter-repo2docker --debug --user-name jovyan --user-id 1000 --no-run --image-name $IMAGE:$TAG $PWD
docker login -u scottyhq --password-stdin
docker tag $IMAGE:$TAG
docker push docker.pkg.github.com/$IMAGE:$TAG
```

## instructions for running jupyterlab from image locally
```
docker pull docker.pkg.github.com/$IMAGE:$TAG
docker run -it --name repo2docker -p 8888:8888 $IMAGE:$TAG jupyter lab --ip 0.0.0.0
docker stop repo2docker
docker rm repo2docker
```
