# ROCm devcontainer Setup

## Build & Run

```sh
devcontainer up --workspace-folder . --dotfiles-repository https://github.com/WesleyCh3n/dotfiles --dotfiles-install-command ".setup"
devcontainer exec --workspace-folder . zsh
```

## Stop

```sh
docker stop rocm-dev
docker rmi $(docker images --filter=reference="vsc*" -q)
```

