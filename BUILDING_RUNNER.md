# Building GitLab runner for RISC-V

This document describes how to build GitLab runner for RISC-V on a RISC-V machine.

The gitlab runner will be used as docker executor for better security implementation.

## Build machine specifications

Build Machine: RISC-V VisionFive 2
Operating System: Ubuntu 24.04.2 LTS
go version (from package manager): go1.22.2 linux/riscv64


## Build procedure

1. Install pre-requisites

```
sudo apt-get install -y docker mercurial git-core wget make build-essential
```

2. Clone the repository

```
git clone https://gitlab.com/gitlab-org/gitlab-runner.git
cd gitlab-runner
```



3. Build dependencies

```
make deps
```

4. Build the binary

```
make runner-and-helper-bin-host
```

The binary will be generated as `<root-of-repository>/out/binaries/gitlab-runner-linux-riscv64`

## Adding the GitLab runner as the systemd service

For mitigating security risks, it is better to create a separate account execute the gitlab-runner binary from there.

For the sake of convenience, let's assume the `gitlab-runner` binary is being used by `gitlab-user` and for using docker, this user should be a member of `docker` group using following command.

```
sudo usermod -aG docker gitlab-user
```

Create a systemd service file for running the runner as a service in `/etc/systemd/system/gitlab-runner.service`.

```
[Unit]
Description=GitLab Runner
After=network.target

[Service]
User=gitlab-runner
Group=gitlab-runner
ExecStart=</path/to/gitlab-runner> run
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

Change the </path/to/gitlab-runner> with the path where gitlab-runner binary is present.

Use the following command to run the service.

```
sudo systemctl daemon-reload
sudo systemctl enable gitlab-runner
sudo systemctl start gitlab-runner
```