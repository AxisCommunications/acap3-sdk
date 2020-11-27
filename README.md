# ACAP SDK release image Dockerfiles

## Pushing new versions

### 1. Using special branches
Keeping releases on branches does not seem like a good implementation due to the
easiness of deleting these.

### 2. Using tags and master branch
This approach is pretty good since it gives possibillity to

* Set up tests for pull requests, i.e. non-master branches
* Merge to master
* Set a tag and build a release per tag
* Both 3.2-armv7hf and 3.2-armv7hf-ubuntu19.10 works fine with correct regex
* Setting one and then the other triggers a new job and the result is two tags
  for the same digest, as desired.


+ (+++) Pros
  + Gives good control on when a release should be built
  + Quite easy to filter on release tags, e.g. "3.([])+-armv7hf([])\*"
  + Works without any additional logic
  + Old releases may be recreated manually by checking out the tag.
  + Should be pretty easy to automate a step of version in Jenkins with
    service user
    + Sed the Dockerfiles for version and ubuntu version
    + Commit the Dockerfiles locally
    + Tag the commits approperly, by parsing Dockerfiles preferably
    + Push the commit and tags to Github Master branch
    + Docker autobuild is triggered to build and tag a acap-sdk release
  + Always possible to update the release without tagging and a stricter regexp
    for Docker autobuild may be used to only build for master, i.e. for 3.2* but
    not 3.2-rc2*
- (---) Cons
  - Might be easy to misspell or miss something in a tag.
  - Hard/not possible to delete tags in GUI, see [Deleting tags](#deleting-tags)
  - Hard to retrigger, manual handling at error or deleting tag and re-tag.
  - Each new tag will run a new autobuild which gives overhead. Keeping build cache
    saves time and resources though and gives a second tag of the same artifact

### 3. Using hooks and master branch
The most flexible way it seems.

+ (+++) Pros
  + Gives good control on when a release should be built
  + May choose to keep a folder per release if desired to easily build.
  + Old releases may be recreated manually by using master and just build the
    desired version.
  + Use master branch, no need to filter. Test may run on pull request, i.e.
    non-master branches.
  + Could more easily be automized to suit specific needs, e.g. to use multiple
    tags on one build and parse the Docker image tag from a Dockerfile.
  + Could add tests that check extract Docker image tag from Dockerfile.
- (---) Cons
  - Requires more work to be done.

### Questions:
* Will we release 3.2-aarch64-ubuntu19.10 and 3.2-aarch64?
* Add sanity checks? Build a helloworld example from standard way and through
  logging in to the container, possible?
* Are we aiming at releasing for more distros and possibly different ubuntu
  distros, i.e. 19.10 and 20.10?


## Other notes

### Docker autotest
To run autotest a file ".test.yml" needs to exist in the same folder, see more
in https://docs.docker.com/docker-hub/builds/automated-testing.

#### Crux
* Not specyfing any version gives Docker compose format version 1, not tested if
  possible to specify other formats

### Creating tags
To create and list two tags for a release

``` bash
git tag 3.0-aarch64-ubuntu19.10 -a -m "Release tag 3.0-aarch64-ubuntu19.10"
git tag 3.0-aarch64 -a -m "Release tag 3.0-aarch64"
git tag
```
Push all tags at once or one at the time
``` bash
git push origin --tags
or
git push origin refs/tags/3.0-aarch64:refs/tags/3.0-aarch64
git push origin refs/tags/3.0-aarch64-ubuntu19.10:refs/tags/3.0-aarch64-ubuntu-19.10
```

### Deleting tags
It seems to not be possible in Github GUI, but pushing a deleted tag is

``` bash
git tag -d 3.0-aarch64-ubuntu19.10
git push origin :refs/tags/3.0-aarch64-ubuntu19.10
```


