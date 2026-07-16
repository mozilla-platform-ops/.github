# Mozilla Platform Operations

This is the public GitHub home of Mozilla's Release Operations (RelOps) team. We build and operate the worker fleet that runs Firefox continuous integration jobs in Taskcluster. That includes machine images, host configuration, hardware and device provisioning, cloud infrastructure, and fleet health.

GitHub holds our public code and project documentation. Mozilla staff can find our internal documentation and contact the team here:

- 💬 [`#relops` on Mozilla Slack](https://mozilla.enterprise.slack.com/archives/CNN462N2F) - Ask the team questions here. The channel requires a Mozilla Slack account.
- 📚 [RelOps on Mana](https://mana.mozilla.org/wiki/spaces/ROPS/overview) - The larger collection of internal runbooks, service ownership notes, access procedures, and team documentation.
- 🎤 [`presentations`](https://github.com/mozilla-platform-ops/presentations) - Public presentations and diagrams about the team's infrastructure and projects.

## Platform support

### Windows

The Windows fleet runs in Azure on Windows 10, Windows 11 24H2 and 25H2, and Windows Server 2022. It includes x64 and Arm64 testers and builders.

- 🖼️ [`worker-images`](https://github.com/mozilla-platform-ops/worker-images) - Builds and validates the Azure VM images used by Windows Taskcluster worker pools. It also publishes release notes and software bills of materials for those images.
- 🧰 [`ronin_puppet`](https://github.com/mozilla-platform-ops/ronin_puppet) - Holds the Puppet roles, profiles, and modules that install and configure the software on Windows workers.
- 🚦 [`fxci-config`](https://github.com/mozilla-releng/fxci-config) - Defines the Taskcluster worker pools that deploy each image. This repository lives in the `mozilla-releng` organization because it configures Firefox CI as a whole.

### Linux

The Linux fleet uses Ubuntu 24.04 images in GCP, with amd64, Arm64, headless, GUI, and trusted pools. RelOps also manages configuration for physical Linux performance workers.

- 🐧 [`worker-images`](https://github.com/mozilla-platform-ops/worker-images) - Builds and tests the Ubuntu images used by GCP worker pools, including trusted level 3 variants.
- 🔧 [`ronin_puppet`](https://github.com/mozilla-platform-ops/ronin_puppet) - Configures long-running Linux hosts, including performance and Android device-host systems.
- 🚦 [`fxci-config`](https://github.com/mozilla-releng/fxci-config) - Connects Taskcluster worker pools to the production GCP images and controls how those pools are provisioned.

### macOS

Firefox CI uses both bare-metal and virtualized Apple silicon workers. RelOps handles their host configuration, VM image builds, and hardware reprovisioning.

- 🍎 [`ronin_puppet`](https://github.com/mozilla-platform-ops/ronin_puppet) - Configures macOS worker roles and the hosts that run virtualized workers.
- 📦 [`macos-vms`](https://github.com/mozilla-platform-ops/macos-vms) - Builds macOS Sequoia VM images with Packer, Tart, and Puppet, then publishes them to an OCI registry for CI hosts.
- 🛰️ [`relops-bootstrap`](https://github.com/mozilla-platform-ops/relops-bootstrap) - Provides zero-touch provisioning and reprovisioning for Mac workers, including MDM enrollment, per-host credentials, Puppet setup, and Taskcluster registration.

### Android

Firefox tests run on physical Android devices through external device providers. RelOps maintains the task dispatch service, device-host tools, and performance diagnostics around those devices.

- 📱 [`mozilla-bitbar-devicepool`](https://github.com/mozilla-platform-ops/mozilla-bitbar-devicepool) - Connects Taskcluster tests to physical Android devices hosted by Bitbar and LambdaTest.
- 🛠️ [`android-tools`](https://github.com/mozilla-platform-ops/android-tools) - Contains small command-line tools used to inspect and manage Android devices in Mozilla's test infrastructure.
- ⏱️ [`fleetbench`](https://github.com/mozilla-platform-ops/fleetbench) - Benchmarks CPU and ADB I/O performance on worker hosts and Android devices. Linux and macOS support is in use; Windows support is still under development.

## Fleet services and operations

- ☁️ [`relops_infra_as_code`](https://github.com/mozilla-platform-ops/relops_infra_as_code) - Manages RelOps cloud and service infrastructure with Terraform across Azure, GCP, and AWS.
- ✈️ [`hangar`](https://github.com/mozilla-platform-ops/hangar) - Gives Mozilla staff a live view of CI workers, pool health, alerts, and hardware state. It combines data from Taskcluster and fleet-management systems.
- 🩺 [`pool-classifier`](https://github.com/mozilla-platform-ops/pool-classifier) - Classifies recent Taskcluster failures and shows worker-pool health, success rates, and failures that still need investigation.
- 📊 [`dashboards`](https://github.com/mozilla-platform-ops/dashboards) - Stores the Grafana dashboards used to monitor worker and service metrics.
- 🚚 [`fleetroll_mvp`](https://github.com/mozilla-platform-ops/fleetroll_mvp) - Audits Puppet state and configuration rollouts on long-running Linux and macOS hosts. It is an MVP rather than the final fleet-management service.

## Tools for Firefox developers

- 📲 [`BuildWatch`](https://github.com/mozilla-platform-ops/BuildWatch) - An iOS app for checking Firefox CI pushes and jobs from Treeherder and Taskcluster.
- 🤖 [`BuildWatch-Android`](https://github.com/mozilla-platform-ops/BuildWatch-Android) - The Android version of BuildWatch, with try-push filtering, job details, failure summaries, and build notifications.

## How changes reach workers

Image and machine configuration usually starts in [`worker-images`](https://github.com/mozilla-platform-ops/worker-images) or [`ronin_puppet`](https://github.com/mozilla-platform-ops/ronin_puppet). The worker-pool definitions that deploy those changes live in [`mozilla-releng/fxci-config`](https://github.com/mozilla-releng/fxci-config).

Firefox CI images must pass tier 1 integration tests before they are deployed to production pools.

## Contributing

Each repository documents its own setup, tests, and contribution process. Send pull requests to the repository that owns the code, and use the issue tracker named in that repository.
