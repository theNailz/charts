# My charts

## How to use

```bash
# Add the repo
helm repo add thenailz https://thenailz.github.io/charts/

# List the charts
helm search repo thenailz
```

### Create your values.yaml

```yaml
docker-readsb-protobuf:
  # Enable this if you want a web frontend. If you do not want the web frontend, remove the entire "ingress:" block with all children.
  ingress:
    enabled: true
    hosts:
      - host: my.cluster.ingress.address
        paths:
          - path: /
            pathType: ImplementationSpecific

  # Select your k8s node with the RTL-SDR attached.
  nodeSelector: 
    kubernetes.io/hostname: my-node-2

  readsb:
    timezone: "Europe/Amsterdam"
    latitude: "12.3456"
    longitude: "1.2345"
```

Then install the chart:

```bash
helm install -n <namespace> readsb -f values.yaml thenailz/docker-readsb-protobuf
```

### Logs

You can check the logs with `kubectl logs -f readsb-docker-readsb-protobuf-0`.

## Common errors

### `[readsb] 2023/05/15 13:43:27 rtlsdr: no supported devices found.`

The RTL-SDR device was not found. Check if the pod landed on the correct node.

### `[error] setmntent (/etc/mtab): No such file or directory`
### `[error] df plugin: cu_mount_getlist failed.`

Not sure why this happens. The map works fine with these errors. Can be ignored.

### `[error] table plugin: Failed to open file "/sys/class/thermal/thermal_zone0/temp": No such file or directory.`

Collectd can't find the temperature sensor. Might be because you're on hardware that doesn't have (that) sensor. Can be ignored.

