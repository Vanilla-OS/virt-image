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
