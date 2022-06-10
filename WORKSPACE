workspace(name = "io_tweag_rules_nixpkgs")

# For documentation

load("@io_tweag_rules_nixpkgs//nixpkgs:repositories.bzl", "rules_nixpkgs_dependencies")

rules_nixpkgs_dependencies()

load("@rules_nixpkgs_core//docs:dependencies_1.bzl", "docs_dependencies_1")

docs_dependencies_1()

load("@rules_nixpkgs_core//docs:dependencies_2.bzl", "docs_dependencies_2")

docs_dependencies_2()

# For tests

load(
    "//nixpkgs:nixpkgs.bzl",
    "nixpkgs_git_repository",
    "nixpkgs_local_repository",
    "nixpkgs_package",
)

nixpkgs_local_repository(
    name = "nixpkgs",
    nix_file = "@rules_nixpkgs_core//:nixpkgs.nix",
    nix_file_deps = ["@rules_nixpkgs_core//:nixpkgs.json"],
)

nixpkgs_package(
    name = "nix_2_4",
    attribute_path = "nix_2_4",
    repositories = {"nixpkgs": "@nixpkgs"},
)

# This is used to run Nix in a sandboxed Bazel test. See the test
# `run-test-invalid-nixpkgs-package`.
nixpkgs_package(
    name = "coreutils_static",
    attribute_path = "pkgsStatic.coreutils",
    repository = "@nixpkgs",
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "7c10271940c6bce577d51a075ae77728964db285dac0a46614a7934dc34303e6",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.26.0/rules_go-v0.26.0.tar.gz",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.26.0/rules_go-v0.26.0.tar.gz",
    ],
)

load(
    "//nixpkgs:toolchains/go.bzl",
    "nixpkgs_go_configure",
)

nixpkgs_go_configure(repository = "@nixpkgs")

load("@io_bazel_rules_go//go:deps.bzl", "go_rules_dependencies")

go_rules_dependencies()

load("//experimental:docker.bzl", "nixpkgs_docker_image")

# Usage:
#   - building: `bazel build @docker_image//:image`
#   - streaming: `bazel run @docker_image//:stream | gzip --fast | skopeo copy docker-archive:/dev/stdin docker://some.registry/docker_image:latest`
nixpkgs_docker_image(
    name = "docker_image",
    srcs = [
        "@hello//nixpkg",
        "@nixpkgs_config_cc_info//nixpkg",
    ],
    repositories = {"nixpkgs": "@nixpkgs"},
)
