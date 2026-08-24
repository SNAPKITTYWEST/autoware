# autoware — Architecture Summary

## Domain
Autonomous driving stack — meta-repository orchestrating the Autoware open-source AV framework across ROS 2, C++, perception/planning/control sub-repositories

## Architecture Type
Meta-repository / multi-repo manifest orchestrator with layered Docker build pipeline and Ansible provisioning

## Primary Language
Dockerfile

## Major Components
- repositories/ — VCS manifest .repos files (autoware, simulator, tools) referencing ~30 sub-repos
- docker/ — layered Dockerfile hierarchy: base → core → universe ± CUDA variants plus docker-bake.hcl
- ansible/ — Ansible roles for environment provisioning: ros2, agnocast, cuda, tensorrt, spconv, artifacts, build_tools, dev_tools, rmw_implementation, version_lock, acados, qt5ct_setup, geographiclib
- .github/workflows/ — CI/CD: docker-build-and-push, health-check, regenerate-lockfiles, bump-repo-versions, watch-huggingface-hub-major
- src/ — empty placeholder directory populated at build time via vcs import
- .devcontainer/ — VS Code devcontainer configs for core-devel and universe-devel images

## Data Stores
- HuggingFace Hub (AutowareFoundation org) — ML model weights for perception/planning (lidar_centerpoint, diffusion_planner, bevfusion, tensorrt_yolox, etc.)
- GHCR (ghcr.io/autowarefoundation/autoware) — published Docker images
- snapshots.ros.org — pinned ROS APT snapshot (ros_snapshot_date: 2026-04-13 for jazzy-amd64)
- ansible/vars/locked-versions-*.yaml — deterministic APT/pip/NVIDIA version lockfiles
- /home/aw/autoware_data/ — ML model weights on host/container filesystem

## External Interfaces
- ROS 2 Humble (Ubuntu 22.04) and Jazzy (Ubuntu 24.04) via rmw_cyclonedds_cpp / CycloneDDS
- NVIDIA CUDA 13.0 + TensorRT — GPU inference for perception
- Agnocast DKMS kernel module v2.3.5 — low-latency IPC for high-throughput ROS topics
- AWSIM simulator — end-to-end GPU-accelerated simulation (universe-cuda-jazzy image)
- scenario_simulator_v2 v22.0.0 — scenario-based testing
- autoware_launch v0.52.0 — launch files for planning-simulator and e2e_simulator

## Evidence Files Read
- README.md
- TRAINING/ARCHITECTURE_SUMMARY.md
- TRAINING/TRAINING_CORPUS.json
- TRAINING/METADATA_TREE.json
- TRAINING/RELATIONSHIP_GRAPH.json
- repositories/autoware.repos
- repositories/simulator.repos
- docker/base.Dockerfile
- docker/core.Dockerfile
- docker/universe.Dockerfile
- docker/docker-bake.hcl
- docker/files/cyclonedds.xml
- docker/docker-entrypoint.sh
- docker/examples/demos/awsim/docker-compose.yaml
- ansible/playbooks/install_image_deps.yaml
- ansible/roles/agnocast/tasks/main.yaml
- ansible/roles/artifacts/tasks/main.yaml
- ansible/vars/locked-versions-jazzy-amd64.yaml
- .github/workflows/docker-build-and-push.yaml
- .github/workflows/health-check.yaml
