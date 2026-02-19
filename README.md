# NetProbe - ICMP Ping & Traceroute Tool

NetProbe is a Python-based networking utility that implements ICMP Ping and a simplified Traceroute tool using raw sockets. Unlike high-level ping/traceroute implementations that rely on built-in system commands, NetProbe constructs and sends ICMP packets directly, allowing full control over packet structure, checksum validation, TTL behavior, and ICMP response parsing.

This project demonstrates a practical understanding of low-level networking concepts including ICMP message formatting, raw socket communication, round-trip time measurement, and hop-by-hop routing behavior.

NetProbe is designed to simulate real diagnostic tools used in network troubleshooting. It is capable of measuring packet latency, detecting unreachable hosts, identifying routing hops, and reporting key statistics that help evaluate network performance and reliability.

Key features include:
- ICMP Echo Request / Echo Reply (Ping) implementation
- Manual ICMP checksum calculation using 16-bit 1’s complement arithmetic
- ICMP packet creation using byte-level struct packing
- Packet validation using identifier, sequence number, and raw payload matching
- RTT (round-trip time) reporting for each successful reply
- Aggregated ping statistics including:
  - minimum RTT
  - maximum RTT
  - average RTT
  - packet loss percentage
- Traceroute functionality by incrementing TTL values per request to reveal intermediate routers
- Handling of common ICMP response types such as:
  - Echo Reply (Type 0)
  - Destination Unreachable (Type 3)
  - Time Exceeded (Type 11)
- Uses select()-based timeout handling to prevent blocking socket behavior
- Displays ICMP type/code feedback to help interpret failures and routing issues

NetProbe’s traceroute implementation works by repeatedly sending ICMP Echo Requests while increasing the TTL value starting from 1. Each router that decrements TTL to zero responds with an ICMP Time Exceeded message, allowing the program to reveal the path toward the destination host. The traceroute process stops once the destination returns an ICMP Echo Reply or the maximum TTL threshold is reached.

To run the program, Python 3 is required. Because NetProbe uses raw ICMP sockets, administrator/root privileges are also required.

To perform a ping test, uncomment a ping call inside `main()`:

```python
icmpHelperPing.sendPing("www.google.com")
```

To perform a traceroute, uncomment a traceroute call inside `main()`:

```python
icmpHelperPing.traceRoute("www.google.com")
```

Then run:

```bash
sudo python3 NetProbe.py
```

The program output includes TTL (hop count), RTT in milliseconds, ICMP type/code values, intermediate router or destination IP addresses, and final ping statistics such as min/max/average RTT and packet loss percentage.

Credits: Inspired by networking concepts from *Computer Networking: A Top-Down Approach (8th Edition)* and ICMP type/code references from IANA.
