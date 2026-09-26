# Local Mesh Transport Isolation Protocol (LMTI-v1.0)

**Classification:** Open Standard / Peer-to-Peer Transport Hardening  
**Canonical Reference:** `LMTI-v1.0`  
**Target Infrastructure:** Autonomous Edge Nodes, Local-First Models, Bare-Metal Compute  

---

## 1. System Topology

[ ATN Task Graph ] ──► [ Local AST Engine ]
│
▼
┌─────────────────────────────────────┐
│ LMTI Transport Layer │
│ (Ephemeral MAC + Padding Engine) │
└──────────────────┬──────────────────┘
│
┌─────────────────────────┼─────────────────────────┐
│ (Noise Protocol) │ (Onion Routing) │ (Direct LAN)
▼ ▼ ▼
[ Node A ] [ Node B ] [ Node C ]
WireGuard / Yggdrasil Tor v3 Hidden Service mDNS / Local Mesh
(Peer Public Key) (.onion Address) (Subnet Isolation)


---

## 2. Core Protocol Invariants

1. **Zero Central Name Resolution:** No public DNS calls (`A`/`AAAA` records). Nodes address peers exclusively via public key cryptographic identity (e.g., WireGuard public keys, Tor v3 `.onion` addresses, or Yggdrasil IPv6 addresses derived from public keys).
2. **End-to-End Payload Encryption:** All state handoffs between nodes execute over Noise Protocol framework channels or authenticated Curve25519 tunnels.
3. **Packet Equalization & Latency Fuzzing:** Outbound packet sizes are padded to uniform 1024-byte blocks, and egress timing is jittered by $\pm 15\text{ms}$ to invalidate router side-channel traffic analysis.
4. **Local Network Autonomy (mDNS/LAN):** Nodes on the same physical subnet or local mesh identify each other via encrypted local multicast without hitting the public internet.

---

## 3. Reference Implementation

- Transport Shield Verification Engine: `proofs/transport_shield.py`
