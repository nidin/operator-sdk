## Unreleased

### Added

- Document new compile-time dependency `mercurial` in user-facing documentation. ([#1683](https://github.com/operator-framework/operator-sdk/pull/1683))

### Changed

- **Breaking change:** CSV config field `role-path` is now `role-paths` and takes a list of strings. Users can now specify multiple `Role` and `ClusterRole` manifests using `role-paths`. ([#1704](https://github.com/operator-framework/operator-sdk/pull/1704))

### Deprecated

### Removed

### Bug Fixes

## v0.9.0

### Added

- Adds support for building OCI images with [podman](https://podman.io/), e.g. `operator-sdk build --image-builder=podman`. ([#1488](https://github.com/operator-framework/operator-sdk/pull/1488))
- New option for [`operator-sdk up local --enable-delve`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#up), which can be used to start the operator in remote debug mode with the [delve](https://github.com/go-delve/delve) debugger listening on port 2345. ([#1422](https://github.com/operator-framework/operator-sdk/pull/1422))
- Enables controller-runtime metrics in Helm operator projects. ([#1482](https://github.com/operator-framework/operator-sdk/pull/1482))
- New flags `--vendor` and `--skip-validation` for [`operator-sdk new`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#new) that direct the SDK to initialize a new project with a `vendor/` directory, and without validating project dependencies. `vendor/` is not written by default. ([#1519](https://github.com/operator-framework/operator-sdk/pull/1519))
- Generating and serving info metrics about each custom resource. By default these metrics are exposed on port 8686. ([#1277](https://github.com/operator-framework/operator-sdk/pull/1277))
- Scaffold a `pkg/apis/<group>/group.go` package file to avoid `go/build` errors when running Kubernetes code generators. ([#1401](https://github.com/operator-framework/operator-sdk/pull/1401))
- Adds a new extra variable containing the unmodified CR spec for ansible based operators. [#1563](https://github.com/operator-framework/operator-sdk/pull/1563)
- New flag `--repo` for subcommands [`new`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#new) and [`migrate`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#migrate) specifies the repository path to be used in Go source files generated by the SDK. This flag can only be used with [Go modules](https://github.com/golang/go/wiki/Modules). ([#1475](https://github.com/operator-framework/operator-sdk/pull/1475))
- Adds `--go-build-args` flag to `operator-sdk build` for providing additional Go build arguments. ([#1582](https://github.com/operator-framework/operator-sdk/pull/1582))
- New flags `--csv-channel` and `--default-channel` for subcommand [`gen-csv`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#gen-csv) that add channels to and update the [package manifest](https://github.com/operator-framework/operator-registry/#manifest-format) in `deploy/olm-catalog/<operator-name>` when generating a new CSV or updating an existing one. ([#1364](https://github.com/operator-framework/operator-sdk/pull/1364))
- Adds `go.mod` and `go.sum` to switch from `dep` to [Go modules](https://github.com/golang/go/wiki/Modules) to manage dependencies for the SDK project itself. ([#1566](https://github.com/operator-framework/operator-sdk/pull/1566))
- New flag `--operator-name` for [`operator-sdk olm-catalog gen-csv`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#gen-csv) to specify the operator name, ex. `memcached-operator`, to use in CSV generation. The project's name is used (old behavior) if `--operator-name` is not set. ([#1571](https://github.com/operator-framework/operator-sdk/pull/1571))

### Changed

- Upgrade the version of the dependency [controller-runtime](https://github.com/kubernetes-sigs/controller-runtime) from `v0.1.10` to `v0.1.12`. ([#1612](https://github.com/operator-framework/operator-sdk/pull/1612))
- Remove TypeMeta declaration from the implementation of the objects ([#1462](https://github.com/operator-framework/operator-sdk/pull/1462/))
- Relaxed API version format check when parsing `pkg/apis` in code generators. API dir structures can now be of the format `pkg/apis/<group>/<anything>`, where `<anything>` was previously required to be in the Kubernetes version format, ex. `v1alpha1`. ([#1525](https://github.com/operator-framework/operator-sdk/pull/1525))
- The SDK and operator projects will work outside of `$GOPATH/src` when using [Go modules](https://github.com/golang/go/wiki/Modules). ([#1475](https://github.com/operator-framework/operator-sdk/pull/1475))
-  `CreateMetricsService()` function from the metrics package accepts a REST config (\*rest.Config) and an array of ServicePort objects ([]v1.ServicePort) as input to create Service metrics. `CRPortName` constant is added to describe the string of custom resource port name. ([#1560](https://github.com/operator-framework/operator-sdk/pull/1560) and [#1626](https://github.com/operator-framework/operator-sdk/pull/1626))
- Changed the flag `--skip-git-init` to [`--git-init`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#new). This changes the default behavior of `operator-sdk new` to not initialize the new project directory as a git repository with `git init`. This behavior is now opt-in with `--git-init`. ([#1588](https://github.com/operator-framework/operator-sdk/pull/1588))
- `operator-sdk new` will no longer create the initial commit for a new project, even with `--git-init=true`. ([#1588](https://github.com/operator-framework/operator-sdk/pull/1588))
- When errors occur setting up the Kubernetes client for RBAC role generation, `operator-sdk new --type=helm` now falls back to a default RBAC role instead of failing. ([#1627](https://github.com/operator-framework/operator-sdk/pull/1627))

### Deprecated

### Removed

- The SDK no longer depends on a `vendor/` directory to manage dependencies *only if* using [Go modules](https://github.com/golang/go/wiki/Modules). The SDK and operator projects will only use vendoring if using `dep`, or modules and a `vendor/` dir is present. ([#1519](https://github.com/operator-framework/operator-sdk/pull/1519))
- **Breaking change:** `ExposeMetricsPort` is removed and replaced with `CreateMetricsService()` function. `PrometheusPortName` constant is replaced with `OperatorPortName`. ([#1560](https://github.com/operator-framework/operator-sdk/pull/1560))
- Removes `Gopkg.toml` and `Gopkg.lock` to drop the use of `dep` in favor of [Go modules](https://github.com/golang/go/wiki/Modules) to manage dependencies for the SDK project itself. ([#1566](https://github.com/operator-framework/operator-sdk/pull/1566))

### Bug Fixes

- Generated CSV's that include a deployment install strategy will be checked for a reference to `metadata.annotations['olm.targetNamespaces']`, and if one is not found a reference will be added to the `WATCH_NAMESPACE` env var for all containers in the deployment. This is a bug because any other value that references the CSV's namespace is incorrect. ([#1396](https://github.com/operator-framework/operator-sdk/pull/1396))
- Build `-trimpath` was not being respected. `$GOPATH` was not expanding because `exec.Cmd{}` is not executed in a shell environment. ([#1535](https://github.com/operator-framework/operator-sdk/pull/1535))
- Running the [scorecard](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#up) with `--olm-deployed` will now only use the first CR set in either the `cr-manifest` config option or the CSV's `metadata.annotations['alm-examples']` as was intended, and access manifests correctly from the config. ([#1565](https://github.com/operator-framework/operator-sdk/pull/1565))
- Use the correct domain names when generating CRD's instead that of the first CRD to be parsed. ([#1636](https://github.com/operator-framework/operator-sdk/pull/1636))

## v0.8.1

### Bug Fixes

- Fixes a regression that causes Helm RBAC generation to contain an empty custom ruleset when the chart's default manifest contains only namespaced resources. ([#1456](https://github.com/operator-framework/operator-sdk/pull/1456))
- Fixes an issue that causes Helm RBAC generation to fail when creating new operators with a Kubernetes context configured to connect to an OpenShift cluster. ([#1461](https://github.com/operator-framework/operator-sdk/pull/1461))

## v0.8.0

### Added

- New option for [`operator-sdk build --image-builder`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#build), which can be used to specify which image builder to use. Adds support for [buildah](https://github.com/containers/buildah/). ([#1311](https://github.com/operator-framework/operator-sdk/pull/1311))
- Manager is now configured with a new `DynamicRESTMapper`, which accounts for the fact that the default `RESTMapper`, which only checks resource types at startup, can't handle the case of first creating a CRD and then an instance of that CRD. ([#1329](https://github.com/operator-framework/operator-sdk/pull/1329))
- Unify CLI debug logging under a global `--verbose` flag ([#1361](https://github.com/operator-framework/operator-sdk/pull/1361))
- [Go module](https://github.com/golang/go/wiki/Modules) support by default for new Go operators and during Ansible and Helm operator migration. The dependency manager used for a new operator can be explicitly specified for new operators through the `--dep-manager` flag, available in [`operator-sdk new`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#new) and [`operator-sdk migrate`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#migrate). `dep` is still available through `--dep-manager=dep`. ([#1001](https://github.com/operator-framework/operator-sdk/pull/1001))
- New optional flag `--custom-api-import` for [`operator-sdk add controller`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#controller) to specify that the new controller reconciles a built-in or external Kubernetes API, and what import path and identifier it should have. ([#1344](https://github.com/operator-framework/operator-sdk/pull/1344))
- Operator Scorecard plugin support ([#1379](https://github.com/operator-framework/operator-sdk/pull/1379)). Documentation for scorecard plugins can be found in the main scorecard doc: [doc/test-framework/scorecard.md](./doc/test-framework/scorecard.md)

### Changed

- When Helm operator projects are created, the SDK now generates RBAC rules in `deploy/role.yaml` based on the chart's default manifest. ([#1188](https://github.com/operator-framework/operator-sdk/pull/1188))
- When debug level is 3 or higher, we will set the klog verbosity to that level. ([#1322](https://github.com/operator-framework/operator-sdk/pull/1322))
- Relaxed requirements for groups in new project API's. Groups passed to [`operator-sdk add api`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#api)'s `--api-version` flag can now have no subdomains, ex `core/v1`. See ([#1191](https://github.com/operator-framework/operator-sdk/issues/1191)) for discussion. ([#1313](https://github.com/operator-framework/operator-sdk/pull/1313))
- Renamed `--docker-build-args` option to `--image-build-args` option for `build` subcommand, because this option can now be shared with other image build tools than docker when `--image-builder` option is specified. ([#1311](https://github.com/operator-framework/operator-sdk/pull/1311))
- Reduces Helm release information in CR status to only the release name and manifest and moves it from `status.conditions` to a new top-level `deployedRelease` field. ([#1309](https://github.com/operator-framework/operator-sdk/pull/1309))
  - **WARNING**: Users with active CRs and releases who are upgrading their helm-based operator should upgrade to one based on v0.7.0 before upgrading further. Helm operators based on v0.8.0+ will not seamlessly transition release state to the persistent backend, and will instead uninstall and reinstall all managed releases.
- Go operator CRDs are overwritten when being regenerated by [`operator-sdk generate openapi`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#openapi). Users can now rely on `+kubebuilder` annotations in their API code, which provide access to most OpenAPIv3 [validation properties](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md#schema-object) (the full set will be supported in the near future, see [this PR](https://github.com/kubernetes-sigs/controller-tools/pull/190)) and [other CRD fields](https://book.kubebuilder.io/beyond_basics/generating_crd.html). ([#1278](https://github.com/operator-framework/operator-sdk/pull/1278))
- Use `registry.access.redhat.com/ubi7/ubi-minimal:latest` base image for the Go and Helm operators and scorecard proxy ([#1376](https://github.com/operator-framework/operator-sdk/pull/1376))
- Allow "Owned CRDs Have Resources Listed" scorecard test to pass if the resources section exists

### Deprecated

### Removed

- The SDK will no longer run `defaulter-gen` on running `operator-sdk generate k8s`. Defaulting for CRDs should be handled with mutating admission webhooks. ([#1288](https://github.com/operator-framework/operator-sdk/pull/1288))
- The `--version` flag was removed. Users should use the `operator-sdk version` command. ([#1444](https://github.com/operator-framework/operator-sdk/pull/1444))
- **Breaking Change**: The `test cluster` subcommand and the corresponding `--enable-tests` flag for the `build` subcommand have been removed ([#1414](https://github.com/operator-framework/operator-sdk/pull/1414))
- **Breaking Change**: The `--cluster-scoped` flag for `operator-sdk new` has been removed so it won't scaffold a cluster-scoped operator. Read the [operator scope](https://github.com/operator-framework/operator-sdk/blob/master/doc/operator-scope.md) documentation on the changes needed to run a cluster-scoped operator. ([#1434](https://github.com/operator-framework/operator-sdk/pull/1434))

### Bug Fixes

- In Helm-based operators, when a custom resource with a failing release is reverted back to a working state, the `ReleaseFailed` condition is now correctly removed. ([#1321](https://github.com/operator-framework/operator-sdk/pull/1321))
- [`operator-sdk generate openapi`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#openapi) no longer overwrites CRD values derived from `+kubebuilder` annotations in Go API code. See issues ([#1212](https://github.com/operator-framework/operator-sdk/issues/1212)) and ([#1323](https://github.com/operator-framework/operator-sdk/issues/1323)) for discussion. ([#1278](https://github.com/operator-framework/operator-sdk/pull/1278))
- Running [`operator-sdk gen-csv`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#gen-csv) on operators that do not have a CRDs directory, ex. `deploy/crds`, or do not have any [owned CRDs](https://github.com/operator-framework/operator-lifecycle-manager/blob/master/Documentation/design/building-your-csv.md#your-custom-resource-definitions), will not generate a "deploy/crds not found" error.

## v0.7.1

### Bug Fixes

- Pin dependency versions in Ansible build and test framework Dockerfiles to fix broken build and test framework images. ([#1348](https://github.com/operator-framework/operator-sdk/pull/1348))
- In Helm-based operators, when a custom resource with a failing release is reverted back to a working state, the `ReleaseFailed` condition is now correctly removed. ([#1321](https://github.com/operator-framework/operator-sdk/pull/1321))

## v0.7.0

### Added

- New optional flag `--header-file` for commands [`operator-sdk generate k8s`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#k8s) and [`operator-sdk add api`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#api) to supply a boilerplate header file for generated code. ([#1239](https://github.com/operator-framework/operator-sdk/pull/1239))
- JSON output support for `operator-sdk scorecard` subcommand ([#1228](https://github.com/operator-framework/operator-sdk/pull/1228))

### Changed

- Updated the helm-operator to store release state in kubernetes secrets in the same namespace of the custom resource that defines the release. ([#1102](https://github.com/operator-framework/operator-sdk/pull/1102))
  - **WARNING**: Users with active CRs and releases who are upgrading their helm-based operator should not skip this version. Future versions will not seamlessly transition release state to the persistent backend, and will instead uninstall and reinstall all managed releases.
- Change `namespace-manifest` flag in scorecard subcommand to `namespaced-manifest` to match other subcommands
- Subcommands of [`operator-sdk generate`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#generate) are now verbose by default. ([#1271](https://github.com/operator-framework/operator-sdk/pull/1271))
- [`operator-sdk olm-catalog gen-csv`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#gen-csv) parses Custom Resource manifests from `deploy/crds` or a custom path specified in `csv-config.yaml`, encodes them in a JSON array, and sets the CSV's [`metadata.annotations.alm-examples`](https://github.com/operator-framework/operator-lifecycle-manager/blob/master/Documentation/design/building-your-csv.md#crd-templates) field to that JSON. ([#1116](https://github.com/operator-framework/operator-sdk/pull/1116))

### Bug Fixes

- Fixed an issue that caused `operator-sdk new --type=helm` to fail for charts that have template files in nested template directories. ([#1235](https://github.com/operator-framework/operator-sdk/pull/1235))
- Fix bug in the YAML scanner used by `operator-sdk test` and `operator-sdk scorecard` that could result in a panic if a manifest file started with `---` ([#1258](https://github.com/operator-framework/operator-sdk/pull/1258))

## v0.6.0

### Added

- New flags for [`operator-sdk new --type=helm`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#new), which can be used to populate the project with an existing chart. ([#949](https://github.com/operator-framework/operator-sdk/pull/949))
- Command [`operator-sdk olm-catalog`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#olm-catalog) flag `--update-crds` optionally copies CRD's from `deploy/crds` when creating a new CSV or updating an existing CSV, and `--from-version` uses another versioned CSV manifest as a base for a new CSV version. ([#1016](https://github.com/operator-framework/operator-sdk/pull/1016))
- New flag `--olm-deployed` to direct the [`scorecard`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#scorecard) command to only use the CSV at `--csv-path` for manifest data, except for those provided to `--cr-manifest`. ([#1044](https://github.com/operator-framework/operator-sdk/pull/1044))
- Command [`version`](https://github.com/operator-framework/operator-sdk/pull/1171) prints the version of operator-sdk. ([#1171](https://github.com/operator-framework/operator-sdk/pull/1171))

### Changed

- Changed the Go, Helm, and Scorecard base images to `registry.access.redhat.com/ubi7-dev-preview/ubi-minimal:7.6` ([#1142](https://github.com/operator-framework/operator-sdk/pull/1142))
- CSV manifest are now versioned according to the `operator-registry` [manifest format](https://github.com/operator-framework/operator-registry#manifest-format). See issue [#900](https://github.com/operator-framework/operator-sdk/issues/900) for more details. ([#1016](https://github.com/operator-framework/operator-sdk/pull/1016))
- Unexported `CleanupNoT` function from `pkg/test`, as it is only intended to be used internally ([#1167](https://github.com/operator-framework/operator-sdk/pull/1167))

### Bug Fixes

- Fix issue where running `operator-sdk test local --up-local` would sometimes leave a running process in the background after exit ([#1089](https://github.com/operator-framework/operator-sdk/pull/1020))

## v0.5.0

### Added

- Updated the Kubernetes dependencies to `1.13.1` ([#1020](https://github.com/operator-framework/operator-sdk/pull/1020))
- Updated the controller-runtime version to `v0.1.10`. See the [controller-runtime `v0.1.10` release notes](https://github.com/kubernetes-sigs/controller-runtime/releases/tag/v0.1.10) for new features and bug fixes. ([#1020](https://github.com/operator-framework/operator-sdk/pull/1020))
- By default the controller-runtime metrics are exposed on port 8383. This is done as part of the scaffold in the main.go file, the port can be adjusted by modifying the `metricsPort` variable. [#786](https://github.com/operator-framework/operator-sdk/pull/786)
- A new command [`operator-sdk olm-catalog`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#olm-catalog) to be used as a parent for SDK subcommands generating code related to Operator Lifecycle Manager (OLM) Catalog integration, and subcommand [`operator-sdk olm-catalog gen-csv`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#gen-csv) which generates a Cluster Service Version for an operator so the OLM can deploy the operator in a cluster. ([#673](https://github.com/operator-framework/operator-sdk/pull/673))
- Helm-based operators have leader election turned on by default. When upgrading, add environment variable `POD_NAME` to your operator's Deployment using the Kubernetes downward API. To see an example, run `operator-sdk new --type=helm ...` and see file `deploy/operator.yaml`. [#1000](https://github.com/operator-framework/operator-sdk/pull/1000)
- A new command [`operator-sdk generate openapi`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#openapi) which generates OpenAPIv3 validation specs in Go and in CRD manifests as YAML. ([#869](https://github.com/operator-framework/operator-sdk/pull/869))
- The `operator-sdk add api` command now generates OpenAPIv3 validation specs in Go for that API, and in all CRD manifests as YAML.

### Changed

- In new Helm operator projects, the scaffolded CR `spec` field now contains the default values.yaml from the generated chart. ([#967](https://github.com/operator-framework/operator-sdk/pull/967))

### Deprecated

### Removed

### Bug Fixes

## v0.4.1

### Bug Fixes

- Make `up local` subcommand respect `KUBECONFIG` env var ([#996](https://github.com/operator-framework/operator-sdk/pull/996))
- Make `up local` subcommand use default namespace set in kubeconfig instead of hardcoded `default` and also add ability to watch all namespaces for ansible and helm type operators ([#996](https://github.com/operator-framework/operator-sdk/pull/996))
- Added k8s_status modules back to generation ([#972](https://github.com/operator-framework/operator-sdk/pull/972))
- Update checks for gvk registration to cover all cases for ansible ([#973](https://github.com/operator-framework/operator-sdk/pull/973) & [#1019](https://github.com/operator-framework/operator-sdk/pull/1019))
- Update reconciler for ansible and helm to use the cache rather than the API client. ([#1022](https://github.com/operator-framework/operator-sdk/pull/1022) & [#1048](https://github.com/operator-framework/operator-sdk/pull/1048) & [#1054](https://github.com/operator-framework/operator-sdk/pull/1054))
- Update reconciler to will update the status everytime for ansible ([#1066](https://github.com/operator-framework/operator-sdk/pull/1066))
- Update ansible proxy to recover dependent watches when pod is killed ([#1067](https://github.com/operator-framework/operator-sdk/pull/1067))
- Update ansible proxy to handle watching cluster scoped dependent watches ([#1031](https://github.com/operator-framework/operator-sdk/pull/1031))

## v0.4.0

### Added

- A new command [`operator-sdk migrate`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#migrate) which adds a main.go source file and any associated source files for an operator that is not of the "go" type. ([#887](https://github.com/operator-framework/operator-sdk/pull/887) and [#897](https://github.com/operator-framework/operator-sdk/pull/897))
- New commands [`operator-sdk run ansible`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#ansible) and [`operator-sdk run helm`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#helm) which run the SDK as ansible  and helm operator processes, respectively. These are intended to be used when running in a Pod inside a cluster. Developers wanting to run their operator locally should continue to use `up local`. ([#887](https://github.com/operator-framework/operator-sdk/pull/887) and [#897](https://github.com/operator-framework/operator-sdk/pull/897))
- Ansible operator proxy added the cache handler which allows the get requests to use the operators cache. [#760](https://github.com/operator-framework/operator-sdk/pull/760)
- Ansible operator proxy added ability to dynamically watch dependent resource that were created by ansible operator. [#857](https://github.com/operator-framework/operator-sdk/pull/857)
- Ansible-based operators have leader election turned on by default. When upgrading, add environment variable `POD_NAME` to your operator's Deployment using the Kubernetes downward API. To see an example, run `operator-sdk new --type=ansible ...` and see file `deploy/operator.yaml`.
- A new command [`operator-sdk scorecard`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#scorecard) which runs a series of generic tests on operators to ensure that an operator follows best practices. For more information, see the [`Scorecard Documentation`](doc/test-framework/scorecard.md)

### Changed

- The official images for the Ansible and Helm operators have moved! Travis now builds, tags, and pushes operator base images during CI ([#832](https://github.com/operator-framework/operator-sdk/pull/832)).
  - [quay.io/operator-framework/ansible-operator](https://quay.io/repository/operator-framework/ansible-operator)
  - [quay.io/operator-framework/helm-operator](https://quay.io/repository/operator-framework/helm-operator)

### Bug Fixes

- Fixes deadlocks during operator deployment rollouts, which were caused by operator pods requiring a leader election lock to become ready ([#932](https://github.com/operator-framework/operator-sdk/pull/932))

## v0.3.0

### Added

- Helm type operator generation support ([#776](https://github.com/operator-framework/operator-sdk/pull/776))

### Changed

- The SDK's Kubernetes Golang dependency versions/revisions have been updated from `v1.11.2` to `v1.12.3`. ([#807](https://github.com/operator-framework/operator-sdk/pull/807))
- The controller-runtime version has been updated from `v0.1.4` to `v0.1.8`. See the `v0.1.8` [release notes](https://github.com/kubernetes-sigs/controller-runtime/releases/tag/v0.1.8) for details.
- The SDK now generates the CRD with the status subresource enabled by default. See the [client doc](https://github.com/operator-framework/operator-sdk/blob/master/doc/user/client.md#updating-status-subresource) on how to update the status subresource. ([#787](https://github.com/operator-framework/operator-sdk/pull/787))

### Deprecated

### Removed

### Bug Fixes

## v0.2.1

### Bug Fixes

- Pin controller-runtime version to v0.1.4 to fix dependency issues and pin ansible idna package to version 2.7 ([#831](https://github.com/operator-framework/operator-sdk/pull/831))

## v0.2.0

### Changed

- The SDK now uses logr as the default logger to unify the logging output with the controller-runtime logs. Users can still use a logger of their own choice. See the [logging doc](https://github.com/operator-framework/operator-sdk/blob/master/doc/user/logging.md) on how the SDK initializes and uses logr.
- Ansible Operator CR status better aligns with [conventions](https://github.com/kubernetes/community/blob/master/contributors/devel/api-conventions.md#typical-status-properties). ([#639](https://github.com/operator-framework/operator-sdk/pull/639))

### Added

- A new command [`operator-sdk print-deps`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#print-deps) which prints Golang packages and versions expected by the current Operator SDK version. Supplying `--as-file` prints packages and versions in Gopkg.toml format. ([#772](https://github.com/operator-framework/operator-sdk/pull/772))
- Add [`cluster-scoped`](https://github.com/operator-framework/operator-sdk/blob/master/doc/user-guide.md#operator-scope) flag to `operator-sdk new` command ([#747](https://github.com/operator-framework/operator-sdk/pull/747))
- Add [`up-local`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#flags-9) flag to `test local` subcommand ([#781](https://github.com/operator-framework/operator-sdk/pull/781))
- Add [`no-setup`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#flags-9) flag to `test local` subcommand ([#770](https://github.com/operator-framework/operator-sdk/pull/770))
- Add [`image`](https://github.com/operator-framework/operator-sdk/blob/master/doc/sdk-cli-reference.md#flags-9) flag to `test local` subcommand ([#768](https://github.com/operator-framework/operator-sdk/pull/768))
- Ansible Operator log output includes much more information for troubleshooting ansible errors. ([#713](https://github.com/operator-framework/operator-sdk/pull/713))
- Ansible Operator periodic reconciliation can be disabled ([#739](https://github.com/operator-framework/operator-sdk/pull/739))

### Bug fixes

- Make operator-sdk command work with composed GOPATH ([#676](https://github.com/operator-framework/operator-sdk/pull/676))
- Ansible Operator "--kubeconfig" command line option fixed ([#705](https://github.com/operator-framework/operator-sdk/pull/705))

## v0.1.1

### Bug fixes
- Fix hardcoded CRD version in crd scaffold ([#690](https://github.com/operator-framework/operator-sdk/pull/690))

## v0.1.0

### Changed

- Use [controller runtime](https://github.com/kubernetes-sigs/controller-runtime) library for controller and client APIs
- See [migration guide](https://github.com/operator-framework/operator-sdk/blob/master/doc/migration/v0.1.0-migration-guide.md) to migrate your project to `v0.1.0`

## v0.0.7

### Added

- Service account generation ([#454](https://github.com/operator-framework/operator-sdk/pull/454))
- Leader election ([#530](https://github.com/operator-framework/operator-sdk/pull/530))
- Incluster test support for test framework ([#469](https://github.com/operator-framework/operator-sdk/pull/469))
- Ansible type operator generation support ([#486](https://github.com/operator-framework/operator-sdk/pull/486), [#559](https://github.com/operator-framework/operator-sdk/pull/559))

### Changed

- Moved the rendering of `deploy/operator.yaml` to the `operator-sdk new` command instead of `operator-sdk build`

## v0.0.6

### Added

- Added `operator-sdk up` command to help deploy an operator. Currently supports running an operator locally against an existing cluster e.g `operator-sdk up local --kubeconfig=<path-to-kubeconfig> --namespace=<operator-namespace>`. See `operator-sdk up -h` for help. [#219](https://github.com/operator-framework/operator-sdk/pull/219) [#274](https://github.com/operator-framework/operator-sdk/pull/274)
- Added initial default metrics to be captured and exposed by Prometheus. [#323](https://github.com/operator-framework/operator-sdk/pull/323) exposes the metrics port and [#349](https://github.com/operator-framework/operator-sdk/pull/323) adds the initial default metrics.
- Added initial test framework for operators [#377](https://github.com/operator-framework/operator-sdk/pull/377), [#392](https://github.com/operator-framework/operator-sdk/pull/392), [#393](https://github.com/operator-framework/operator-sdk/pull/393)

### Changed

- All the modules in [`pkg/sdk`](https://github.com/operator-framework/operator-sdk/tree/4a9d5a5b0901b24679d36dced0a186c525e1bffd/pkg/sdk) have been combined into a single package. `action`, `handler`, `informer` `types` and `query` pkgs have been consolidated into `pkg/sdk`. [#242](https://github.com/operator-framework/operator-sdk/pull/242)
- The SDK exposes the Kubernetes clientset via `k8sclient.GetKubeClient()` #295
- The SDK now vendors the k8s code-generators for an operator instead of using the prebuilt image `gcr.io/coreos-k8s-scale-testing/codegen:1.9.3` [#319](https://github.com/operator-framework/operator-sdk/pull/242)
- The SDK exposes the Kubernetes rest config via `k8sclient.GetKubeConfig()` #338
- Use `time.Duration` instead of `int` for `sdk.Watch` [#427](https://github.com/operator-framework/operator-sdk/pull/427)

### Fixed

- The cache of available clients is being reset every minute for discovery of newely added resources to a cluster. [#280](https://github.com/operator-framework/operator-sdk/pull/280)
