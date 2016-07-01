# Ansible Role: prometheus

An Ansible role that installs Prometheus Monitoring server on Ubuntu-based machines with systemd.

## Requirements

All needed packages will be installed with this role.

## Role Variables

Available main variables are listed below, along with default values:
```yaml
prometheus_version: 0.20.0

prometheus_global_scrape_interval: 15s
prometheus_global_evaluation_interval: 15s
prometheus_global_scrape_timeout: 10s
prometheus_self_scrape_interval: "{{ prometheus_global_scrape_interval }}"
prometheus_self_evaluation_interval: "{{ prometheus_global_scrape_interval }}"

prometheus_root_dir: /opt/prometheus
prometheus_config_dir: /etc/prometheus
prometheus_pid_path: /var/run/prometheus.pid
prometheus_db_dir: /var/lib/prometheus

prometheus_web_external_url: "http://localhost:9090/"

prometheus_config_flags:
  'config.file': '{{ prometheus_config_dir }}/prometheus.yml'
  'storage.local.path': '{{ prometheus_db_dir }}'
  'web.external-url': '{{ prometheus_web_external_url }}'
```
All variables you can see [here](defaults/main.yml).

## Dependencies

This role doesn't have dependencies.

## Example Playbook
```yaml
- hosts: monitoring
  roles:
    - { role: UnderGreen.prometheus }
  vars:
    prometheus_jobs:
      - name: files_sd
        config_type: file_sd_configs
        config_options:
          files:
            - '{{ prometheus_file_sd_config_dir }}/*.json'
            - '{{ prometheus_file_sd_config_dir }}/*.yml'
            - '{{ prometheus_file_sd_config_dir }}/*.yaml'
            - { refresh_interval: '30s' }

      - name: static
        config_type: static_configs
        config_options:
          targets:
            - host.example.com:9000
```
## License

GPLv2
