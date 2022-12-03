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

# Setup Prometheus

## 1. move / Rename
```sh
mv prometheus-2.40.5.linux-amd64 prometheus-files

```

## 2.  Create a Prometheus user, required directories, and make Prometheus the user as the owner of those directories.
```sh
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus

```

## 3. Copy prometheus and promtool binary from prometheus-files folder to /usr/local/bin and change the ownership to prometheus user.
```sh
sudo cp prometheus-files/prometheus /usr/local/bin/
sudo cp prometheus-files/promtool /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool

```

## 4. Move the consoles and console_libraries directories from prometheus-files to /etc/prometheus folder and change the ownership to prometheus user.

```sh
sudo cp -r prometheus-files/consoles /etc/prometheus
sudo cp -r prometheus-files/console_libraries /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries

```

> Notes: After setup All the prometheus configurations should be present in /etc/prometheus/prometheus.yml file.

## 5. Create the prometheus.yml file.
```sh
sudo vi /etc/prometheus/prometheus.yml
```

## 6. Copy the following contents to the prometheus.yml file.
```sh
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']

```

## 7. Change the ownership of the file to prometheus user.
```sh
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml

```


# Setup Prometheus Service File
## Step 1: Create a prometheus service file.

```sh
sudo vi /etc/systemd/system/prometheus.service

```

## Step 2: Copy the following content to the file.
```sh
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

## Step 3: Reload the systemd service to register the prometheus service and start the prometheus service.

```sh
sudo systemctl daemon-reload
sudo systemctl start prometheus

```

## Check the prometheus service status using the following command.
```sh
sudo systemctl status prometheus

```

![image prometheus service](https://github.com/alfayz-tv/doc/blob/master/images/prometheus.png)

## sudo systemctl status prometheus
> Now you will be able to access the prometheus UI on 9090 port of the prometheus server.

```sh
http://<prometheus-ip>:9090/graph

```
You should be able to see the following UI as shown below.

![image prometheus web ui](https://github.com/alfayz-tv/doc/blob/master/images/prometheus_web.png)