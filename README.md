# Description

Deploy prometheus iperf3 exporter using ansible.

## Role Variables
All variables which can be overridden are stored in defaults/main.yml file.

## Prometheus Configuration

The iPerf3 exporter needs to be passed the target as a parameter, this can be done with relabelling.
Optional: pass the port that the target iperf3 server is lisenting on as the "port" parameter.

Example config:
```yml
scrape_configs:
  - job_name: 'iperf3'
    metrics_path: /probe
    static_configs:
      - targets:
        - foo.server
        - bar.server
    params:
      port: ['5201']
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9579  # The iPerf3 exporter's real hostname:port.
```

### Querying the bandwidth

You can use the following Prometheus query to get the receiver bandwidth (download speed on measured iperf server) in Mbits/sec:

```
iperf3_received_bytes / iperf3_received_seconds * 8 / 1000000
```

## License

Apache License 2.0, see [LICENSE](https://github.com/).
