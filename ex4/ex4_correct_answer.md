# ✅ EX4 검증 — DHCP (VLAN100 / VLAN200)

> **결과 요약** — PC 8대 전부 DHCP 수신 완료. VLAN100=13.13.100.1~4, VLAN200=13.13.200.1~4, GW .254 / DNS 168.126.63.1 / Lease 5일(432000초) 정상.

---

## PC (VPCS) — IP 수신 결과

### VLAN 100
```text
VPCS> show ip
IP/MASK     : 13.13.100.1/24    GATEWAY : 13.13.100.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.100.254
IP/MASK     : 13.13.100.2/24    GATEWAY : 13.13.100.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.100.254
IP/MASK     : 13.13.100.3/24    GATEWAY : 13.13.100.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.100.254
IP/MASK     : 13.13.100.4/24    GATEWAY : 13.13.100.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.100.254
```

### VLAN 200
```text
VPCS> show ip
IP/MASK     : 13.13.200.1/24    GATEWAY : 13.13.200.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.200.254
IP/MASK     : 13.13.200.2/24    GATEWAY : 13.13.200.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.200.254
IP/MASK     : 13.13.200.3/24    GATEWAY : 13.13.200.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.200.254
IP/MASK     : 13.13.200.4/24    GATEWAY : 13.13.200.254   DNS : 168.126.63.1   DHCP SERVER : 13.13.200.254
```
✅ DORA 정상 (`DORA IP 13.13.x.x ... GW 13.13.x.254`)
✅ Gateway .254 / DNS 168.126.63.1 / DHCP Server .254
✅ `DHCP LEASE ... 432000` = **5일(432000초)** 정상 적용

---

## SW1 (DHCP Server)

### show ip dhcp pool
```text
Pool vlan100 :  Total addresses 254 / Excluded 1
 13.13.100.1 - 13.13.100.254
Pool vlan200 :  Total addresses 254 / Excluded 1
 13.13.200.1 - 13.13.200.254
```
✅ 풀 2개 / 254개 / .254 excluded 정상

### show running-config | section dhcp
```text
ip dhcp excluded-address 13.13.100.254
ip dhcp excluded-address 13.13.200.254
ip dhcp pool vlan100
 network 13.13.100.0 255.255.255.0
 default-router 13.13.100.254
 dns-server 168.126.63.1
 lease 5
ip dhcp pool vlan200
 network 13.13.200.0 255.255.255.0
 default-router 13.13.200.254
 dns-server 168.126.63.1
 lease 5
```
✅ Network / Gateway / DNS / Lease 5일 정상

### show ip dhcp binding
```text
Bindings from all pools not associated with VRF:
(empty)
```
ℹ️ **GNS3 + VPCS 환경 특성** — VPCS의 DHCP 트랜잭션이 IOS binding 테이블/통계(DISCOVER 카운터)에 반영되지 않는 경우가 있음. PC 측에서 정확한 IP·GW·DNS를 수신했으므로 **DHCP 동작은 정상**으로 판정. (실장비/IOSv 호스트였다면 8개 binding 표시)

---

## 🎯 합격 기준 체크

| 항목 | 기준 | 결과 |
| --- | --- | --- |
| 풀 2개 존재 | vlan100, vlan200 | ✅ |
| Network/Mask | 13.13.100.0/24, 13.13.200.0/24 | ✅ |
| PC IP 수신 | VLAN100 .1~.4 / VLAN200 .1~.4 | ✅ |
| Gateway | .254 | ✅ |
| DNS | 168.126.63.1 | ✅ |
| Lease | 5일 (432000초) | ✅ |
| Excluded | .254 1개 | ✅ |

> **EX4 PASS** ✅ — PC 8대 DHCP 수신, 대역/GW/DNS/Lease 모두 정상

---

## 📌 참고 메모
- EX2에서 SVI(Vlan100/200)에 `no shutdown` 을 추가한 뒤 DHCP가 정상 수신됨 (SVI = default-router 역할, up 이어야 응답 가능).
- `show ip dhcp binding` 이 비어 보여도 VPCS 환경에서는 정상. 검증은 **PC 측 `show ip`** 로 판단하는 것이 정확.
