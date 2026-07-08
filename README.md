# Peekyport

JavaFX desktop app for mapping a home network, inspecting discovered devices, and surfacing basic safety risks. 

<img width="400" height="400" alt="smallpeeky" src="https://github.com/user-attachments/assets/26e6290c-26d5-4c9a-818a-a4669567eecb" />


## What the current version does

- Scans active local IPv4 interfaces and sweeps the likely home-LAN range
- Discovers reachable devices using reachability, ARP data, and common service ports
- Offers an explicit opt-in Nmap deep scan that uses a local Nmap install for broader host discovery and top-port scanning
- Shows a draggable topology-style map centered on the gateway or current host, now kept device-focused for easier scanning. Dragging a node pins it in place; unpinned nodes are laid out automatically and flow around your pinned nodes without disturbing them. Right-click a pinned node to release it back to automatic layout.
- Infers logical links such as default-route uplinks, Wi-Fi paths, host-to-service relationships, and likely peer-device service paths
- Lets you toggle IP labels and manually customize node labels, types, uploaded images, floor numbers, room names, cluster/group assignments, shapes, and per-node background/border styling from the details panel
- Includes a Safety tab with a clickable severity pie chart that drills into affected devices and the findings behind each slice
- Includes a Web Traffic tab that imports CSV traffic data or Wireshark `.pcap/.pcapng` captures, enriches them from current/saved scans, and summarizes them by device, device type, and traffic category
- Includes a History tab that archives scans locally and compares device, risk, and logical-link changes over time for one router/gateway location
- Includes an Unknown Device Investigator in the details panel that can probe local clues like MAC hints, web titles, TLS names, ports, and protocols
- Includes a Router tab for saved Netgear settings and a basic connection test against common web/SNMP surfaces
- Includes a Documentation tab with a built-in usage guide, searchable help topics, and section-based walkthroughs
- Includes a dedicated Links tab with logical-link filters, sortable inferred-activity breakdowns, and a connection-only graph for the currently visible links
- Lets you zoom and pan around both graphical views so dense device and link maps stay explorable
- Lists device names, IPs, MACs, inferred device types, detected protocols, and open common ports
- Flags common risks such as Telnet, FTP, exposed SMB, remote desktop, and gateway admin surfaces
- Reads host Wi-Fi context on macOS when available and highlights weak Wi-Fi security

<img width="400" height="auto" alt="Screenshot 2026-07-07 184930" src="https://github.com/user-attachments/assets/58e4c412-fc30-4648-b27f-ac4d04f0b2b0" />

<img width="400" height="auto" alt="Screenshot 2026-07-07 184841" src="https://github.com/user-attachments/assets/8c04582d-a3bf-4c0e-98d7-5e6593fee1b2" />



## Important limitations

- Physical distance between remote devices cannot be measured directly from a normal desktop app. The app currently shows **logical proximity** and, for the current machine only, host-side Wi-Fi signal strength.
- Connection lines are **logical relationships inferred from scan data and device-role heuristics**, not guaranteed physical switch-port or RF paths.
- Manual node customizations and the IP-label toggle persist locally in `~/.net-map-desktop/settings.properties` and are keyed by IP address.
- When devices have floor/room assignments, the topology switches into floor bands with room clusters so the map better reflects the home layout.
- Devices can also be assigned to named clusters with a custom highlight color; topology and links render those groups as soft bounded regions with faint group labels.
- Router settings and scan-archive preferences are also stored in `~/.net-map-desktop/settings.properties`. Saved router/SNMP credentials are still local convenience data (not encrypted at rest), but the settings file and its parent folder are now created with owner-only permissions (`rw-------` / `rwx------`) on POSIX systems so other local accounts cannot read them. On Windows the per-user home-directory ACLs apply.
- Archived scans are written as local `.properties` snapshots in the chosen scan-history folder so the History tab can compare one router location over time.
- Topology link activity is inferred from discovered ports, protocols, and uplink roles; it is not packet-capture throughput telemetry.
- The Unknown Device Investigator uses safe local evidence sources and a small built-in hint set; it is not a full vendor-OUI database or deep fingerprinting engine yet.
- The Web Traffic tab can import Wireshark captures through local `tshark`. If `tshark` is not installed or not on your PATH, direct capture import will fail with a clear message.
- The Nmap deep scan depends on a local `nmap` install. The app checks PATH plus common install locations, and you can also provide a custom Nmap path in the header field.
- When saved scans are available, Web Traffic imports use that archive history as a fallback source for device labels and types, with preference given to the current router/location.
- The Web Traffic tab also supports CSV summaries. A practical header set is: `deviceIp,deviceName,deviceType,category,bytesIn,bytesOut,requests`.
- The scan intentionally stays conservative and home-network friendly. For large subnets, it narrows to a `/24`-sized sweep around the active interface.
- Wi-Fi details are OS-dependent. The current implementation reads macOS Wi-Fi telemetry through the built-in `airport` tooling when available.
- This version does not yet do packet capture, SNMP polling, vendor OUI lookup, or time-series traffic telemetry.

