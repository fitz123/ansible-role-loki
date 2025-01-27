Role for managing [loki](https://github.com/grafana/loki)

For possible settings and format look at [example configs](https://github.com/grafana/loki/blob/master/docs/configuration/examples.md)  
This role only installs loki. For promtail, see [this role](https://github.com/patrickjahns/ansible-role-promtail)

## Compatibility
This role is compatible with any modern systemd-based distro.  

## Role Variables
| Variable name                   | Default value | Description                                        |
| ------------------------------- | ------------- | -------------------------------------------------- |
| loki_version                    | `2.3.0`       | version of loki                                    |
| loki_system_user                | `loki`        | user for running loki                              |
| loki_system_group               | `loki`        | group for running loki                             |
| loki_server_http_listen_port    | `3100`        | listen port for loki                               |
| loki_server_http_listen_address | `localhost`   | listen address for loki                            |
| loki_directories                | `[]`          | array of directories to create before running loki |
| loki_arguments                  | `[]`          | arguments to pass to loki binary                   |
| loki_memberlist                 | empty         | YAML with memberlist settings |
| loki_schema_config              | default dict  | YAML with schema config                            |
| loki_storage_config             | default dict  | YAML with storage config                           |
| loki_ingester                   | default dict  | YAML with ingester settings                        |
| loki_limits_config              | default dict  | YAML with limits settings                          |
| loki_chunk_store_config         | default dict  | YAML with chuck store settings                     |
| loki_table_manager              | default dict  | YAML with table manager settings                   |
| loki_compactor                  | empty         | YAML with compactor settings                       |
| loki_query_range                | empty         | YAML with query range settings
| loki_ruler                      | empty         | YAML with ruler settings
| loki_alert_rules                | `[]`          | YAML with alerts

## Settings
To configure loki with role you just need to supply YAML to each corresponding block. See example configs from loki github repo for examples.   
Default settings are provided to get "proof-of-concept" installation up and running, you can see them in defaults/main.yml  
Please note that by default loki writes to /tmp/, you will need to adjust loki_storage_config in case you want S3/GCS/other supported storage  

## Alerts (WIP, not working ATM)
To use alerts you will need to use loki's ruler. Default setting enables it, with rules stored in /tmp with empty ruleset. Alerts follows prometheus format. Sample alert:
```yaml
loki_alert_rules:
  groups:
    - name: nginx
      rules:
        - alert: nginx
          annotations:
            description: Errors in nginx requests
          expr: '{% raw %}sum(count_over_time({app="nginx", container="application", type="error"} > 0{% endraw %}'
          for: 1m
          labels:
            severity: warning
```
