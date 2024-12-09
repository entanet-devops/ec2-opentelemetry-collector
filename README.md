
# Role: OpenTelemetry Collector Configuration

## Description
This Ansible role installs and configures the OpenTelemetry Collector (`otelcol-contrib`) and Prometheus Node Exporter on Linux systems. It includes support for systemd service management, log rotation, and custom memory limits.

---

## Requirements
- **Supported OS:** Ubuntu 20.04, Amazon Linux 2
- **Dependencies:**
  - Python 3.8+
  - Ansible 2.10+

---

## Role Variables
| Variable Name                | Default Value                                                                                          | Description                                                                                       |
|------------------------------|------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `otel_debug_level`           | `normal`                                                                                           | Logging level for the OpenTelemetry Collector.                                                   |
| `otel_user`                  | `otel`                                                                                            | The user under which the OpenTelemetry Collector runs.                                           |
| `otel_group`                 | `otel`                                                                                            | The group under which the OpenTelemetry Collector runs.                                          |
| `otel_config_path`           | `/etc/otelcol-contrib/opentel.yaml`                                                                | Path to the OpenTelemetry Collector configuration file.                                          |
| `otel_service_name`          | `otelcol-contrib`                                                                                 | Name of the OpenTelemetry Collector systemd service.                                             |
| `otel_service_path`          | `/etc/systemd/system/{{ otel_service_name }}.service`                                             | Path to the OpenTelemetry Collector service file.                                                |
| `otel_exec_path`             | `/usr/bin/otelcol-contrib`                                                                        | Path to the OpenTelemetry Collector executable.                                                  |
| `otel_log_path`              | `/var/log/otelcol/collector.log`                                                                  | Path to the OpenTelemetry Collector log file.                                                    |
| `otel_version`               | `v0.115.0`                                                                                       | Version of the OpenTelemetry Collector to install.                                               |
| `otel_url`                   | `https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/...`        | URL to download the OpenTelemetry Collector package.                                             |
| `monitoring_ssm_path`        | `/devops/production/`                                                                             | SSM path for monitoring configuration.                                                           |
| `monitoring_endpoint`        | Value from AWS SSM                                                                                | Monitoring endpoint fetched from AWS Systems Manager Parameter Store.                            |
| `otel_config_override`       | `""`                                                                                              | Override configuration for OpenTelemetry Collector, if needed.                                   |
| `otel_go_mem_limit_env`      | `GOMEMLIMIT=400MiB`                                                                               | Environment variable for Go memory limit.                                                        |
| `log_rotate_max_megabytes`   | `10`                                                                                              | Rotate log files when they reach 10 MB.                                                          |
| `log_rotate_max_days`        | `5`                                                                                               | Retain log files for 5 days.                                                                     |
| `log_rotate_max_backups`     | `3`                                                                                               | Keep a maximum of 3 backup log files.                                                            |
| `log_rotate_localtime`       | `true`                                                                                           | Use local time for log rotation timestamps.                                                      |
| `limit_mib`                  | `500`                                                                                            | Memory limit for the OpenTelemetry Collector in MiB.                                             |
| `spike_limit_mib`            | `100`                                                                                            | Additional memory allowance for temporary spikes in MiB.                                         |
| `check_interval`             | `1s`                                                                                             | Interval for checking memory usage.                                                              |
| `send_batch_size`            | `1024`                                                                                           | Batch size for sending telemetry data.                                                           |
| `timeout`                    | `5s`                                                                                             | Timeout for sending telemetry batches.                                                           |
| `send_batch_max_size`        | `2048`                                                                                           | Maximum batch size for telemetry data.                                                           |
| `exporter_user`              | `exporter`                                                                                       | User under which the Node Exporter runs.                                                         |
| `exporter_group`             | `exporter`                                                                                       | Group under which the Node Exporter runs.                                                        |
| `exporter_version`           | `v1.8.2`                                                                                        | Version of Prometheus Node Exporter to install.                                                  |
| `exporter_url`               | `https://github.com/prometheus/node_exporter/releases/download/...`                              | URL to download the Node Exporter package.                                                       |
| `exporter_port`              | `9100`                                                                                           | Port on which the Node Exporter listens.                                                         |

---

## Usage

### Example Playbook
```yaml
- hosts: all
  roles:
    - role: otel-collector-config
      vars:
        otel_debug_level: debug
        otel_version: v0.120.0
        monitoring_ssm_path: "/devops/staging/"
        log_rotate_max_days: 10
```

---

## Dependencies
- AWS Systems Manager (SSM) must be configured if `monitoring_endpoint` uses SSM lookups.
- Requires Prometheus server if Node Exporter metrics are collected.

---

## Author Information
Created by [Your Name/Team Name].  
For issues, please contact [your email/team contact].  

---
