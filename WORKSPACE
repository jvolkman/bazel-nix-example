workspace(name = "bazel_nix_example")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

######################
# Nix
######################
http_archive(
    name = "io_tweag_rules_nixpkgs",
    sha256 = "7aee35c95251c1751e765f7da09c3bb096d41e6d6dca3c72544781a5573be4aa",
    strip_prefix = "rules_nixpkgs-0.8.0",
    urls = ["https://github.com/tweag/rules_nixpkgs/archive/v0.8.0.tar.gz"],
)

load("@io_tweag_rules_nixpkgs//nixpkgs:repositories.bzl", "rules_nixpkgs_dependencies")

rules_nixpkgs_dependencies()

load("@io_tweag_rules_nixpkgs//nixpkgs:nixpkgs.bzl", "nixpkgs_git_repository", "nixpkgs_package", "nixpkgs_python_configure")

nixpkgs_git_repository(
    name = "nixpkgs",
    revision = "21.05",  # Any tag or commit hash
    sha256 = "",  # optional sha to verify package integrity!
)

nixpkgs_python_configure(
    repository = "@nixpkgs//:default.nix",
)

######################
# Python Support
######################
http_archive(
    name = "rules_python",
    sha256 = "934c9ceb552e84577b0faf1e5a2f0450314985b4d8712b2b70717dc679fdc01b",
    url = "https://github.com/bazelbuild/rules_python/releases/download/0.3.0/rules_python-0.3.0.tar.gz",
)

#########
# Docker
#########

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "59d5b42ac315e7eadffa944e86e90c2990110a1c8075f1cd145f487e999d22b3",
    strip_prefix = "rules_docker-0.17.0",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.17.0/rules_docker-v0.17.0.tar.gz"],
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")

container_deps()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_load",
)

load("@io_bazel_rules_docker//repositories:py_repositories.bzl", "py_deps")

py_deps()

load(
    "@io_bazel_rules_docker//python:image.bzl",
    py_image_repos = "repositories",
)

py_image_repos()

load(
    "@io_bazel_rules_docker//python3:image.bzl",
    py3_image_repos = "repositories",
)

py3_image_repos()

nixpkgs_package(
    name = "raw_python38_base_image",
    build_file_content = """
package(default_visibility = [ "//visibility:public" ])
exports_files(["image"])
    """,
    nix_file = "//:python38_base_image.nix",
    repository = "@nixpkgs//:default.nix",
)

container_load(
    name = "python38_base_image",
    file = "@raw_python38_base_image//:image",
)

