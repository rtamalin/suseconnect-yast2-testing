# Testing environment for suseconnect YaST2 integration

This repo holds tooling to assist in testing suseconnect YaST2 integration
leveraging the libsuseconnect + Ruby bindings.

## Supported SLE versions

Currently the following SLE versions are supported:

* SLE 15 SP3 - 15sp3 is the short id
* SLE 15 SP4 - 15sp4 is the short id
* SLE 15 SP5 - 15sp5 is the short id
* SLE 15 SP6 - 15sp6 is the short id
* SLE 15 SP7 - 15sp7 is the short id

Plan is to add support for the following SLE versions:

* SLE 12 SP5 - 12sp5 will be the short id

## How to setup

Copy the env.example to the appropriate file for the target SLE
version, e.g. env.15sp7 for SLE 15 SP7, and update it with the relevant
registration codes.

## Built test images

The standard base images don't include yast2 so we need to build custom
images with required yast2 packages installed.

We leverage docker compose to manage building the SLE version specific
images using a common Dockerfile.sles that takes build arguments to
determine which version it is building for.

The build relies on the `env.<short_id>` file existing, so please remember
to create appropriate files before running `docker compose build`.

# Using the test environment

You can launch standalone automatically deleted instances of the specific
SLE version environments using `docker compose run --rm -it <short_id>`.

For example to launch a SLE 15 SP6 environment you would run:

```
% docker compose run --rm -it 15sp6 /bin/bash                
Container recreate-15sp6-run-10a4ec9b482a Creating 
Container recreate-15sp6-run-10a4ec9b482a Created 
```
