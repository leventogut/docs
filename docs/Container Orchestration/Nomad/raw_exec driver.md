---
title: Using raw_exec driver
summary: Using raw_exec driver.
tags:
  - nomad
  - exec
  - hashicorp
  - hashi
---
## Installation

[Nomad docs](https://learn.hashicorp.com/tutorials/nomad/get-started-install?in=nomad/get-started)


Full configuration options can be found at https://www.nomadproject.io/docs/configuration

## Sample config

```hcl
data_dir = "/opt/nomad/data"
bind_addr = "0.0.0.0"
datacenter = "dc1"
advertise {
  http = "x.x.x.x"
  rpc  = "x.x.x.x"
  serf = "x.x.x.x"
}

server {
  enabled = true
  bootstrap_expect = 1
}

client {
  enabled = true
  servers = ["127.0.0.1:4646"]
}

plugin "raw_exec" {
  config {
    enabled = true
  }
}

server_join {
  retry_join = [ "y.y.y.y", "z.z.z.z" ]
  retry_max = 3
  retry_interval = "15s"
}
```

Job Configuration

```hcl
job "drouter-xxx-processor-job" {
  datacenters = ["dc1"]

  group "xxx" {
    count = "5"

    task "xxx" {
      driver = "raw_exec"

      config {
        command = "/opt/drouter/xxx_processor.py"

        args = [
          "-vv",
          "--amqp-host=",
          "--amqp-user=",
          "--amqp-pass=",
          "--amqp-qname=",
          "--amqp-rkey=",
          "--amqp-xchg="
        ]
      }
    }
  }
}
```

## References & further reading

- [Nomad project](https://www.nomadproject.io/)
- [Raw Fork/Exec Driver](https://www.nomadproject.io/docs/drivers/raw_exec)
