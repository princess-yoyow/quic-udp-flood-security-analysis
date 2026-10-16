# Evaluating the Impact of UDP Flood Attacks on QUIC and Firewall-Based Mitigation

> A controlled Mininet lab exploring how UDP flood traffic affects a QUIC/HTTP3 server and how an `iptables` firewall changes observed attack traffic.

## Project Overview

QUIC is a UDP-based transport protocol that supports HTTP/3. This project implements a small QUIC test network using **Mininet** and **aioquic**, captures baseline traffic, exercises UDP flood scenarios in an isolated lab, and compares packet captures before and after firewall mitigation.

**Scope:** Educational, controlled lab experimentation. All testing was conducted against simulated hosts in the Mininet topology.

## Objectives

- Build a functional QUIC client/server network in Mininet.
- Capture and inspect normal QUIC communication in Wireshark.
- Compare UDP flood scenarios by attack intensity and payload length.
- Apply firewall-based mitigation with `iptables` and observe the resulting traffic.

## Lab Architecture

| Host | Role | Address |
|---|---|---|
| `h1` | QUIC/HTTP3 server | `10.0.0.1` |
| `h2` | QUIC client | `10.0.0.2` |
| `h3` | UDP flood test host | `10.0.0.3` |

The custom topology also includes Mininet switches (`s3` and `s4`). The slide deck names the topology file `topo2.py` and the topology `mytopo`. QUIC testing targeted UDP port **4433**.

## Tools and Technologies

`Mininet` · `aioquic` · `Python` · `Wireshark` · `hping3` · `iptables` · `Linux`

## Experimental Method

1. **Network setup:** Launch the custom Mininet topology and verify inter-host connectivity.
2. **QUIC baseline:** Run an `aioquic` server on `h1` and client on `h2`; capture QUIC Initial, Handshake, and encrypted payload traffic.
3. **UDP flood tests:** Generate controlled traffic from `h3` to `h1` on UDP port 4433. The slides describe approximately 10-second runs with three intensity levels and three payload sizes.
4. **Firewall mitigation:** Apply `iptables` filtering, then compare packet captures before and after the mitigation.

## Results

### Intensity variation (~10 seconds per scenario)

| Scenario | Intensity | Captured packets |
|---|---|---:|
| A1 | Low | 515 |
| A2 | Medium | 1,823 |
| A3 | High | 314,555 |

### Payload variation

| Scenario | Payload size | Captured packets |
|---|---|---:|
| B1 | Small (±10 bytes) | 2,085 |
| B2 | Medium (±200 bytes) | 1,969 |
| B3 | Large (±800 bytes) | 1,755 |

### Before and after firewall

| Observation | Result reported in the presentation |
|---|---|
| Before mitigation | UDP flood traffic observed in packet captures |
| After mitigation | No attack packets captured during the observed test |

**Interpretation:** Within the tested configuration, firewall filtering prevented attack traffic from appearing in the post-mitigation capture. This is a lab observation, not a claim of comprehensive DoS protection. The presentation also notes the limitations of static firewall rules.

## Documentation

- [Presentation and experimental evidence (PDF)](docs/QUIC_UDP_Flood_Presentation.pdf)

The slides include the Mininet topology, QUIC setup, Wireshark captures, attack scenarios, firewall screenshots, and summary results.

## Limitations

- Tests were conducted in a simulated Mininet environment, not on a production network.
- The supplied presentation reports packet capture results but does not provide complete reproducible scripts, detailed performance measurements, or independent service-availability metrics.
- The mitigation is based on manually configured static firewall rules; effectiveness beyond the tested scenarios has not been established.

## Repository Roadmap

- [ ] Add the original sanitized Mininet topology and experiment scripts.
- [ ] Add sanitized packet-capture samples or selected screenshots.
- [ ] Document exact firewall rules and the capture vantage point once verified from original lab artifacts.
- [ ] Add step-by-step reproduction instructions after scripts are checked.

## Author

**Selvi Melani**  
Cybersecurity / Network Security Project

---

*Only run security traffic experiments on systems and lab networks you own or are explicitly authorized to test.*
