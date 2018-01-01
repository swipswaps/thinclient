TODO: write detailed documentation.

## Provision

Server machine is used to provision both itself and template machine, because we don't want additional packages(ansible) inside image. But we have to install python2-minimal to make ansible work.

## Generating image

Some directories are excluded from rootfs image to make it more compact: `/boot`, `/usr/share/doc`, `/var/lib/apt/lists` and others, see `build.sh`.

Vagrant need static addresses in `/etc/network/interfaces`, so before generating image, it is replaced with symlink to `/tmp/interfaces`. After generating image it is moved back.

File `/tmp/interfaces` is generated by special script, used as systemd service. It enables DHCP for all network interfaces found on the machine.

## Initrd hacks

initrd has custom booot script `ram` and hook to incude necessary binaries and modules. Script name is passed to kernel in boot parameters.

Overlays(optional) are mounted using AUFS.