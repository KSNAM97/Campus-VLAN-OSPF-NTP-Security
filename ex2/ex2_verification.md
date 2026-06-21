# ✅ EX2 검증 — VTP / STP Root-Bridge

## SW1 (Server / Root)
```text
show vtp status
show vtp password
show vlan brief
show spanning-tree vlan 100
show spanning-tree vlan 200
show spanning-tree vlan 130
show spanning-tree root
show ip interface brief | include Vlan
```

## SW2 (Client)
```text
show vtp status
show vlan brief
```

## SW3 (Client)
```text
show vtp status
show vlan brief
```

## 🎯 합격 기준
- SW1: `VTP Operating Mode: Server`, SW2/SW3: `Client`
- 모든 SW에서 `VTP Domain Name: ccnp` 동일
- `show vlan brief` 에 VLAN 100(CISCO_CCNA), 130, 200(CISCO_CCNP) 존재
- SW1이 모든 VLAN의 Root Bridge (`This bridge is the root` 표시)
