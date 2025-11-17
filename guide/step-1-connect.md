**Step 1 \- Connect & verify**

Make sure you can access RHEL AI VM prepared for you: `ssh cloud-user@bastion.xxxxx.sandboxxxxx.xxxxxxx.com`

If it fails → call a facilitator.

Check out your GPUs:`nvidia-smi`

nvidia-smi (NVIDIA System Management Interface) is a command-line utility for monitoring and managing
NVIDIA GPU devices. It provides real-time information on GPU usage, temperature, power consumption, and
memory, along with details about the GPU model, driver version, and CUDA version

Check Podman version: `podman --version`

Podman is a daemonless, open source, Linux native tool designed to make it easy to find, run, build, share
and deploy applications using Open Containers Initiative (OCI) Containers and Container Images.

If either fails → call a facilitator.