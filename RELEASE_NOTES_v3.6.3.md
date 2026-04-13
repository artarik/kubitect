# Release Notes v3.6.3

## Fixed

- Fixed Terraform output generation for clusters without dedicated
  `loadBalancer.instances`.
  When `cluster.nodes.loadBalancer.vip` is explicitly configured, Kubitect now
  preserves that VIP in `output-yaml` and `infrastructure.yaml` instead of
  incorrectly replacing it with the first master node IP.

- Preserved the full process environment when invoking Terraform from Kubitect.
  This fixes remote libvirt provider failures caused by missing environment
  variables such as `HOME`, which previously led to SSH config resolution
  issues like `/.ssh/config`.

- Expanded `~` in `cluster.nodeTemplate.ssh.privateKeyPath` before reading a
  user-provided SSH key pair.
  This fixes local runs that failed with errors such as:
  `keygen: failed to read private key file id_k0s: open ~/.ssh/id_k0s: no such file or directory`

## Documentation

- Added `mkisofs` as an explicit local requirement for Kubitect because the
  Terraform libvirt provider uses it to build cloud-init ISO images.

- Added troubleshooting guidance for:
  - missing `mkisofs`
  - missing KVM support on the target libvirt host

## Notes

- Some validation and build commands could not be fully executed in the
  sandboxed environment because external Go modules were not available without
  network access.
