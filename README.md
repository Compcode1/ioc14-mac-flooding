# IOC 14 – MAC Flooding (Switch CAM Table Exhaustion)

This case study provides a complete behavioral analysis of a MAC flooding attack—a stealthy Layer 2 technique that poisons a switch’s memory by overwhelming it with fake MAC addresses. The result is a fail-open condition in the switch, causing it to broadcast all unicast traffic and enabling passive traffic interception by the attacker.

## Overview

The attacker in this case is already inside the network—either through a compromised host or physical access—and uses a local NIC to launch a MAC flood. By sending thousands of spoofed MAC frames, the switch’s CAM table (Content Addressable Memory) is exhausted. With no space left to map MAC-to-port assignments, the switch broadcasts traffic indiscriminately, exposing sensitive communications to the attacker's interface.

This case examines attacker behavior, OS and OSI layer interactions, host-based indicators, and network-level response strategies using a structured forensic triage model.

## Key Concepts Covered

- The mechanics of CAM table exhaustion and its impact on Layer 2 behavior
- Host-based tool execution: how MAC flooding is launched and persisted
- Network triage techniques to detect broadcast storms and MAC instability
- Mapping attacker behavior across operating system and OSI layers
- Strategic defender actions to isolate flooding hosts and secure switches
- Follow-on attacks enabled by MAC flooding: credential theft, NTLM relay, SMB hijacking

## Investigative Structure

This project follows a forensic triage model that integrates host and network telemetry:

- **Telemetry Sources**: Packet capture sensors, EDR, NetFlow analysis
- **Primary Triage**: Host System Activity Protocol – detects attacker tool execution and persistence
- **Secondary Triage**: Network Triage Protocol – validates broadcast anomalies and port-level flooding
- **Operating System Layers**: Process execution, persistence infrastructure, background services, credential usage, monitoring evasion, and NIC manipulation
- **OSI Layers Affected**: Primarily Layer 2, with cascading effects at Layer 3, Layer 4, and Layer 7

## Tools & Techniques Observed

- `macof` (DSniff suite)
- SCAPI-based or Python-crafted MAC flooding scripts
- Raw socket access or packet driver misuse
- Registry autoruns and scheduled tasks
- Volatile memory analysis using Volatility for NIC-level activity and socket states

## Defender Response Framework

- Shut down offending switch ports and inspect CAM table behavior
- Identify flooding host via MAC/IP correlation, EDR telemetry, and DHCP leases
- Search for credential misuse (Event IDs 4624, 4625, 4648) and unauthorized tool execution (Event ID 4688)
- Analyze persistence via registry and scheduled task review
- Dump memory to detect in-memory flooding tools or raw socket activity
- Enforce port security (MAC limits, sticky bindings), ARP inspection, and DHCP snooping

## Strategic Takeaways

MAC flooding is not a standalone event—it is a quiet foothold tactic used to gain visibility in internal networks. It is often a precursor to credential-based attacks like NTLM relay, SMB hijacking, or full session interception. The attacker leverages Layer 2 trust and broadcast behavior to prepare for lateral movement. Defenders must correlate host- and network-level signals to detect and contain such low-visibility tactics before they escalate.

## Files

- `ioc14-mac-flooding.ipynb`: Full structured case study with technical, investigative, and narrative sections

## License

This project is provided for educational and research purposes. Attribution is encouraged.
