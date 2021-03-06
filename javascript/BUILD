load(
    "//build_defs:build_defs.bzl",
    "intellij_plugin",
    "intellij_plugin_library",
    "optional_plugin_xml",
    "stamped_plugin_xml",
)
load("//intellij_platform_sdk:build_defs.bzl", "select_for_plugin_api")
load(
    "//testing:test_defs.bzl",
    "intellij_integration_test_suite",
    "intellij_unit_test_suite",
)

licenses(["notice"])  # Apache 2.0

java_library(
    name = "javascript",
    srcs = glob(["src/**/*.java"]),
    visibility = ["//java/com/google/devtools/intellij/blaze/plugin/javascript:__pkg__"],
    deps = [
        "//base",
        "//common/experiments",
        "//common/transactions",
        "//intellij_platform_sdk:jsr305",
        "//intellij_platform_sdk:plugin_api",
        "//sdkcompat",
        "//third_party/javascript",
    ],
)

optional_plugin_xml(
    name = "optional_xml",
    module = "JavaScript",
    plugin_xml = "src/META-INF/javascript-contents.xml",
)

optional_plugin_xml(
    name = "pre-v193_xml",
    module = "JavaScript",
    plugin_xml = "src/META-INF/pre-v193.xml",
)

intellij_plugin_library(
    name = "plugin_library",
    optional_plugin_xmls = [":optional_xml"] + select_for_plugin_api({
        "intellij-2019.2": [":pre-v193_xml"],
        "intellij-ue-2019.2": [":pre-v193_xml"],
        "clion-2019.2": [":pre-v193_xml"],
        "default": [],
    }),
    plugin_xmls = ["src/META-INF/blaze-javascript.xml"],
    visibility = ["//visibility:public"],
    deps = [":javascript"],
)

stamped_plugin_xml(
    name = "javascript_plugin_xml",
    plugin_id = "com.google.idea.blaze.javascript",
    plugin_name = "com.google.idea.blaze.javascript",
    plugin_xml = "tests/integrationtests/META-INF/test.xml",
)

intellij_plugin(
    name = "javascript_integration_test_plugin",
    testonly = 1,
    plugin_xml = ":javascript_plugin_xml",
    deps = [
        ":plugin_library",
        "//base:plugin_library",
        "//java:plugin_library",
    ],
)

intellij_unit_test_suite(
    name = "unit_tests",
    srcs = glob(["tests/unittests/**/*.java"]),
    test_package_root = "com.google.idea.blaze",
    deps = [
        ":javascript",
        "//base",
        "//base:unit_test_utils",
        "//common/experiments",
        "//common/experiments:unit_test_utils",
        "//intellij_platform_sdk:jsr305",
        "//intellij_platform_sdk:plugin_api_for_tests",
        "//intellij_platform_sdk:test_libs",
        "@junit//jar",
    ],
)

intellij_integration_test_suite(
    name = "integration_tests",
    srcs = glob(["tests/integrationtests/**/*.java"]) + select_for_plugin_api({
        "intellij-2019.2": glob(["tests/integrationtests.192/**/*.java"]),
        "intellij-ue-2019.2": glob(["tests/integrationtests.192/**/*.java"]),
        "clion-2019.2": glob(["tests/integrationtests.192/**/*.java"]),
        "default": [],
    }),
    required_plugins = "com.google.idea.blaze.javascript,JavaScript",
    test_package_root = "com.google.idea.blaze",
    runtime_deps = [":javascript_integration_test_plugin"],
    deps = [
        ":javascript",
        "//base",
        "//base:integration_test_utils",
        "//base:unit_test_utils",
        "//intellij_platform_sdk:jsr305",
        "//intellij_platform_sdk:plugin_api_for_tests",
        "//intellij_platform_sdk:test_libs",
        "//sdkcompat",
        "//third_party/javascript:javascript_for_tests",
        "@junit//jar",
    ],
)
