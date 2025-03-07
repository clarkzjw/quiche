## Generate test data on the server

Create a testing file of size 1 GB from `/dev/urandom`

```bash
dd if=/dev/urandom of=./static/test_1G bs=1M count=100
```

## Run server

```bash
QLOGDIR=./log cargo run --bin quiche-server -- --cert apps/src/bin/cert.crt --key apps/src/bin/cert.key --listen 0.0.0.0:1234 --root ./static/
```

## Run client

```bash
QLOGDIR=./log cargo run --bin quiche-client -- --no-verify https://<server_ip>:1234/test_1G --cc-algorithm reno
```

### `--cc-algorithm`

Available `--cc-algorithm` options are:

- `reno`
- `cubic`
- `bbr`
- `bbr2`

### `--disable-hystart`

More information on: [CUBIC and HyStart++ Support in quiche](https://blog.cloudflare.com/cubic-and-hystart-support-in-quiche/)

## qlog

Inspect and plot qlog files in the `log` directory ending with `.sqlog`.

