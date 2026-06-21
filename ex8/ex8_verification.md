# ✅ EX8 검증 — IP Fragment DoS 방어

## R1
```text
show access-lists ANTI_FRAG
show ip interface g1/0 | include access list
show ip interface g1/0 | include Inbound
show running-config interface g1/0
```

## 정상 통신 영향 없음 확인
```text
! 다른 장비에서 R1 Loopback으로 ping
ping 100.10.1.1
! PC에서도 정상 ping 가능해야 함
```

## Fragment 매칭 통계 (시간 경과 후)
```text
show access-lists ANTI_FRAG
clear access-list counters ANTI_FRAG    ! 통계 초기화
show access-lists ANTI_FRAG             ! 다시 확인
```

## 🎯 합격 기준
- ACL 두 줄 표시: `deny ip any host 100.10.1.1 fragments` + `permit ip any any`
- `Inbound access list is ANTI_FRAG` (g1/0에 정상 적용)
- 정상 ping(100.10.1.1) 통과
- 시간이 지나면 `permit ip any any` 의 matches 카운트 증가
- Fragment 공격 시 `deny ... fragments` matches 증가
