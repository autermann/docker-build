# Docker Build Script

Build scripts that automatically tags an image based on the Git metadata.


## Usage

```
Usage: docker-build [options] <path>

Parameters:
  path                             The path to the git repository (defaults to the current working directory)

Options:
  -b, --build-arg <build-arg>      Additional build arguments (may occur multiple times).
  -c, --context <build-context>    The build context (defaults to .)
                                   This can also be set using the environment variable BUILD_CONTEXT
  -f, --file <path-to-Dockerfile>  The path to the Dockerfile (defaults to Dockerfile)
                                   This can also be set using the environment variable DOCKER_FILE
  -l, --latest                     If this image should be tagged as 'latest'
                                   This can also be set using the environment variable LATEST=true
  -L, --latest-branch <branch>     If the specified branch is build the image will be tagged as 'latest'
                                   This can also be set using the environment variable LATEST_BRANCH
      --license                    The license of this image.
                                   This can also be set using the environment variable LICENSE
      --maintainer <maintainer>    The image maintainer to be added as a label (defaults to the committer)
                                   This can also be set using the environment variable MAINTAINER
  -p, --password <password>        The password for the registry
                                   This can also be set using the environment variable REGISTRY_PASS
      --prune                      If the build image should be removed afterwards
                                   This can also be set using the environment variable PRUNE=true
      --pull                       Should docker build pull images (defaults to false)
                                   This can also be set using the environment variable PULL=true
      --push                       If the images should be pushed (defaults to false)
                                   This can also be set using the environment variable PUSH=true
      --version-level <level>      The level of the version up to which tags should be generated. One
                                   of major, minor or patch (defaults to patch)
                                   This can also be set using the environment variable VERSION_LEVEL
      --no-commit                  Do not create tags for the Git commit
                                   This can also be set using the environment variable NO_COMMIT=true
      --no-branch                  Do not create a tag named after the branch.
                                   This can also be set using the environment variable NO_BRANCH=true
  -r, --repository <repository>    The repository name (defaults to the path of the git remote URL)
                                   This can also be set using the environment variable REPOSITORY
  -R, --registry <registry>        The registry to push to (defaults to docker.52north.org)
                                   This can also be set using the environment variable REGISTRY
  -s, --suffix <suffix>            The suffix to apply to the generated image tags (e.g. 'alpine')
                                   This can also be set using the environment variable TAG_SUFFIX
  -u, --username <username>        The username for the registry
                                   This can also be set using the environment variable REGISTRY_USER
      --url <url>                  The project URL to be added as a image label (defaults to the git remote URL)
                                   This can also be set using the environment variable URL
  -v, --version <version>          The version to use for this image (in addition to any git tag)
                                   This can also be set using the environment variable VERSION
      --vendor <vendor>            The vendor to be added as a image label (defaults to 52°North GmbH)
                                   This can also be set using the environment variable VENDOR
  -h, --help                       Output usage information
```

## Example `Makefile`

```make
#!make
CURL               := $(shell which curl)
DOCKER             := $(shell which docker)
BUILD_DIR          := .build
DOCKER_BUILD       := ./$(BUILD_DIR)/docker-build
DOCKER_BUILD_URL   := "https://raw.githubusercontent.com/autermann/docker-build/master/docker-build"
DOCKER_BUILD_FLAGS := --push --pull --latest-branch master

.PHONY: all clean docker
all: docker

$(DOCKER_BUILD):
	@mkdir -p $(BUILD_DIR)
	@$(CURL) -sLf $(DOCKER_BUILD_URL) -o $(DOCKER_BUILD)
	@chmod +x $(DOCKER_BUILD)

clean:
	@rm -rf $(BUILD_DIR)

docker: $(DOCKER_BUILD)
	$(DOCKER_BUILD) $(DOCKER_BUILD_FLAGS) .
```
