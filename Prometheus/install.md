# Install Prometheus
## 1. Download prometheus
```sh
https://prometheus.io/download/
```

## 2. Extract
```sh
tar xvfz prometheus-*.tar.gz
```

## 3. To Folder
```sh
cd prometheus-*
```

## 4. Configure Prometheus
```
nano prometheus.yml
```

## 5. Start Prometheus
```sh
# Start Prometheus.
# By default, Prometheus stores its database in ./data (flag --storage.tsdb.path).
./prometheus --config.file=prometheus.yml

```

## 6. Go to Browser
```sh
localhost:9090/metrics
```