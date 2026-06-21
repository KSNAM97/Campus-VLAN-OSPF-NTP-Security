# ✅ EX1 검증 — L2 EtherChannel (PAgP / LACP)

## SW1
```text
show etherchannel summary
show etherchannel 12 port-channel
show etherchannel 12 detail
show interfaces trunk
show interfaces port-channel 12 switchport
show pagp neighbor
```

## SW2
```text
show etherchannel summary
show etherchannel 12 port-channel
show etherchannel 23 port-channel
show interfaces trunk
show pagp neighbor
show lacp neighbor
```

## SW3
```text
show etherchannel summary
show etherchannel 23 port-channel
show interfaces trunk
show lacp neighbor
show lacp 23 internal
```

## 🎯 합격 기준
- `show etherchannel summary` 에서 **Po12, Po23** 이 `SU` 상태 (S=Layer2, U=in use)
- 멤버 포트 옆 **(P)** 표시 = bundled (정상)
- `show interfaces trunk` 에서 Encapsulation: **802.1q**
- PAgP는 **desirable/desirable**, LACP는 **active/active** Neighbor 형성
