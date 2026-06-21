# ✅ EX5 검증 — Loopback / Routed-Interface / SVI

## R1
```text
show ip interface brief
show interfaces loopback 100
show interfaces g1/0
ping 121.160.10.2
```

## R2
```text
show ip interface brief
show interfaces loopback 200
show interfaces loopback 211
show interfaces g1/0
ping 121.160.20.2
```

## SW1
```text
show ip interface brief
show interfaces gi2/3 switchport
show running-config interface gi2/3
show running-config interface vlan 130
ping 121.160.10.1
ping 13.13.30.2
```

## SW3
```text
show ip interface brief
show interfaces gi2/3 switchport
show running-config interface gi2/3
show running-config interface vlan 130
ping 121.160.20.1
ping 13.13.30.1
```

## 🎯 합격 기준
- 모든 인터페이스 상태가 **up/up**
- SW1/SW3 의 gi2/3 에서 `Switchport: Disabled` (no switchport 적용됨)
- 직접 연결 구간 ping 성공 (R1↔SW1, R2↔SW3, SW1↔SW3 via VLAN130)
