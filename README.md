# Bazel + Nix + Docker

This tiny example uses Bazel to build a Docker image on top of a Nix-built base image containing a tiny Python app.  It requires Bazel and Nix to be installed, and must be run on a Linux host. 

```shell
bazel run //:hello
```
