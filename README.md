# Prometheus SNMP Exporter Modules

Some files I made to work as modules to add to the [Prometheus SNMP Exporter] configuration file.

This repository was strongly inspired in the [Kyle Wooten]'s "[iDrac Prometheus SNMP Module]".

I included some metrics to their original iDrac module and built my own files for Fortinet Fortigate and HP Proliant.

These files won't win any prize in the [Yaml Lint] test, but it works.

## Equipments

The modules were tested with the follow equipments:

```
Fortigate 1500D
Dell iDrac
HP Proliant BL460c (Gen8, Gen10)
```

## How to use these Modules

1. Add the chosen file as a block in your snmp.yml file.
2. Change the field "community: yourveryowncommunity" adding your very own snmp community.
3. See down below how to configure the respective module in your Prometheus configuration file.

## Numbers and Status

### Fortigate OID Number an Status
```
0. False (OK, Up and Running with no warning or error)
1. True
```

### Dell OID Number and Status
```
1. Other
2. Unknown
3. OK
4. Non-critical
5. Critical
6. Non-recoverable
```

### HP OID Number and Status
```
1. Unknown
2. Running
3. Warning
4. Testing
5. Down
```

## Prometheus Configuration

Add the jobs below to your Prometheus Configuration File to scrape the respective equipment metrics

### Fortinet Fortigate

```
- job_name: 'fortigate'
    static_configs:
      - targets:
        - your.host.com  # SNMP device.
    metrics_path: '/snmp'
    params:
      module: [fortinet_fortigate]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9116 # Change here with your real exporter address:port
```

### Dell iDrac

```
  - job_name: 'idrac'
    static_configs:
      - targets:
        - your.host.com  # SNMP device.
    metrics_path: '/snmp'
    params:
      module: [dell_idrac]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9116 # Change here with your real exporter address:port
```

### HP Proliant

```
  - job_name: 'hp_proliant'
    static_configs:
      - targets:
        - your.host.com  # SNMP device.
    metrics_path: '/snmp'
    params:
      module: [hp_proliant]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9116 # Change here with your real exporter address:port
```

[//]: #
[Prometheus SNMP Exporter]: https://github.com/prometheus/snmp_exporter
[Kyle Wooten]: https://github.com/billykwooten
[iDrac Prometheus SNMP Module]: https://github.com/billykwooten/idrac_promethus_snmp_module
[Yaml Lint]: https://github.com/adrienverge/yamllint
