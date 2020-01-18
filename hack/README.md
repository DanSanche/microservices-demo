## `hack/` 

This directory provides scripts for building and pushing Docker images, and tagging new demo
releases. 

### release process
The process of releasing a new version of Hipstershop is made up of the following steps:
- build new flat kubernetes and istio manifests at ./release/
- push new tagged containers to gcr.io/google-samples/microservices-demo
- push a new git tag to the repo

### env variables 

- `TAG` - git release tag / Docker tag. 
- `REPO_PREFIX` - Docker repo prefix to push images. Format: `$user/$project`. e.g. gcr.io/google-samples/microservices-demo.
  Resulting images will be of the format `$user/$project/$svcname:$tag` (where `svcname` = `adservice`, `cartservice`, etc.)

### scripts 

1. `./make-docker-images.sh`: builds and pushes images to the specified Docker repository.
2. `./make-release-artifacts.sh`: generates a combined YAML file with image $TAG at: 
   `./release/kubernetes-manifests.yaml` and `./release/istio-manifests.yaml`.
3. `./propose-release.sh`: runs scripts 1 and 2, then pushes a `release/$TAG` branch to the repo for review.
   GitHub Actions will automatically create a PR from the new branch, and will tag the release when the PR is merged
