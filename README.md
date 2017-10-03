# version_exporter

Exports the expiration time of your domains as prometheus metrics.

## Running

```console
./version_exporter -b ":9222"
```

Or with docker:

```console
docker run -p 9222:9222 caarlos0/version_exporter
```

## Configuration

On the prometheus settings, add the domain_expoter prober:

```yaml
- job_name: domain
  scrape_interval: 2h
  metrics_path: /probe
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: domain
    - target_label: __address__
      replacement: localhost:9222 # version_exporter address
  static_configs:
    - targets:
      - prometheus/prometheus
      - goreleaser/goreleaser
      - caarlos0/version_exporter
```

It works more or less like prometheus's
[blackbox_exporter](https://github.com/prometheus/blackbox_exporter).

Alerting rules example:

```rules
// TODO
```

## Building locally

Install the needed tooling and libs:

```console
make setup
```

Run with:

```console
go run main.go
```

Run tests with:

```console
make test
```
