# Virt Image

Containerfile for building a virt-manager image.

This image is based on top of [`vanillaos/pico`](https://github.com/Vanilla-OS/core-image/pkgs/container/pico) and offers a virt
installation.

## Build

### Requirements

- [Vib](https://github.com/Vanilla-OS/Vib)
- Podman or Docker

## Build the image

```bash
vib build recipe.yml
podman image build -t vanillaos/virt .
```

## Run

### Requirements

- Podman or Docker
- [Distrobox](https://github.com/89luca89/distrobox)

### Usage

> The container needs to be rootfull.

```bash
distrobox create --root --init --unshare-groups --unshare-ipc --unshare-netns --unshare-process -i ghcr.io/vanilla-os/virt:main -n virt # replace with your local image if you built it
distrobox enter --root virt
```

Then start virt-manager:

```bash
virt-manager
```

## Troubleshooting

### Unathorized when sharing a USB device

This should not happen, the image has a dedicated Polkit policy for this. If you encounter this issue, please open an issue.

### Cannot connect to a remote libvirt daemon over SSH

Please, refer to [this](https://github.com/89luca89/distrobox/blob/main/docs/posts/run_libvirt_in_distrobox.md) from the Distrobox documentation.

### Default network unreachable

If the default network is unreachable, try to start it manually:

```bash
sudo virsh net-start default
virsh net-autostart default
```

if the above command fails due to missing network, try loading it from the XML:

```bash
sudo virsh net-define /usr/share/libvirt/networks/default.xml
sudo virsh net-start default
virsh net-autostart default
```

## Use of Generative AI

Maintainers may use generative AI tools as assistants while working on virt-image. Non-trivial assisted commits disclose the tool, model, and scope of the work.

AI tools may assist with code comments, documentation, repetitive code, and issue triage. Maintainers make project decisions and review every assisted change before it is merged.

Use these trailers for non-trivial assisted commits:

```plain
Assisted-by: <tool>:<model-version>
AI-Scope: <what the tool generated and the prompt or a short prompt summary>
```

Single-line completions, renames, and formatting changes do not need trailers.

Coding agents must also follow [AGENTS.md](AGENTS.md) before changing files,
creating commits, or opening pull requests.
