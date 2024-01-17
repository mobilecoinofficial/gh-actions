# gh-actions
MobileCoin customized GitHub Actions

### Contribute

- Add composite actions in subfolders
- Note: will need to wire up builds for dockerfiles if added.

## Actions

### cache-cargo-packages

Example

```
steps:
- name: Cache cargo packages
  if: "! steps.rust_artifact_cache.outputs.cache-hit"
  uses: mobilecoinofficial/gh-actions/cache-cargo-packages@v0
  with:
    cache_buster: ${{ vars.CACHE_BUSTER }}
    additional_keys: ${{ matrix.network.chain_id }}
```

### cache-rust-binaries

Example

```
steps:
- name: Cache rust build binaries
  id: rust_artifact_cache
  uses: mobilecoinofficial/gh-actions/cache-rust-binaries@v0
  with:
    cache_buster: ${{ vars.CACHE_BUSTER }}
    additional_keys: ${{ matrix.network.chain_id }}
```

### checkout

Example

```
steps:
- name: Checkout
  uses: mobilecoinofficial/gh-actions/checkout@v0
```

### docker

Example

```
- name: Docker
  uses: mobilecoinofficial/gh-actions/docker@v0
  with:
    flavor: latest=true
    images: ${{ env.DOCKER_ORG }}/${{ env.REPO }}
    tags: |
      type=ref,event=branch
      type=semver,pattern=v{{version}}
      type=sha
    dockerfile: .internal-ci/docker/Dockerfile.${{ env.REPO }}
```

### build-downstream-rust-repo

Note: this action needs `pull-requests:write` permissions to add comments to the PR.

Example

```
jobs:
  android-bindings:
    runs-on: mcf-dev-large-x64
    container: mobilecoin/builder-install:v0.0.32
    permissions:
      pull-requests: write
    steps:
    - name: Check that android-bindings still builds
      uses: mobilecoinofficial/gh-actions/build-downstream-rust-repo@v0
      with:
        remote_repo: mobilecoinofficial/android-bindings
        submodule_path: mobilecoin
```
