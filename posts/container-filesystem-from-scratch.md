## How Container Filesystems Work (Building One from Scratch)

### TL;DR
- Containers isolate processes primarily with kernel namespaces; a container filesystem is just a rootfs tree + a chroot-like pivot + a set of bind mounts.
- Union/overlay layers provide copy-on-write semantics so images stay read-only while containers get a writable upperdir.
- You can assemble a minimal container runtime with `unshare`, `pivot_root`/`chroot`, and a few bind mounts — no Docker required.

### Steps I walked through
- Create a minimal rootfs (e.g., Alpine busybox) and bind-mount `/proc`, `/sys`, `/dev` into it for basic tooling to work.
- Use `unshare -m -u -i -n -p` to create new mount, UTS, IPC, network, and PID namespaces; this isolates hostname, mounts, and processes.
- Perform `pivot_root` (or `chroot`) into the rootfs so PID 1 in the new namespace sees it as `/`.
- Start a process (e.g., `/bin/sh`) inside the new namespace; from the outside it is just another host process with different namespaces.
- Add OverlayFS for image layers: lowerdir(s) = image layers (read-only), upperdir = container writable layer, workdir = required scratch. Mount them together as the container’s rootfs.

### Key concepts to remember
- Namespaces handle isolation (PID, NET, MNT, UTS, IPC, USER); cgroups handle resource limits; OverlayFS handles copy-on-write.
- The container “image” is just a set of rootfs layers; the container runtime mounts them and provides a writable upper layer.
- No VM here: processes are host processes with constrained views of the world.

### Practical quickstart (pseudo-commands)
- Prepare rootfs: `mkdir -p /tmp/rootfs/{upper,work,merged}` and have an extracted base in `/tmp/rootfs/base`.
- Mount overlay: `mount -t overlay overlay -o lowerdir=/tmp/rootfs/base,upperdir=/tmp/rootfs/upper,workdir=/tmp/rootfs/work /tmp/rootfs/merged`.
- Bind essentials: `mount --bind /proc /tmp/rootfs/merged/proc` (repeat for `/sys`, `/dev` as needed).
- Enter namespaces: `unshare -fpun --mount-proc=/tmp/rootfs/merged/proc chroot /tmp/rootfs/merged /bin/sh`.

### Why this matters for DevSecOps/SRE
- Faster incident debugging: understanding namespaces and overlay helps when `/proc` looks empty or mounts differ in a container vs. host.
- Security reviews: validate mount options (noexec,nodev,nosuid) and ensure only required host paths are bind-mounted.
- Performance/cost: recognize when overlay churn or log writes explode your upperdir; place writable layers on appropriate storage.
