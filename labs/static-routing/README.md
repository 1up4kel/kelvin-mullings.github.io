# Static Routing Between HQ, Branch, and Data Center

Cisco Packet Tracer lab by Kelvin Mullings.

[Download the Packet Tracer lab](./Static_Routing_v2.pkt)

## Purpose

Practice IPv4 addressing, serial WAN connections, static routing, loopback interfaces, and connectivity troubleshooting across three routers.

## Planned topology and addressing

The table below documents the lab design. Interface names depend on the installed serial module slot; check `show ip interface brief` for the names in the saved project.

| Segment | Addresses |
| --- | --- |
| HQ LAN | 192.168.10.0/24; PC-A 192.168.10.10; gateway 192.168.10.1 |
| R1 to R2 | R1 10.0.0.1/30; R2 10.0.0.2/30 |
| Branch LAN | 192.168.20.0/24; PC-B 192.168.20.20; gateway 192.168.20.1 |
| R2 to R3 | R2 10.0.1.1/30; R3 10.0.1.2/30 |
| Data center simulation | R3 Loopback0: 203.0.113.1/32 |

Equipment: three Cisco 2911 routers, two PCs, HWIC-2T serial modules, and serial DCE/DTE connections. R1 connects to R2, and R2 connects to R3.

## Open the lab

1. Download `Static_Routing_v2.pkt`.
2. Open Cisco Packet Tracer and choose File > Open.
3. Select the downloaded file.
4. Inspect addressing and routes, then run the checks below.

## Routing design

- R1 forwards traffic for the Branch LAN, R2–R3 transit subnet, and R3 loopback to 10.0.0.2.
- R2 forwards traffic for the HQ LAN to 10.0.0.1 and the R3 loopback to 10.0.1.2.
- R3 forwards traffic for both LANs and the R1–R2 transit subnet to 10.0.1.1.
- Return routes are needed for successful communication.

## Verification checklist

Run on each router:

```text
show ip interface brief
show ip route
show running-config
```

Confirm used interfaces show up/up and the required remote routes are present.

From PC-A, open Desktop > Command Prompt:

```text
ping 192.168.10.1
ping 192.168.20.20
ping 203.0.113.1
tracert 203.0.113.1
```

From PC-B:

```text
ping 192.168.20.1
ping 192.168.10.10
ping 203.0.113.1
```

Expected outcome: both PCs can reach their gateways, each other, and R3's loopback.

These are verification instructions and expected outcomes. No successful test results from the saved project have been independently recorded here.

## Troubleshooting

- Use the actual serial interface names shown by `show ip interface brief`.
- Enable used router interfaces with `no shutdown`.
- Check the DCE end with `show controllers serial <interface-number>` and configure `clock rate 64000` there.
- Confirm both ends of each serial link use the same subnet and encapsulation.
- Check PC default gateways and static routes in both directions.
- Repeat an initial ping if ARP resolution causes an early timeout.

Save router changes with `copy running-config startup-config`, then save the Packet Tracer project.
