# autoware -- Architecture Summary

## Domain
Autonomous driving stack -- orchestration meta-repository for the Autoware open-source AV framework

## Architecture Type
Meta-repository / multi-repo manifest orchestrator with layered Docker build pipeline and Ansible provisioning

## Primary Language
Dockerfile / Shell / Python / HCL (Ansible + GitHub Actions CI)

## Major Components
- repositories/ -- VCS manifest files (.repos) listing all sub-repos
- docker/ -- layered Dockerfile hierarchy (base -> core -> universe +/- CUDA) and docker-bake.hcl
- ansible/ -- environment provisioning roles (ROS 2, CUDA, TensorRT, agnocast, ML model artifacts)
- .github/workflows/ -- CI/CD pipelines for Docker build/push, health checks, lockfile management, version bumping
- src/ -- placeholder checkout directory for vcs-imported source repos
- .devcontainer/ -- VS Code devcontainer configs for core-devel and universe-devel images

## Data Stores
- HuggingFace Hub (AutowareFoundation org) -- ML model weights for perception/planning nodes
- GHCR (ghcr.io/autowarefoundation/autoware) -- published Docker images
- snapshots.ros.org -- pinned ROS APT snapshot for reproducible builds
- ansible/vars/locked-versions-*.yaml -- deterministic APT/pip/NVIDIA version lockfiles

## External Interfaces
- ROS 2 (Humble / Jazzy) middleware via CycloneDDS rmw_cyclonedds_cpp
- NVIDIA CUDA 13.0 + TensorRT -- GPU inference for perception
- Agnocast kernel module -- low-latency IPC for ROS topics (DKMS-based)
- AWSIM (end-to-end simulator) and scenario_simulator_v2 (scenario testing)
- autoware_launch -- launch files for planning-simulator and e2e_simulator demos

## Evidence Files Read
- README.md
- repositories/autoware.repos
- repositories/simulator.repos
- repositories/tools.repos
- docker/base.Dockerfile
- docker/core.Dockerfile
- docker/universe.Dockerfile
- docker/docker-bake.hcl
- docker/files/cyclonedds.xml
- docker/examples/demos/planning-simulator/docker-compose.yaml
- docker/examples/demos/awsim/docker-compose.yaml
- ansible/playbooks/install_image_deps.yaml
- ansible/roles/ros2/tasks/main.yaml
- ansible/roles/agnocast/tasks/main.yaml
- ansible/roles/tensorrt/tasks/main.yaml
- ansible/roles/artifacts/tasks/main.yaml
- ansible/roles/version_lock/tasks/main.yaml
- ansible/roles/rmw_implementation/defaults/main.yaml
- ansible/vars/locked-versions-jazzy-amd64.yaml
- .devcontainer/universe-devel/devcontainer.json
- .github/workflows/docker-build-and-push.yaml
- .github/workflows/docker-build-pipeline.yaml
- .github/workflows/health-check.yaml
- .github/workflows/health-check-reusable.yaml
- .github/workflows/bump-repo-versions-autoware.yaml
- .github/workflows/regenerate-lockfiles.yaml
- .github/workflows/watch-huggingface-hub-major.yaml
- src/README.md
