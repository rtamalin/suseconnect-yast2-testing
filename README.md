# Testing environment for suseconnect YaST2 integration

This repo holds tooling to assist in testing suseconnect YaST2 integration
leveraging the libsuseconnect + Ruby bindings.

## Supported SLE versions

Currently the following SLE versions are supported:

* SLE 15 SP3 - `15sp3` is the short id
* SLE 15 SP4 - `15sp4` is the short id
* SLE 15 SP5 - `15sp5` is the short id
* SLE 15 SP6 - `15sp6` is the short id
* SLE 15 SP7 - `15sp7` is the short id

Plan is to add support for the following SLE versions:

* SLE 12 SP5 - 12sp5 will be the short id

# How to setup

Copy the env.example to the appropriate file for the target SLE
version, e.g. `env.15sp7` for SLE 15 SP7, and update it with the relevant
registration codes.

## Build test images

The standard base images don't include `yast2` so we need to build custom
images with required YaST2 packages installed.

We leverage docker compose to manage building the SLE version specific
images using a common Dockerfile.sles that takes build arguments to
determine which version it is building for.

The build relies on the `env.<short_id>` file existing, so please remember
to create appropriate files before running `docker compose build`.

## Sharing files with the test environments

Associated with each SLE version environment is a directory named for the
relevant `<short_id>`, e.g. `15sp6` for SLE 15 SP6, into which you can copy
files or RPMs that you want available within the testing environment.

This directory hierarchy will be available as /work in the test environments.

# Using the test environment

You can launch standalone automatically deleted instances of the specific
SLE version environments using `docker compose run --rm -it <short_id>`.

For example to launch a SLE 15 SP6 environment you would run:

```
% docker compose run --rm -it 15sp6 /bin/bash                
Container recreate-15sp6-run-10a4ec9b482a Creating 
Container recreate-15sp6-run-10a4ec9b482a Created 
```

If you have built test suseconnect-ng packages you can copy them to the
corresponding directory that is shared with that environment, and then
install them within the test environment.

For example if you have built test packages for the v1.21.0 version of
suseconnect-ng, you could download them from OBS or IBS using the `osc
getbinaries` command, and copy them to the `15sp6` folder, and then you
can install them within the enviornment using zypper as follows:

```
Container recreate-15sp6-run-ac05d97c460b Creating
Container recreate-15sp6-run-ac05d97c460b Created
8859072d29a9:/ # zypper --no-gpg-checks install /work/*.rpm
Refreshing service 'container-suseconnect-zypp'.
Retrieving repository 'SLE_BCI' metadata .........................................................................................[done]
Building repository 'SLE_BCI' cache ..............................................................................................[done]
Loading repository data...
Reading installed packages...
Resolving package dependencies...

The following 3 packages are going to be upgraded:
  libsuseconnect suseconnect-ng suseconnect-ruby-bindings

The following 3 packages are going to change vendor:
  libsuseconnect             SUSE LLC <https://www.suse.com/> -> obs://build.opensuse.org/home:fmccarthy
  suseconnect-ng             SUSE LLC <https://www.suse.com/> -> obs://build.opensuse.org/home:fmccarthy
  suseconnect-ruby-bindings  SUSE LLC <https://www.suse.com/> -> obs://build.opensuse.org/home:fmccarthy

The following 3 packages have no support information from their vendor:
  libsuseconnect suseconnect-ng suseconnect-ruby-bindings

3 packages to upgrade, 3  to change vendor.

Package download size:     6.5 MiB

Package install size change:
              |      31.6 MiB  required by packages that will be installed
    46.1 KiB  |  -   31.6 MiB  released by packages that will be removed

Backend:  classic_rpmtrans
Continue? [y/n/v/...? shows all options] (y):
Retrieving: suseconnect-ng-1.21.0-150600.119.1.x86_64 (Plain RPM files cache)                                       (1/3),   3.9 MiB
suseconnect-ng-1.21.0-150600.119.1.x86_64.rpm:
    Header V3 RSA/SHA256 Signature, key ID 31d28dfaca1641a3: NOKEY
    V3 RSA/SHA256 Signature, key ID 31d28dfaca1641a3: NOKEY
suseconnect-ng-1.21.0-150600.119.1.x86_64 (Plain RPM files cache): Signature verification failed [4-Signatures public key is not available]
Accepting package despite the error. (--no-gpg-checks)

warning: /var/tmp/zypp.BQh6hw/zypper/_tmpRPMcache_/%CLI%/suseconnect-ng-1.21.0-150600.119.1.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID ca1641a3: NOKEY
Repository Plain RPM files cache does not define 'gpgkey=' URLs.
Retrieving: libsuseconnect-1.21.0-150600.119.1.x86_64 (Plain RPM files cache)                                       (2/3),   2.5 MiB
libsuseconnect-1.21.0-150600.119.1.x86_64.rpm:
    Header V3 RSA/SHA256 Signature, key ID 31d28dfaca1641a3: NOKEY
    V3 RSA/SHA256 Signature, key ID 31d28dfaca1641a3: NOKEY
libsuseconnect-1.21.0-150600.119.1.x86_64 (Plain RPM files cache): Signature verification failed [4-Signatures public key is not available]
Accepting package despite the error. (--no-gpg-checks)

Repository Plain RPM files cache does not define 'gpgkey=' URLs.
Retrieving: suseconnect-ruby-bindings-1.21.0-150600.119.1.x86_64 (Plain RPM files cache)                            (3/3),  31.3 KiB
suseconnect-ruby-bindings-1.21.0-150600.119.1.x86_64.rpm:
    Header V3 RSA/SHA256 Signature, key ID 31d28dfaca1641a3: NOKEY
    V3 RSA/SHA256 Signature, key ID 31d28dfaca1641a3: NOKEY
suseconnect-ruby-bindings-1.21.0-150600.119.1.x86_64 (Plain RPM files cache): Signature verification failed [4-Signatures public key is not available]
Accepting package despite the error. (--no-gpg-checks)

Repository Plain RPM files cache does not define 'gpgkey=' URLs.

Checking for file conflicts: .....................................................................................................[done]
warning: /var/cache/zypper/RPMS/suseconnect-ng-1.21.0-150600.119.1.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID ca1641a3: NOKEY
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
(1/3) Installing: suseconnect-ng-1.21.0-150600.119.1.x86_64 ......................................................................[done]
warning: /var/cache/zypper/RPMS/libsuseconnect-1.21.0-150600.119.1.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID ca1641a3: NOKEY
(2/3) Installing: libsuseconnect-1.21.0-150600.119.1.x86_64 ......................................................................[done]
warning: /var/cache/zypper/RPMS/suseconnect-ruby-bindings-1.21.0-150600.119.1.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID ca1641a3: NOKEY
(3/3) Installing: suseconnect-ruby-bindings-1.21.0-150600.119.1.x86_64 ...........................................................[done]
Running post-transaction scripts .................................................................................................[done]

8859072d29a9:/ #
```
