licenses(["notice"])  # Apache 2

load(
    "//bazel:envoy_build_system.bzl",
    "envoy_cc_library",
    "envoy_package",
    "envoy_proto_library",
)

envoy_package()

envoy_cc_library(
    name = "cds_api_lib",
    srcs = ["cds_api_impl.cc"],
    hdrs = ["cds_api_impl.h"],
    deps = [
        "//include/envoy/config:subscription_interface",
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/local_info:local_info_interface",
        "//source/common/common:cleanup_lib",
        "//source/common/common:minimal_logger_lib",
        "//source/common/config:resources_lib",
        "//source/common/config:subscription_factory_lib",
        "//source/common/config:utility_lib",
        "//source/common/protobuf:utility_lib",
        "@envoy_api//envoy/api/v2:cds_cc",
    ],
)

envoy_cc_library(
    name = "cluster_manager_lib",
    srcs = ["cluster_manager_impl.cc"],
    hdrs = ["cluster_manager_impl.h"],
    deps = [
        ":cds_api_lib",
        ":load_balancer_lib",
        ":load_stats_reporter_lib",
        ":ring_hash_lb_lib",
        ":subset_lb_lib",
        "//include/envoy/api:api_interface",
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/http:codes_interface",
        "//include/envoy/local_info:local_info_interface",
        "//include/envoy/network:dns_interface",
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/ssl:context_manager_interface",
        "//include/envoy/thread_local:thread_local_interface",
        "//include/envoy/upstream:cluster_manager_interface",
        "//source/common/common:enum_to_int",
        "//source/common/common:utility_lib",
        "//source/common/config:cds_json_lib",
        "//source/common/config:grpc_mux_lib",
        "//source/common/config:utility_lib",
        "//source/common/grpc:async_client_manager_lib",
        "//source/common/http:async_client_lib",
        "//source/common/http/http1:conn_pool_lib",
        "//source/common/http/http2:conn_pool_lib",
        "//source/common/network:resolver_lib",
        "//source/common/network:utility_lib",
        "//source/common/protobuf:utility_lib",
        "//source/common/router:shadow_writer_lib",
        "//source/common/tcp:conn_pool_lib",
        "//source/common/upstream:priority_conn_pool_map_impl_lib",
        "//source/common/upstream:upstream_lib",
        "@envoy_api//envoy/admin/v2alpha:config_dump_cc",
        "@envoy_api//envoy/api/v2/core:base_cc",
    ],
)

envoy_cc_library(
    name = "conn_pool_map",
    hdrs = ["conn_pool_map.h"],
    deps = [
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/upstream:resource_manager_interface",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:debug_recursion_checker_lib",
    ],
)

envoy_cc_library(
    name = "conn_pool_map_impl_lib",
    hdrs = ["conn_pool_map_impl.h"],
    deps = [
        ":conn_pool_map",
    ],
)

envoy_cc_library(
    name = "priority_conn_pool_map",
    hdrs = ["priority_conn_pool_map.h"],
    deps = [
        ":conn_pool_map",
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/upstream:resource_manager_interface",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:debug_recursion_checker_lib",
    ],
)

envoy_cc_library(
    name = "priority_conn_pool_map_impl_lib",
    hdrs = ["priority_conn_pool_map_impl.h"],
    deps = [
        ":conn_pool_map_impl_lib",
        ":priority_conn_pool_map",
    ],
)

envoy_cc_library(
    name = "edf_scheduler_lib",
    hdrs = ["edf_scheduler.h"],
    deps = ["//source/common/common:assert_lib"],
)

envoy_cc_library(
    name = "health_checker_base_lib",
    srcs = ["health_checker_base_impl.cc"],
    hdrs = ["health_checker_base_impl.h"],
    deps = [
        "//include/envoy/upstream:health_checker_interface",
        "//source/common/router:router_lib",
        "@envoy_api//envoy/api/v2/core:health_check_cc",
        "@envoy_api//envoy/data/core/v2alpha:health_check_event_cc",
    ],
)

envoy_cc_library(
    name = "health_checker_lib",
    srcs = ["health_checker_impl.cc"],
    hdrs = ["health_checker_impl.h"],
    external_deps = [
        "grpc_health_proto",
    ],
    deps = [
        ":health_checker_base_lib",
        # TODO(dio): Remove dependency to server.
        "//include/envoy/server:health_checker_config_interface",
        "//source/common/grpc:codec_lib",
        "//source/common/http:codec_client_lib",
        "//source/common/upstream:host_utility_lib",
        # TODO(dio): Remove dependency to health checkers extension when redis_health_check is removed.
        "//source/extensions/health_checkers:well_known_names",
    ],
)

envoy_cc_library(
    name = "host_utility_lib",
    srcs = ["host_utility.cc"],
    hdrs = ["host_utility.h"],
    deps = ["//include/envoy/upstream:upstream_interface"],
)

envoy_cc_library(
    name = "load_balancer_lib",
    srcs = ["load_balancer_impl.cc"],
    hdrs = ["load_balancer_impl.h"],
    deps = [
        ":edf_scheduler_lib",
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/stats:stats_interface",
        "//include/envoy/upstream:load_balancer_interface",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:assert_lib",
        "//source/common/common:stack_array",
        "//source/common/protobuf:utility_lib",
    ],
)

envoy_cc_library(
    name = "load_stats_reporter_lib",
    srcs = ["load_stats_reporter.cc"],
    hdrs = ["load_stats_reporter.h"],
    deps = [
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/stats:stats_macros",
        "//include/envoy/upstream:cluster_manager_interface",
        "//source/common/common:minimal_logger_lib",
        "//source/common/grpc:async_client_lib",
        "@envoy_api//envoy/service/load_stats/v2:lrs_cc",
    ],
)

envoy_cc_library(
    name = "health_discovery_service_lib",
    srcs = ["health_discovery_service.cc"],
    hdrs = ["health_discovery_service.h"],
    deps = [
        ":health_checker_lib",
        ":upstream_includes",
        "//include/envoy/api:api_interface",
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/server:transport_socket_config_interface",
        "//include/envoy/ssl:context_manager_interface",
        "//include/envoy/stats:stats_macros",
        "//include/envoy/upstream:cluster_manager_interface",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:backoff_lib",
        "//source/common/common:minimal_logger_lib",
        "//source/common/config:utility_lib",
        "//source/common/grpc:async_client_lib",
        "//source/common/network:resolver_lib",
        "//source/extensions/transport_sockets:well_known_names",
        "//source/server:transport_socket_config_lib",
        "@envoy_api//envoy/service/discovery/v2:hds_cc",
    ],
)

envoy_cc_library(
    name = "logical_dns_cluster_lib",
    srcs = ["logical_dns_cluster.cc"],
    hdrs = ["logical_dns_cluster.h"],
    deps = [
        ":cluster_factory_lib",
        ":upstream_includes",
        "//include/envoy/thread_local:thread_local_interface",
        "//include/envoy/upstream:cluster_factory_interface",
        "//source/common/common:empty_string",
        "//source/common/config:utility_lib",
        "//source/common/network:address_lib",
        "//source/common/network:utility_lib",
        "//source/common/protobuf",
        "//source/common/protobuf:utility_lib",
        "//source/extensions/clusters:well_known_names",
    ],
)

envoy_cc_library(
    name = "original_dst_cluster_lib",
    srcs = ["original_dst_cluster.cc"],
    hdrs = ["original_dst_cluster.h"],
    deps = [
        ":cluster_factory_lib",
        ":upstream_includes",
        "//include/envoy/secret:secret_manager_interface",
        "//include/envoy/upstream:cluster_factory_interface",
        "//source/common/common:empty_string",
        "//source/common/network:address_lib",
        "//source/common/network:utility_lib",
        "//source/extensions/clusters:well_known_names",
    ],
)

envoy_cc_library(
    name = "outlier_detection_lib",
    srcs = ["outlier_detection_impl.cc"],
    hdrs = ["outlier_detection_impl.h"],
    deps = [
        "//include/envoy/access_log:access_log_interface",
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/event:timer_interface",
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/upstream:outlier_detection_interface",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:assert_lib",
        "//source/common/common:utility_lib",
        "//source/common/http:codes_lib",
        "//source/common/protobuf",
        "@envoy_api//envoy/api/v2:cds_cc",
        "@envoy_api//envoy/data/cluster/v2alpha:outlier_detection_event_cc",
    ],
)

envoy_cc_library(
    name = "resource_manager_lib",
    hdrs = ["resource_manager_impl.h"],
    deps = [
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/upstream:resource_manager_interface",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:assert_lib",
    ],
)

envoy_cc_library(
    name = "thread_aware_lb_lib",
    srcs = ["thread_aware_lb_impl.cc"],
    hdrs = ["thread_aware_lb_impl.h"],
    external_deps = ["abseil_synchronization"],
    deps = [
        ":load_balancer_lib",
    ],
)

envoy_cc_library(
    name = "maglev_lb_lib",
    srcs = ["maglev_lb.cc"],
    hdrs = ["maglev_lb.h"],
    deps = [
        ":thread_aware_lb_lib",
        ":upstream_lib",
    ],
)

envoy_cc_library(
    name = "ring_hash_lb_lib",
    srcs = ["ring_hash_lb.cc"],
    hdrs = ["ring_hash_lb.h"],
    deps = [
        ":thread_aware_lb_lib",
        "//source/common/common:minimal_logger_lib",
    ],
)

envoy_cc_library(
    name = "eds_lib",
    srcs = ["eds.cc"],
    hdrs = ["eds.h"],
    deps = [
        ":cluster_factory_lib",
        ":upstream_includes",
        "//include/envoy/config:grpc_mux_interface",
        "//include/envoy/config:subscription_interface",
        "//include/envoy/local_info:local_info_interface",
        "//include/envoy/secret:secret_manager_interface",
        "//include/envoy/upstream:cluster_factory_interface",
        "//include/envoy/upstream:locality_lib",
        "//source/common/config:metadata_lib",
        "//source/common/config:subscription_factory_lib",
        "//source/common/config:utility_lib",
        "//source/common/network:address_lib",
        "//source/common/network:resolver_lib",
        "//source/common/network:utility_lib",
        "//source/common/protobuf:utility_lib",
        "//source/extensions/clusters:well_known_names",
        "@envoy_api//envoy/api/v2:eds_cc",
        "@envoy_api//envoy/api/v2/core:base_cc",
        "@envoy_api//envoy/api/v2/endpoint:endpoint_cc",
    ],
)

envoy_cc_library(
    name = "subset_lb_lib",
    srcs = ["subset_lb.cc"],
    hdrs = ["subset_lb.h"],
    deps = [
        ":load_balancer_lib",
        ":maglev_lb_lib",
        ":ring_hash_lb_lib",
        ":upstream_lib",
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/upstream:load_balancer_interface",
        "//source/common/common:assert_lib",
        "//source/common/common:minimal_logger_lib",
        "//source/common/config:metadata_lib",
        "//source/common/protobuf",
        "//source/common/protobuf:utility_lib",
    ],
)

envoy_cc_library(
    name = "upstream_lib",
    srcs = ["upstream_impl.cc"],
    deps = [
        ":eds_lib",
        ":health_checker_lib",
        ":logical_dns_cluster_lib",
        ":original_dst_cluster_lib",
        ":upstream_includes",
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/event:timer_interface",
        "//include/envoy/network:dns_interface",
        "//include/envoy/network:listen_socket_interface",
        "//include/envoy/ssl:context_interface",
        "//include/envoy/upstream:health_checker_interface",
        "//source/common/common:enum_to_int",
        "//source/common/common:utility_lib",
        "//source/common/config:protocol_json_lib",
        "//source/common/config:tls_context_json_lib",
        "//source/common/http:utility_lib",
        "//source/common/network:address_lib",
        "//source/common/network:resolver_lib",
        "//source/common/network:socket_option_factory_lib",
        "//source/common/network:utility_lib",
        "//source/common/protobuf",
        "//source/common/protobuf:utility_lib",
        "//source/extensions/transport_sockets:well_known_names",
        "//source/server:transport_socket_config_lib",
        "@envoy_api//envoy/api/v2/core:base_cc",
    ],
)

envoy_cc_library(
    name = "upstream_includes",
    hdrs = ["upstream_impl.h"],
    external_deps = ["abseil_synchronization"],
    deps = [
        ":load_balancer_lib",
        ":outlier_detection_lib",
        ":resource_manager_lib",
        "//include/envoy/event:timer_interface",
        "//include/envoy/local_info:local_info_interface",
        "//include/envoy/network:dns_interface",
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/server:transport_socket_config_interface",
        "//include/envoy/ssl:context_manager_interface",
        "//include/envoy/thread_local:thread_local_interface",
        "//include/envoy/upstream:cluster_manager_interface",
        "//include/envoy/upstream:health_checker_interface",
        "//include/envoy/upstream:load_balancer_interface",
        "//include/envoy/upstream:locality_lib",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:callback_impl_lib",
        "//source/common/common:enum_to_int",
        "//source/common/common:minimal_logger_lib",
        "//source/common/config:metadata_lib",
        "//source/common/config:well_known_names",
        "//source/common/stats:isolated_store_lib",
        "//source/common/stats:stats_lib",
        "//source/server:init_manager_lib",
        "//source/server:transport_socket_config_lib",
        "@envoy_api//envoy/api/v2/core:base_cc",
        "@envoy_api//envoy/api/v2/endpoint:endpoint_cc",
    ],
)

envoy_cc_library(
    name = "cluster_factory_lib",
    srcs = ["cluster_factory_impl.cc"],
    deps = [
        ":cluster_factory_includes",
        ":health_checker_lib",
        ":upstream_includes",
        "//include/envoy/event:dispatcher_interface",
        "//include/envoy/event:timer_interface",
        "//include/envoy/network:dns_interface",
        "//include/envoy/network:listen_socket_interface",
        "//include/envoy/ssl:context_interface",
        "//include/envoy/upstream:health_checker_interface",
        "//source/common/common:enum_to_int",
        "//source/common/common:utility_lib",
        "//source/common/config:protocol_json_lib",
        "//source/common/config:tls_context_json_lib",
        "//source/common/http:utility_lib",
        "//source/common/network:address_lib",
        "//source/common/network:resolver_lib",
        "//source/common/network:socket_option_factory_lib",
        "//source/common/network:utility_lib",
        "//source/common/protobuf",
        "//source/common/protobuf:utility_lib",
        "//source/extensions/clusters:well_known_names",
        "//source/extensions/transport_sockets:well_known_names",
        "//source/server:transport_socket_config_lib",
        "@envoy_api//envoy/api/v2/core:base_cc",
    ],
)

envoy_cc_library(
    name = "cluster_factory_includes",
    hdrs = ["cluster_factory_impl.h"],
    deps = [
        ":load_balancer_lib",
        ":outlier_detection_lib",
        ":resource_manager_lib",
        ":upstream_includes",
        "//include/envoy/event:timer_interface",
        "//include/envoy/local_info:local_info_interface",
        "//include/envoy/network:dns_interface",
        "//include/envoy/runtime:runtime_interface",
        "//include/envoy/server:transport_socket_config_interface",
        "//include/envoy/ssl:context_manager_interface",
        "//include/envoy/thread_local:thread_local_interface",
        "//include/envoy/upstream:cluster_factory_interface",
        "//include/envoy/upstream:cluster_manager_interface",
        "//include/envoy/upstream:health_checker_interface",
        "//include/envoy/upstream:load_balancer_interface",
        "//include/envoy/upstream:locality_lib",
        "//include/envoy/upstream:upstream_interface",
        "//source/common/common:callback_impl_lib",
        "//source/common/common:enum_to_int",
        "//source/common/common:minimal_logger_lib",
        "//source/common/config:metadata_lib",
        "//source/common/config:utility_lib",
        "//source/common/config:well_known_names",
        "//source/common/stats:isolated_store_lib",
        "//source/common/stats:stats_lib",
        "//source/extensions/clusters:well_known_names",
        "//source/server:init_manager_lib",
        "//source/server:transport_socket_config_lib",
        "@envoy_api//envoy/api/v2/core:base_cc",
        "@envoy_api//envoy/api/v2/endpoint:endpoint_cc",
    ],
)
