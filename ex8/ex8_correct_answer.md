# ✅ EX8 검증 — IP Fragment DoS 방어

> **결과 요약** — ANTI_FRAG ACL 2줄(deny fragments + permit) 정상, g1/0 inbound 적용, 정상 ping 100% 통과, permit 카운터 증가·clear 정상 동작.

---

## R1

### show access-lists ANTI_FRAG
```text
Extended IP access list ANTI_FRAG
    10 deny ip any host 100.10.1.1 fragments
    20 permit ip any any (1 match)
```
✅ 2줄 구성: `deny ... fragments` + `permit ip any any`
✅ permit 라인 match 발생 → 정상 트래픽 통과 중

### show ip interface g1/0 | include access list
```text
  Outgoing access list is not set
  Inbound  access list is ANTI_FRAG
```
✅ Inbound 방향에 ANTI_FRAG 적용 (in 방향)

### show ip interface g1/0 | include Inbound
```text
  Inbound  access list is ANTI_FRAG
```
✅ Inbound = ANTI_FRAG 확인

### show running-config interface g1/0
```text
interface GigabitEthernet1/0
 ip address 121.160.10.1 255.255.255.0
 ip access-group ANTI_FRAG in
 negotiation auto
```
✅ `ip access-group ANTI_FRAG in` 정상 적용

---

## 정상 통신 영향 없음 확인

### SW1 → 100.10.1.1 (R1 Loopback)
```text
Sending 5, 100-byte ICMP Echos to 100.10.1.1:
!!!!!
Success rate is 100 percent (5/5)
```
✅ 단편화되지 않은 정상 ICMP는 `permit ip any any`로 통과 → DoS 차단이 정상 통신에 영향 없음

---

## Fragment 매칭 통계 (시간 경과 / clear 동작)

### 시간 경과 후
```text
Extended IP access list ANTI_FRAG
    10 deny ip any host 100.10.1.1 fragments
    20 permit ip any any (22 matches)
```
✅ 정상 트래픽 누적으로 permit 카운터 증가 (1 → 22)

### clear access-list counters ANTI_FRAG → 재확인
```text
Extended IP access list ANTI_FRAG
    10 deny ip any host 100.10.1.1 fragments
    20 permit ip any any (1 match)
```
✅ 카운터 초기화 정상 동작 (clear 후 새 트래픽 1건 카운트)

---

## 🎯 합격 기준 체크

| 항목 | 기준 | 결과 |
| --- | --- | --- |
| ACL 2줄 구성 | deny fragments + permit any | ✅ |
| 적용 방향 | g1/0 Inbound | ✅ |
| 정상 ping | 100.10.1.1 통과(100%) | ✅ |
| permit 카운터 | 시간 경과 시 증가 | ✅ |
| clear 동작 | 카운터 초기화 정상 | ✅ |

> **EX8 PASS** ✅ — IP Fragment DoS 방어 ACL 구성·적용·정상통신 보존·카운터 동작 모두 정상

---

## 📌 참고 메모
- `deny ip any host 100.10.1.1 fragments` 는 **2번째 이후 단편(non-initial fragment)** 만 매칭하여 차단함. 정상 통신의 첫 패킷(initial fragment / non-fragment)은 permit으로 통과하므로 일반 트래픽에 영향 없음.
- 실제 fragment 공격을 재현하려면 큰 페이로드로 단편화를 유발(예: `ping 100.10.1.1 size 2000 df-bit` 미설정 후 대용량 전송)했을 때 `deny ... fragments` 의 match 카운트가 증가하는 것으로 확인 가능.
- `permit ip any any (n matches)` 카운터 증가는 정상 트래픽이 ACL을 거쳐 통과하고 있다는 증거로, ACL이 인터페이스에서 실제 평가되고 있음을 의미.
