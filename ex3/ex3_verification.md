# ✅ EX3 검증 — VLAN Access / PortFast

## SW1 / SW3 (공통)
```text
show vlan brief
show interfaces gi0/0 switchport
show interfaces status
show spanning-tree interface gi0/0 portfast
show spanning-tree interface gi0/1 portfast
show spanning-tree interface gi0/2 portfast
show spanning-tree interface gi0/3 portfast
show running-config interface gi0/0
```

## PC 통신 테스트
```text
! PC1 (VLAN 100)
ping 13.13.100.254       ! Gateway
ping 13.13.100.2         ! 같은 VLAN PC
ping 13.13.200.1         ! 다른 VLAN PC (Inter-VLAN)
ping 13.13.100.3         ! SW3쪽 같은 VLAN PC
```

## 🎯 합격 기준
- `show vlan brief` 에서 Gi0/0-0/1 은 VLAN 100, Gi0/2~0/3 은 VLAN 200
- 각 포트의 `Administrative Mode: static access`
- portfast 결과: **enabled**
- PC → Gateway, PC → 다른 VLAN PC 모두 ping 성공
