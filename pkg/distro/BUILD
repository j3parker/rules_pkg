package(
    default_visibility = ["//visibility:private"],
)

load("@rules_pkg//:pkg.bzl", "pkg_tar", "version")

# Build the artifact to put on the github release page.
pkg_tar(
    name = "rules_pkg-%s" % version,
    srcs = [
        "@rules_pkg//:standard_package",
    ],
    extension = "tar.gz",
    # It is all source code, so make it read-only.
    mode = "0444",
    # Make it owned by root so it does not have the uid of the CI robot.
    owner = "0.0",
    package_dir = ".",
)

py_library(
    name = "util",
    srcs = [
        "__init__.py",
        "release_tools.py",
        ":release_version.py",
    ],
    srcs_version = "PY3",
    deps = [
        "@bazel_tools//tools/python/runfiles",
    ],
)

py_binary(
    name = "print_rel_notes",
    srcs = [
        "print_rel_notes.py",
    ],
    data = [
        ":rules_pkg-%s.tar.gz" % version,
    ],
    python_version = "PY3",
    deps = [
        ":util",
        "@bazel_tools//tools/python/runfiles",
    ],
)

py_test(
    name = "packaging_test",
    size = "large",
    srcs = [
        "packaging_test.py",
    ],
    data = [
        ":rules_pkg-%s.tar.gz" % version,
        "testdata/BUILD.tmpl",
    ],
    local = True,
    python_version = "PY3",
    tags = [
        "no_windows",
        "noci",
    ],
    deps = [
        ":util",
        "@bazel_tools//tools/python/runfiles",
    ],
)

genrule(
    name = "version_as_py",
    outs = ["release_version.py"],
    cmd = "echo RELEASE_VERSION = \\'%s\\' >$@" % version
)
