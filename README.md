Simple dashboard to use with [ssl-exporter](https://github.com/ribbybibby/ssl_exporter)


Shows:

- Total number of domains in job
- History time graph of failed ssl-checks
- Table with current failed ssl-checks
- Table with all domains, certificate issuer, how much time left until certificate expiry
- Filters by job (primary for secure / insecure checks), domain (primary for history graph filter)


It is recommended to use default exporter value and verify certitificates.

Example ssl-exporter job configuration:

```yml
 - job_name: 'ssl'
    metrics_path: /probe
    scrape_interval: 2m
    static_configs:
      - targets:
        - 'my.server.com:443'
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: ssl-exporter:9219  # SSL exporter instance
```

If you have services which use not publicly verifiable certificates (i.e. internal or self-signed, services on custom port, etc)
it is better to have another scrape job `ssl-insecure` connecting to different instance of ssl-exporter container,
launched with config option __`--tls.insecure=true`__.
