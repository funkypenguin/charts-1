# Change Log

This file documents all notable changes to Falco Helm Chart. The release
numbering uses [semantic versioning](http://semver.org).

## v6.2.1

* Bump container plugin to 0.3.3

## v6.2.0

* Switch to `collectors.containerEngine` configuration by default
* Update `collectors.containerEngine.engines` default values
* Fix containerd socket path configuration
* Address "container.name shows container.id" issue
* Address "Missing k8s.pod name, container.name, other metadata with k3s" issue
* Bump container plugin to 0.3.2

## v6.1.0

* feat(falco): Add possibility to custom falco pods hostname

## v6.0.2

* Bump Falco to 0.41.3
* Bump container plugin to 0.3.1

## v6.0.1

* Bump Falco to 0.41.2
* Bump container plugin to 0.3.0

## v6.0.0

* Rename Falco Talon configuration keys naming

## v5.0.3

* Bump container plugin to 0.2.6

## v5.0.2

* Bump container plugin to 0.2.5
* Bump Falco to 0.41.1

## v5.0.1

* Correct installation issue when both artifact installation and follow are enabled

## v5.0.0
* Bump falcoctl to 0.11.2
* Use default falco image flavor (wolfi) by default

## v4.22.0
* Bump Falco to 0.41.0;
* Bump falco rules to 4.0.0;
* Deprecate old container engines in favor of the new container plugin;
* Add support for the new container plugin;
* Update k8smeta plugin to 0.3.0;
* Update falco configuration;

## v4.21.2

* add falco-talon as falco subchart

## v4.21.1

* removed falco-expoter (now deprecated) references from the readme 

## v4.21.0

* feat(falco): adding imagePullSecrets at the service account level

## v4.20.1

* correctly mount the volumes based on socket path
* unit tests for container engines socket paths

## v4.20.0

* bump falcoctl to 0.11.0

## v4.19.0

* fix falco version to 0.40.0

## v4.18.0

* update the chart for falco 0.40;
* remove deprecated cli flag `--cri` and use instead the configuration file. More info here: https://github.com/falcosecurity/falco/pull/3329
* use new falco images, for more info see: https://github.com/falcosecurity/falco/issues/3165

## v4.17.2

* update(falco): add ports definition in falco container spec

## v4.17.1

* docs(falco): update README.md to reflect latest driver configuration and correct broken links

## v4.17.0

* update(falco): bump k8saudit version to 0.11

## v4.16.2

* fix(falco): set dnsPolicy to ClusterFirstWithHostNet when gvisor driver is enabled to prevent DNS lookup failures for cluster-internal services

## v4.16.1

* fix(falco/serviceMonitor): set service label selector
* new(falco/tests): add unit tests for serviceMonitor label selector

## v4.16.0

* bump falcosidekick dependency to v0.9.* to match with future versions

## v4.15.1

* fix: change the url for the concurrent queue classes docs

## v4.15.0

* update(falco): bump falco version to 0.39.2 and falcoctl to 0.10.1

## v4.14.2

* fix(falco/readme): use `rules_files` instead of deprecated `rules_file` in README config snippet

## v4.14.1

* fix(falco/dashboard): make pod variable independent of triggered rules. CPU and memory are now visible for each 
  pod, even when no rules have been triggered for that falco instance.

## v4.14.0

* Bump k8smeta plugin to 0.2.1, see: https://github.com/falcosecurity/plugins/releases/tag/plugins%2Fk8smeta%2Fv0.2.1

## v4.13.0

* Expose new config entries for k8smeta plugin:`verbosity` and `hostProc`.

## v4.12.0

* Set apparmor to `unconfined` (disabled) when `leastPrivileged: true` and (`kind: modern_ebpf` or `kind: ebpf`)

## v4.11.2

* only prints env key if there are env values to be passed on `falcoctl.initContainer` and `falcoctl.sidecar`

## v4.11.1

* add details for the scap drops buffer charts with the dir  and drops labels

## v4.11.0

* new(falco): add grafana dashboard for falco

## v4.10.0

* Bump Falco to v0.39.1

## v4.9.1

* feat(falco): add labels and annotations to the metrics service

## v4.9.0

* Bump Falco to v0.39.0
* update(falco): add new configuration entries for Falco
  This commit adds new config keys introduces in Falco 0.39.0.
  Furthermore, updates the unit tests for the latest changes
  in the values.yaml.
* cleanup(falco): remove deprecated falco configuration
  This commit removes the "output" config key that has
  been deprecated in falco.
* update(falco): mount proc filesystem for plugins
  The following PR in libs https://github.com/falcosecurity/libs/pull/1969
  introduces a new platform for plugins that requires access to the
  proc filesystem.
* fix(falco): update broken link pointing to Falco docs
  After the changes made by the following PR to the Falco docs https://github.com/falcosecurity/falco-website/pull/1362
  this commit updates a broken link.

## v4.8.3

* The init container, when driver.kind=auto, automatically generates
  a new Falco configuration file and selects the appropriate engine
  kind based on the environment where Falco is deployed.

  With this commit, along with falcoctl PR #630, the Helm charts now 
  support different driver kinds for Falco instances based on the 
  specific node they are running on. When driver.kind=auto is set,
  each Falco instance dynamically selects the most suitable 
  driver (e.g., ebpf, kmod, modern_ebpf) for the node.
  +-------------------------------------------------------+
  | Kubernetes Cluster                                    |
  |                                                       |
  |  +-------------------+  +-------------------+        |
  |  | Node 1             |  | Node 2             |        |
  |  |                   |  |                   |        |
  |  | Falco (ebpf) |  | Falco (kmod)       |        |
  |  +-------------------+  +-------------------+        |
  |                                                       |
  |                 +-------------------+                |
  |                 | Node 3             |                |
  |                 |                   |                |
  |                 | Falco (modern_ebpf)|                |
  |                 +-------------------+                |
  +-------------------------------------------------------+

## v4.8.2

* fix(falco): correctly mount host filesystems when driver.kind is auto

  When falco runs with kmod/module driver it needs special filesystems
  to be mounted from the host such /dev and /sys/module/falco.
  This commit ensures that we mount them in the falco container.

  Note that, the /sys/module/falco is now mounted as /sys/module since
  we do not know which kind of driver will be used. The falco folder 
  exists under /sys/module only when the kernel module is loaded, 
  hence it's not possible to use the /sys/module/falco hostpath when driver.kind 
  is set to auto.

## v4.8.1

* fix(falcosidekick): add support for custom service type for webui redis

## v4.8.0

* Upgrade Falco version to 0.38.2

## v4.7.2

* use rules_files key in the preset values files

## v4.7.1

* fix(falco/config): use rules_files instead of deprecated key rules_file

## v4.7.0

* bump k8smeta plugin to version 0.2.0. The new version, resolves a bug that prevented the plugin
  from populating the k8smeta fields. For more info see:
  * https://github.com/falcosecurity/plugins/issues/514
  * https://github.com/falcosecurity/plugins/pull/517

## v4.6.3

* fix(falco): mount client-certs-volume only if certs.existingClientSecret is defined

## v4.6.2

* bump falcosidekick dependency to v0.8.* to match with future versions

## v4.6.1

* bump falcosidekick dependency to v0.8.2 (fixes bug when using externalRedis in UI)

## v4.6.0

* feat(falco): add support for Falco metrics

## v4.5.2

* bump falcosidekick dependency version to v0.8.0, for falcosidekick 2.29.0

## v4.5.2

* reording scc configuration, making it more robust to plain yaml comparison

## v4.5.1

* falco is now able to reconnect to containerd.socket

## v4.5.0

* bump Falco version to 0.38.1

## v4.4.3

* Added a `labels` field in the controller to provide extra labeling for the daemonset/deployment

## v4.4.2

* fix wrong check in pod template where `existingSecret` was used instead of `existingClientSecret`

## v4.4.1

* bump k8s-metacollector dependency version to v0.1.1. See: https://github.com/falcosecurity/k8s-metacollector/releases

## v4.3.1

* bump falcosidekick dependency version to v0.7.19 install latest version through falco chart

## v4.3.0

* `FALCO_HOSTNAME` and `HOST_ROOT` are now set by default in pods configuration.

## v4.2.6

* bump falcosidekick dependency version to v0.7.17 install latest version through falco chart

## v4.2.5

* fix docs

## v4.2.4

* bump falcosidekick dependency version to v0.7.15 install latest version through falco chart

## v4.2.3

* fix(falco/helpers): adjust formatting to be compatible with older helm versions

## v4.2.2

* fix(falco/README): dead link

## v4.2.1
* fix(falco/README): typos, formatting and broken links

## v4.2.0

* Bump falco to v0.37.1 and falcoctl to v0.7.2

## v4.1.2
* Fix links in output after falco install without sidekick

## v4.1.1

* Update README.md.

## v4.1.0

* Reintroduce the service account.

## v4.0.0
The new chart introduces some breaking changes. For folks upgrading Falco please see the BREAKING-CHANGES.md file.

* Uniform driver names and configuration to the Falco one: https://github.com/falcosecurity/falco/pull/2413;
* Fix usernames and groupnames resolution by mounting the `/etc` filesystem;
* Drop old kubernetes collector related resources;
* Introduce the new k8s-metacollector and k8smeta plugin (experimental);
* Enable the dependency resolver for artifacts in falcoctl since the Falco image does not ship anymore the plugins;
* Bump Falco to 0.37.0;
* Bump falcoctl to 0.7.0.

## v3.8.7

*  Upgrade falcosidekick chart to `v0.7.11`.

## v3.8.6

* no changes to the chart itself. Updated README.md and makefile.

## v3.8.5

* Add mTLS cryptographic material load via Helm for Falco

## v3.8.4

* Upgrade Falco to 0.36.2: https://github.com/falcosecurity/falco/releases/tag/0.36.2

## v3.8.3

* Upgrade falcosidekick chart to `v0.7.7`.

## v3.8.2

* Upgrade falcosidekick chart to `v0.7.6`.

## v3.8.1

* noop change just to test the ci

## v3.8.0

* Upgrade Falco to 0.36.1: https://github.com/falcosecurity/falco/releases/tag/0.36.1
* Sync values.yaml with 0.36.1 falco.yaml config file.

## v3.7.1

* Update readme

## v3.7.0

* Upgrade Falco to 0.36. https://github.com/falcosecurity/falco/releases/tag/0.36.0
* Sync values.yaml with upstream falco.yaml config file.
* Upgrade falcoctl to 0.6.2. For more info see the release notes: https://github.com/falcosecurity/falcoctl/releases/tag/v0.6.2

## v3.6.2

* Cleanup wrong files

## v3.6.1

* Upgrade falcosidekick chart to `v0.7.1`.

## v3.6.0

* Add `outputs` field to falco configuration

## v3.5.0

## Major Changes

* Support configuration of revisionHistoryLimit of the deployment

## v3.4.1

* Upgrade falcosidekick chart to `v0.6.3`.

## v3.4.0

* Introduce an ability to use an additional volumeMounts for `falcoctl-artifact-install` and `falcoctl-artifact-follow` containers.

## v3.3.1

* No changes made to the falco chart, only some fixes in the makefile

## v3.3.0
* Upgrade Falco to 0.35.1. For more info see the release notes: https://github.com/falcosecurity/falco/releases/tag/0.35.1
* Upgrade falcoctl to 0.5.1. For more info see the release notes: https://github.com/falcosecurity/falcoctl/releases/tag/v0.5.1
* Introduce least privileged mode in modern ebpf. For more info see: https://falco.org/docs/setup/container/#docker-least-privileged-modern-ebpf

## v3.2.1
* Set falco.http_output.url to empty string in values.yaml file

## v3.2.0
* Upgrade Falco to 0.35.0. For more info see the release notes: https://github.com/falcosecurity/falco/releases/tag/0.35.0
* Sync values.yaml with upstream falco.yaml config file.
* Upgrade falcoctl to 0.5.0. For more info see the release notes: https://github.com/falcosecurity/falcoctl/releases/tag/v0.5.0
* The tag used to install and follow the falco rules is `1`
* The tag used to install and follow the k8saudit rules is `0.6`

## v3.1.5

* Use list as default for env parameter of init and follow containers

## v3.1.4

* Fix typo in values-k8audit file

## v3.1.3

* Updates the grpc-service to use the correct label selector

## v3.1.2

* Bump `falcosidekick` dependency to 0.6.1

## v3.1.1
* Update `k8saudit` section in README.md file.

## v3.1.0
* Upgrade Falco to 0.34.1

## v3.0.0
* Drop support for falcosecuriy/falco image, only the init container approach is supported out of the box;
* Simplify the driver-loader init container logic;
* Support **falcoctl** tool in the chart:
  * Install the *rulesfile* artifacts;
  * Follow the *rulesfile* artifacts in order to have the latest rules once they are released from falcosecurity org;
* Support the **modern-bpf** probe a new driver (experimental)
* Add a new file *BREAKING_CHANGES.md* to document the breaking changes and how to update the new chart.

## v2.5.5

* Bump `falcosidekick` dependency to 0.5.16

## v2.5.4

* Fix incorrect entry in v2.5.2 changelog

## v2.5.3

* Bump `falcosidekick` dependency to 0.5.14

## v2.5.2

* Fixed notes template to only include daemon set info if set to daemon set

## v2.5.1

* Update README to clarify driver behavior for chart

## v2.5.0

* Support custom dictionaries when setting environment variables

Note: this is a breaking change. If you were passing _objects_ to `extra.env` or `driver.loader.initContainer.env` , you will need to update your values file to pass _lists_.

## v2.4.7

* Add `controller.annotations` configuration

## v2.4.6

* Bump `falcosidekick` dependency to 0.5.11

## v2.4.5

* Bump `falcosidekick` dependency to 0.5.10

## v2.4.4

* Update README for gRPC

## v2.4.3

* Update README for gVisor and GKE

## v2.4.2

* Add toleration for node-role.kubernetes.io/control-plane

## v2.4.1

* Fixed error in values.yaml comments

## v2.4.0

* Add support for Falco+gVisor
* Add new preset `values.yaml `file for gVisor-enabled GKE clusters

## v2.3.1

* Fixed incorrect spelling of `been`

## v2.3.0

* Add variable namespaceOverride to allow setting release namespace in values

## v2.2.0

* Change the grpc socket path from `unix:///var/run/falco/falco.soc` to `unix:///run/falco/falco.sock`. Please note that this change is potentially a breaking change if upgrading falco from a previous version and you have external consumers of the grpc socket.

## v2.1.0

* Bump Falco to 0.33.0
* Implicitly disable `syscall` source when not required
* Update `values.yaml` to reflect the new configuration options in Falco 0.33.0
* Mount `/sys/module/falco` when deployed using the `kernel module`
* Update rulesets for falco and plugins

## v2.0.18

* Bump `falcosidekick` dependency to 0.5.9

## v2.0.17

* Fix: remove `namespace` from `clusterrole` and `clusterrolebinding` metadata

## v2.0.16

* Allow setting `resources` and `securityContext` on the `falco-driver-loader` init container

## v2.0.15

* Allow passing args to the `falco-driver-loader` init container

## v2.0.14

* Fix debugfs mount when `falco-no-driver` image and ebpf driver is used

## v2.0.13

* Upgrade Falco to 0.32.2

## v2.0.12

* Fully disable the driver when running in CI

## v2.0.11

* Correct CI values.

## v2.0.10

* Fix name of the falco certs secret.

## v2.0.9

* Fix the `certs-secret.yaml` template by correctly pointing to the root context when using the helpers.

## v2.0.8

* When using ebpf probe Falco is deployed in `privileged` mode instead of `least privileged`.

## v2.0.7

* Fix templating for priorityClassName in pod-template.tpl

## v2.0.6

* Add ability to enable `tty` for the falco container. Needed to force falco logs to be immediately displayed as they are emitted. Useful in test/debug scenarios.

## v2.0.5

* Mount `/proc` only when syscall data source is enabled (default). This behaviour can be overridden via `mounts.enforceProcMount` for edge cases where the `/proc` `hostPath` mount is required without having the syscall data source enabled at the same time.

## v2.0.4

* Fix templating for init containers in pod-template.tpl

## v2.0.3

* Add ability to specify extra environment variables to driver loader initContainer

## v2.0.2

update(falco/OWNERS): move inactive approvers to emeritus_approvers

## v2.0.1

* Add description for configuration variable in values.yaml
* Add linting target in Makefile
* Remove configuration values table from README.md
* Fix section titles in README.md

## v2.0.0

**Note**
*This release is a complete refactor of the Falco Helm Chart. Thus, it introduces some breaking changes.*
*Please, do not reuse values from previous chart installations.*

* Upgrade Falco to 0.32.1
* Massive refactoring of the chart implementation
* Add ability to use either a daemonset or a deployment (depending on the installation scenario)
* Add ability to specify custom network services
* New settings for the drivers configuration
* New Makefile to generate helm documentation
* Add values-k8saudit.yaml preset for the k8saudit plugin
* Fix use `load_plugins` instead of `loadPlugins` in Falco configuration
* Update `containerSecurityContext` (former `securityContext`) now takes precedence over auto configs
* Move `leastPriviledged` mode under eBPF and add missing `SYS_PTRACE` cap
* Update group values for metadata collection under "collectors"
* Remove several settings in favour of `extra.env`
* Use chart `appVersion` as default image tag
* Move setting from `image.pullSecrets` to `imagePullSecrets`
* Add an option to set desidered replicas
* Improve selector labels
* Modernize labels and improve internal helpers
* Deprecate PSP (template removed)
* Fake event generator removed from this chart

## v1.19.4

* Bump Falco Sidekick dependency.

## v1.19.3

* Add `watchConfigFiles` value to falco README

## v1.19.2

* Bump Falco Sidekick dependency.
* Add support for DaemonSet podSecurityContext and securityContext.

## v1.19.1

* Fix the changelog for 1.19.0

## v1.19.0

* Upgrade to Falco 0.32.0 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.32.0/CHANGELOG.md))
* Various Falco config settings were updated for Falco 0.32.0

### Breaking Changes

* Audit Log is now supported via k8saudit plugin (when enabled, syscall instrumentation will be disabled)
* dynamicBackend support for Audit Log is now deprecated

## v1.18.6

* Bump falcosidekick chart dependency (fix issue with the UI)

## v1.18.5

* Bump falcosidekick chart dependency

## v1.18.4

* Now the url to falcosidekick on NOTES.txt on falco helm chart points to the right place.

## v1.18.3

* Fix for [issue 318](https://github.com/falcosecurity/charts/issues/318) - Missing comma in k8s_audit_rules.yaml.

## v1.18.2

* Further fix for `--reuse-values` option after the introduction of `crio.enabled`.

## v1.18.1

* Workaround to make this chart work with Helm `--reuse-values` option after the introduction of `crio.enabled`.

## v1.18.0

* Added support for cri-o

## v1.17.6

Remove whitespace around `falco.httpOutput.url` to fix the error `libcurl error: URL using bad/illegal format or missing URL`.

## v1.17.5

* Changed `falco.httpOutput.url` so that it always overrides the default URL, even when falcosidekick is enabled. (NOTE: don't use this version, see v1.17.6)

## v1.17.4

* Upgrade to Falco 0.31.1 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.31.1/CHANGELOG.md))
* Update rulesets from Falco 0.31.1

## v1.17.3

* Fix quoting around `--k8s-node`

## v1.17.2

* Add `leastPrivileged.enabled` configuration

## v1.17.1

* Fixed `priority` level `info` change to `informational`

## v1.17.0

* Upgrade to Falco 0.31.0 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.31.0/CHANGELOG.md))
* Update rulesets from Falco 0.31.0
* Update several configuration options under the `falco` node to reflect the new Falco version
* Initial plugins support

## v1.16.4

* Bump falcosidekick chart dependency

## v1.16.2

* Add `serviceAccount.annotations` configuration

## v1.16.1

* Fixed string escaping for `--k8s-node`

## v1.16.0

* Upgrade to Falco 0.30.0 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.30.0/CHANGELOG.md))
* Update rulesets from Falco 0.30.0
* Add `kubernetesSupport.enableNodeFilter` configuration to enable node filtering when requesting pods metadata from Kubernetes
* Add `falco.metadataDownload` configuration for fine-tuning container orchestrator metadata fetching params
* Add `falco.jsonIncludeTagsProperty` configuration to include tags in the JSON output

## v1.15.7

* Removed `maxSurge` reference from comment in Falco's `values.yaml` file.

## v1.15.6

* Update `Falcosidekick` chart to 0.3.13

## v1.15.4

* Update `Falcosidekick` chart to 0.3.12

## v1.15.3

* Upgrade to Falco 0.29.1 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.29.1/CHANGELOG.md))
* Update rulesets from Falco 0.29.1

## v1.15.2

* Add ability to use an existing secret of key, cert, ca as well as pem bundle instead of creating it from files

## v1.15.1

* Fixed liveness and readiness probes schema when ssl is enabled

## v1.14.1

* Update `Falcosidekick` chart to 0.3.8

## v1.14.1

* Update image tag to 0.29.0 in values.yaml

## v1.14.0

* Upgrade to Falco 0.29.0 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.29.0/CHANGELOG.md))
* Update rulesets from Falco 0.29.0

## v1.13.2

* Fixed incorrect spelling of `fullfqdn`

## v1.13.1

* Fix port for readinessProbe and livenessProbe

## v1.13.0

* Add liveness and readiness probes to Falco

## v1.12.0

* Add `kubernetesSupport` configuration to make Kubernetes Falco support optional in the daemonset (enabled by default)

## v1.11.1

* Upgrade to Falco 0.28.1 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.28.1/CHANGELOG.md))

## v1.11.0

* Bump up version of chart for `Falcosidekick` dependency to `v3.5.0`

## v1.10.0

* Add `falcosidekick.fullfqdn` option to connect `falco` to `falcosidekick` with full FQDN
* Bump up version of chart for `Falcosidekick` dependency

## v1.9.0

* Upgrade to Falco 0.28.0 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.28.0/CHANGELOG.md))
* Update rulesets from Falco 0.28.0

## v1.8.1

* Bump up version of chart for `Falcosidekick` dependency

## v1.8.0

* Bump up version of chart for `Falcosidekick` dependency

## v1.7.10

* Update rule `Write below monitored dir` description

## v1.7.9

* Add a documentation section about the driver

## v1.7.8

* Increase CPU limit default value

## v1.7.7

* Add a documentation section about using init containers

## v1.7.6

* Correct icon URL
## v1.7.5

* Update downstream sidekick chart

## v1.7.4

* Add `ebpf.probe.path` configuration option

## v1.7.3

* Bump up version of chart for `Falcosidekick` dependency

## v1.7.2

* Fix `falco` configmap when `Falcosidekick` is enabled, wrong service name was used

## v1.7.1

* Correct image tag for Falco 0.27.0

## v1.7.0

* Upgrade to Falco 0.27.0 (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.27.0/CHANGELOG.md))
* Add `falco.output_timeout` configuration setting

## v1.6.1

### Minor Changes

* Add `falcosidekick` as an optional dependency

## v1.6.0

### Minor Changes

* Remove deprecated integrations (see [#123](https://github.com/falcosecurity/charts/issues/123))

## v1.5.8

### Minor Changes

* Add value `extraVolumes`, allow adding extra volumes to falco daemonset
* Add value `extraVolumeMounts`, allow adding extra volumeMounts to falco container in falco daemonset

## v1.5.6

### Minor Changes

* Add `falco.webserver.sslEnabled` config, enabling SSL support
* Add `falco.webserver.nodePort` configuration as an alternative way for exposing the AuditLog webhook (disabled by default)

## v1.5.5

### Minor Changes

* Support release namespace configuration

## v1.5.4

### Minor Changes

* Upgrade to Falco 0.26.2, `DRIVERS_REPO` now defaults to https://download.falco.org/?prefix=driver/ (see the [Falco changelog](https://github.com/falcosecurity/falco/blob/0.26.2/CHANGELOG.md))

## v1.5.3

### Minor Changes

* Deprecation notice for gcscc, natsOutput, snsOutput, pubsubOutput integrations
* Clean up old references from documentation

## v1.5.2

### Minor Changes

* Add Pod Security Policy Support for the fake event generator

## v1.5.1

### Minor Changes

* Replace extensions apiGroup/apiVersion because of deprecation

## v1.5.0

### Minor Changes

* Upgrade to Falco 0.26.1
* Update ruleset from Falco 0.26.1
* Automatically set the appropriate apiVersion for rbac

## v1.4.0

### Minor Changes

* Allow adding InitContainers to Falco pod with `extraInitContainers` configuration

## v1.3.0

### Minor Changes

* Upgrade to Falco 0.25.0
* Update ruleset from Falco 0.25.0

## v1.2.3

### Minor Changes

* Fix duplicate mount point problem when both gRPC and NATS integrations are enabled

## v1.2.2

### Minor Changes

* Allow configuration using values for `imagePullSecrets` setting
* Add `docker.io/falcosecurity/falco` image to `falco_privileged_images` macro

## v1.2.1

### Minor Changes

* Add SecurityContextConstraint to allow deploying in Openshift

## v1.2.0

### Minor Changes

* Upgrade to Falco 0.24.0
* Update ruleset from Falco 0.24.0
* gRPC Unix Socket support
* Set default threadiness to 0 ("auto" behavior) for the gRPC server

## v1.1.10

### Minor Changes

* Switch to `falcosecurity/event-generator`
* Allow configuration using values for `fakeEventGenerator.args` setting
* Update ruleset
* New releasing mechanism

## v1.1.9

### Minor Changes

* Add missing privileges for the apps Kubernetes API group
* Allow client config url for Audit Sink with `auditLog.dynamicBackend.url`

## v1.1.8

### Minor Changes

* Upgrade to Falco 0.23.0
* Correct socket path for `--cri` flag
* Always mount `/etc` (required by `falco-driver-loader`)

## v1.1.7

### Minor Changes

* Add pod annotation support for daemonset

## v1.1.6

### Minor Changes

* Upgrade to Falco 0.21.0
* Upgrade rules to Falco 0.21.0

## v1.1.5

### Minor Changes

* Add headless service for gRPC server
* Allow gRPC certificates configuration by using `--set-file`

## v1.1.4

### Minor Changes

* Make `/lib/modules` writable from the container

## v1.1.3

### Minor Changes

* Allow configuration using values for `grpc` setting
* Allow configuration using values for `grpc_output` setting

## v1.1.2

### Minor Changes

* Upgrade to Falco 0.20.0
* Upgrade rules to Falco 0.20.0

## v1.1.1

### Minor Changes

* Upgrade to Falco 0.19.0
* Upgrade rules to Falco 0.19.0
* Remove Sysdig references, Falco is a project by its own name

## v1.1.0

### Minor Changes

* Revamp auditLog feature
* Upgrade to latest version (0.18.0)
* Replace CRI references with containerD

## v1.0.12

### Minor Changes

* Support multiple lines for `falco.programOutput.program`

## v1.0.11

### Minor Changes

* Add affinity

## v1.0.10

### Minor Changes

* Migrate API versions from deprecated, removed versions to support Kubernetes v1.16

## v1.0.9

### Minor Changes

* Restrict the access to `/dev` on underlying host to read only

## v1.0.8

### Minor Changes

* Upgrade to Falco 0.17.1
* Upgrade rules to Falco 0.17.1

## v1.0.7

### Minor Changes

* Allow configuration using values for `nodeSelector` setting

## v1.0.6

### Minor Changes

* Falco does a rollingUpgrade when the falco or falco-rules configMap changes
  with a helm upgrade

## v1.0.5

### Minor Changes

* Add 3 resources (`daemonsets`, `deployments`, `replicasets`) to the ClusterRole resource list
  Ref: [PR#514](https://github.com/falcosecurity/falco/pull/514) from Falco repository

## v1.0.4

### Minor Changes

* Upgrade to Falco 0.17.0
* Upgrade rules to Falco 0.17.0

## v1.0.3

### Minor Changes

* Support [`priorityClassName`](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)

## v1.0.2

### Minor Changes

* Upgrade to Falco 0.16.0
* Upgrade rules to Falco 0.16.0

## v1.0.1

### Minor Changes

* Extra environment variables passed to daemonset pods

## v1.0.0

### Major Changes

* Add support for K8s audit logging

## v0.9.1

### Minor Changes

* Allow configuration using values for `time_format_iso8601` setting
* Allow configuration using values for `syscall_event_drops` setting
* Allow configuration using values for `http_output` setting
* Add CHANGELOG entry for v0.8.0, [not present on its PR](https://github.com/helm/charts/pull/14813#issuecomment-506821432)

## v0.9.0

### Major Changes

* Add nestorsalceda as an approver

## v0.8.0

### Major Changes

* Allow configuration of Pod Security Policy. This is needed to get Falco
  running when the Admission Controller is enabled.

## v0.7.10

### Minor Changes

* Fix bug with Google Cloud Security Command Center and Falco integration

## v0.7.9

### Minor Changes

* Upgrade to Falco 0.15.3
* Upgrade rules to Falco 0.15.3

## v0.7.8

### Minor Changes

* Add TZ parameter for time correlation in Falco logs

## v0.7.7

### Minor Changes

* Upgrade to Falco 0.15.1
* Upgrade rules to Falco 0.15.1

## v0.7.6

### Major Changes

* Allow to enable/disable usage of the docker socket
* Configurable docker socket path
* CRI support, configurable CRI socket
* Allow to enable/disable usage of the CRI socket

## v0.7.5

### Minor Changes

* Upgrade to Falco 0.15.0
* Upgrade rules to Falco 0.15.0

## v0.7.4

### Minor Changes

* Use the KUBERNETES_SERVICE_HOST environment variable to connect to Kubernetes
  API instead of using a fixed name

## v0.7.3

### Minor Changes

* Remove the toJson pipeline when storing Google Credentials. It makes strange
  stuff with double quotes and does not allow to use base64 encoded credentials

## v0.7.2

### Minor Changes

* Fix typos in README.md

## v0.7.1

### Minor Changes

* Add Google Pub/Sub Output integration

## v0.7.0

### Major Changes

* Disable eBPF by default on Falco. We activated eBPF by default to make the
  CI pass, but now we found a better method to make the CI pass without
  bothering our users.

## v0.6.0

### Major Changes

* Upgrade to Falco 0.14.0
* Upgrade rules to Falco 0.14.0
* Enable eBPF by default on Falco
* Allow to download Falco images from different registries than `docker.io`
* Use rollingUpdate strategy by default
* Provide sane defauls for falco resource management

## v0.5.6

### Minor Changes

* Allow extra container args

## v0.5.5

### Minor Changes

* Update correct slack example

## v0.5.4

### Minor Changes

* Using Falco version 0.13.0 instead of latest.

## v0.5.3

### Minor Changes

* Update falco_rules.yaml file to use the same rules that Falco 0.13.0

## v0.5.2

### Minor Changes

* Falco was accepted as a CNCF project. Fix references and download image from
  falcosecurity organization.

## v0.5.1

### Minor Changes

* Allow falco to resolve cluster hostnames when running with ebpf.hostNetwork: true

## v0.5.0

### Major Changes

* Add Amazon SNS Output integration

## v0.4.0

### Major Changes

* Allow Falco to be run with a HTTP proxy server

## v0.3.1

### Minor Changes

* Mount in memory volume for shm. It was used in volumes but was not mounted.

## v0.3.0

### Major Changes

* Add eBPF support for Falco. Falco can now read events via an eBPF program
  loaded into the kernel instead of the `falco-probe` kernel module.

## v0.2.1

### Minor Changes

* Update falco_rules.yaml file to use the same rules that Falco 0.11.1

## v0.2.0

### Major Changes

* Add NATS Output integration

### Minor Changes

* Fix value mismatch between code and documentation

## v0.1.1

### Minor Changes

* Fix several typos

## v0.1.0

### Major Changes

* Initial release of Sysdig Falco Helm Chart
