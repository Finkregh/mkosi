# mkosi Changelog

## v10

- Minimum supported Python version is now 3.7.
- Automatic configuration of the network for Arch Linux was removed to bring
  different distros more in line with each other. To add it back, add a
  postinstall script to configure your network manager of choice.
- The `--default` option was changed to not affect the search location of
  `mkosi.default.d/`. mkosi now always searches for `mkosi.default.d/` in the
  working directory.
- `quiet` was dropped from the default kernel command line.
- `--source-file-transfer` and `--source-file-transfer-final` now accept an
  empty value as the argument which can be used to override a previous setting.
- A new command `mkosi serve` can be used to serve build artifacts using a
  small embedded HTTP server. This is useful for `machinectl pull-raw …` and
  `machinectl pull-tar …`.
- A new command `mkosi genkey` can be used to generate secure boot keys for
  use with mkosi's `--secure-boot` options. The number of days the keys should
  remain valid can be specified via `--secure-boot-valid-days=` and their CN via
  `--secure-boot-common-name=`.
- When booting images with `qemu`, firmware that supports Secure Boot will be
  used if available.
- `--source-resolve-symlinks` and `--source-resolve-symlinks-final` options are
  added to control how symlinks in the build sources are handled when
  `--source-file-transfer[-final]=copy-all` is used.
- `--build-environment=` option was added to set variables for the build script.
- `--usr-only` option was added to build images that comprise only the `/usr/`
  directory, instead of the whole root file system. This is useful for stateless
  systems where `/etc/` and `/var/` are populated by
  `systemd-tmpfiles`/`systemd-sysusers` and related calls at boot, or systems
  that are originally shipped without a root file system, but where
  `systemd-repart` adds one on the first boot.
- Support for "image versions" has been added. The version number can be set
  with `--version-number=`. It is included in the default output filename and
  passed as `$IMAGE_VERSION` to the build script. In addition, `mkosi bump` can
  be used to increase the version number by one, and `--auto-bump` can be used
  to increase it automatically after successful builds.
- Support for "image identifiers" has been added. The id can be set with
  `--image=id` and is passed to the build script as `$IMAGE_ID`.
- The list of packages to install can be configured with `--base-packages=`.
  With `--base-packages=no`, only packages specified with `--packages=` will be
  installed. With `--base-packages=conditional`, various packages will be
  installed "conditionally", i.e. only if some other package is otherwise
  pulled in. For example, `systemd-udev` may be installed only if `systemd`
  is listed in `--packages=`.
- CPIO output format has been added. This is useful for kernel initramfs images.
- Output compression can be configured with `--compress-fs=` and
  `--compress-output=`, and support for `zstd` has been added.
- `--ssh-key=` option was added to control the ssh key used to connect to the
  image.
- `--remove-files=` option was added to remove file from the generated images.
- Inline comments are now allowed in config files (anything from `#` until the
  end of the line will be ignored).
- The development branch was renamed from `master` to `main`.


## v9

### Highlighted Changes

- The mkosi Github action now defaults to the current release of mkosi instead
  of the tip of the master branch.
- Add a `ssh` verb and accompanying `--ssh` option. The latter sets up SSH keys
  for direct SSH access into a booted image, whereas the former can be used to
  start an SSH connection to the image.
- Allow for distribution specific `mkosi.*` files in subdirectories of
  `mkosi.default.d/`. These files are only processed if a subdirectory named
  after the target distribution of the image is found in `mkosi.default.d/`.
- The summary of used options for the image is now only printed when building
  the image for the first time or when the `summary` verb is used.
- All of mkosi's output, except for the build script, will now go to
  stderr. There was no clear policy on this before and this choice makes it
  easier to use images generated and booted via mkosi with language servers
  using stdin and stdout for communication.
- `--source-file-transfer` now defaults to `copy-git-others` to also include
  untracked files.
- [black](https://github.com/psf/black) is now used as a code style and
  conformance with it is checked in CI.
- Add a new `--ephemeral` option to boot into a temporary snapshot of the image
  that will be thrown away on shutdown.
- Add a new option `--network-veth` to set up a virtual Ethernet link between
  the host and the image for usage with nspawn or QEMU
- Add a new `--autologin` option to automatically log into the root account upon
  boot of the image. This is useful when using mkosi for boot tests.
- Add a new `--hostonly` option to generate host specific initrds. This is
  useful when using mkosi for boot tests.
- Add a new `--install-directory` option and special directory
  `mkosi.installdir/` that will be used as `$DESTDIR` for the build script, so
  that the contents of this directory can be shared between builds.
- Add a new `--include-directory` option and special directory
  `mkosi.includedir/` that will be mounted at `/usr/include` during the
  build. This way headers files installed during the build can be made available
  to the host system, which is useful for usage with language servers.
- Add a new `--source-file-transfer-final` option to complement
  `--source-file-transfer`. It does the same `--source-file-transfer` does for
  the build image, but for the final one.
- Add a new `--tar-strip-selinux-context` option to remove SELinux xattrs. This
  is useful when an image with a target distribution not using SELinux is
  generated on a host that is using it.
- Document the `--no-chown` option. Using this option, artifacts generated by
  mkosi are not chowned to the user invoking mkosi when it is invoked via
  sudo. It has been with as for a while, but hasn't been documented until now.

### Fixed Issues

- [#506](https://github.com/systemd/mkosi/issues/506)
- [#559](https://github.com/systemd/mkosi/issues/559)
- [#561](https://github.com/systemd/mkosi/issues/561)
- [#562](https://github.com/systemd/mkosi/issues/562)
- [#575](https://github.com/systemd/mkosi/issues/575)
- [#580](https://github.com/systemd/mkosi/issues/580)
- [#593](https://github.com/systemd/mkosi/issues/593)

### Authors

- Daan De Meyer
- Joerg Behrmann
- Luca Boccassi
- Peter Hutterer
- ValdikSS
