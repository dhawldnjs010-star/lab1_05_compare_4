# LAB1-05 4비트 크기 비교기 — 실험 전 레포트

- 과목: 전자전기컴퓨터설계실험Ⅱ / LAB1 조합논리 (교육 번호 05, 기존 번호 9)
- 작성자: 엄상혁 (학번 ______) / 조: ______ / 작성일: 2026-09-__
- 설계 모듈: `compare_4` / TB: `tb_compare_4` / 저장소: https://github.com/dhawldnjs010-star/lab1_05_compare_4
- 소스 커밋: `57fe948` (`57fe948e350462e2b855d6ffb6848f4a14982789`)

## 1. 실험 목적과 확인할 것

입력 조합과 출력의 관계를 **진리표·파형으로 설명**한다. 이 실험의 입출력은 입력 `a[3:0], b[3:0]` / 출력 `o[2:0] = {a>b, a==b, a<b}`.

## 2. 예상 입력·출력 (진리표, 비트 순서: MSB가 왼쪽)

| a | b | o[2]=a>b | o[1]=a==b | o[0]=a<b | o |
|---|---|---|---|---|---|
| 0101 | 0101 | 0 | 1 | 0 | 010 |
| 1001 | 0011 | 1 | 0 | 0 | 100 |
| 0011 | 1001 | 0 | 0 | 1 | 001 |
| 0000 | 0000 | 0 | 1 | 0 | 010 |
| 1111 | 0000 | 1 | 0 | 0 | 100 |
| 0000 | 1111 | 0 | 0 | 1 | 001 |

출력 `o`는 항상 원-핫이다. 256개 조합 전부 TB가 검사한다.

## 3. 직접 작성한 코드와 파일 역할

작성 파일: `src/compare_4.v`, `sim/tb_compare_4.sv`, `constraints/pins.xdc`

```verilog
// src/compare_4.v
module compare_4(
    input  wire [3:0] a,
    input  wire [3:0] b,
    output wire [2:0] o
);
    assign o = {a > b, a == b, a < b}; 
endmodule
```

**설계 설명.** 세 비교 결과를 묶어 3비트 원-핫 출력을 만든다. 세 비트 중 정확히 하나만 1이 된다.

| 파일 | 역할 |
|---|---|
| `src/*.v` | 설계(RTL). 합성 대상이며 TB를 넣지 않는다. |
| `sim/tb_compare_4.sv` | DUT 연결, 입력 자극, 예상값 검사(`$fatal`), `wave.vcd` 덤프(`$dumpfile`·`$dumpvars`), `$finish`. 입력을 10 ns 간격으로 바꾸며 256개 조합을 검사한다. |
| `constraints/pins.xdc` | 보드 핀 배정(PACKAGE_PIN·IOSTANDARD·get_ports). Icarus 기능 시뮬레이션의 입력이 아니다. |
| `simulation.json` | `sources`, `testbench`(`sim/tb_compare_4.sv`), `simulation_top`(`tb_compare_4`) 지정. |

## 4. 핀 제약(XDC) 설명과 상태 — **손상·불완전**

문법에 맞지 않는 줄: `set_property IOSTANDARD LVCMOS33 [get_ports`; 핀/IOSTANDARD 누락 포트: b[3], b[2], b[1], b[0], o[2], o[1], o[0]

| 포트 | PACKAGE_PIN | IOSTANDARD |
|---|---|---|
| a[3] | Y1 | LVCMOS33 |
| a[2] | W3 | LVCMOS33 |
| a[1] | U2 | LVCMOS33 |
| a[0] | T1 | LVCMOS33 |
| b[3] | W4 | (없음) |
| b[2] | W1 | (없음) |
| b[1] | V4 | (없음) |
| b[0] | U4 | (없음) |
| o[2] | L4 | (없음) |
| o[1] | M4 | (없음) |
| o[0] | M2 | (없음) |

- `get_ports`의 이름·대괄호 표기가 RTL 포트명과 일치해야 한다. XDC는 시뮬레이션이 검증하지 않으므로 핀 배정은 Vivado 구현·보드에서 별도로 확인한다.

## 5. 사전 시뮬레이션 결과 (VS Code + Icarus Verilog)

| 항목 | 결과 |
|---|---|
| 콘솔 PASS 문구 | `LAB1_PASS compare_4 cases=256` |
| 검사한 입력 조합 수 | 256개 (진리표 전 조합 검사) |
| 종료 시각 | 2560 ns (`$finish`) |
| 로그·파형 | `evidence/simulation.txt`, `evidence/wave.vcd` (저장소에 커밋됨) |
| 파형 캡처 | ______ (VaporView에서 입력·출력 확대 후 캡처, `evidence/`에 저장) |

> `LAB1_PASS`는 TB가 계산한 기대값과 출력이 전부 일치했다는 자기검사 결과이다. TB의 기대식이 설계와 같은 관점으로 쓰였는지는 위 진리표와 파형으로 직접 대조해 설명한다.

- 파형에서 확인한 대표 구간(시간 · 입력 → 출력): ______
- 진리표와 어긋난 부분과 원인: ______

## 6. 수정 전후 결과 (실패 → 복구 실험)

| 단계 | 내용 |
|---|---|
| 정상 | 위 5절의 PASS 로그를 보관 |
| 수정 제안 | `{a > b, a == b, a < b}` → `{a < b, a == b, a > b}` |
| 기대되는 실패 | a≠b 행에서 o[2]와 o[0]이 뒤바뀜 — 실패 로그에서 vector·expected·actual 확인 |
| 실제 실패 로그 | ______ (`build/sim/run-.../simulation.log`) |
| 복구 후 | 원래대로 복구, Save All → `02 Simulate` → PASS 재확인: ☐ |
| 변경 이유·원인·복구 결과 | ______ |

## 7. 실험 당일 보드 확인 계획

- 장비: Spartan-7 XC7S75 교육용 보드(part `xc7s75fgga484-1`). 연결 전 전원·핀 기능·I/O 전압(LVCMOS33)을 확인하고, 배선 변경은 전원을 끈 상태에서 한다.
- 확인 계획: a>b, a==b, a<b 세 경우에서 LED 3개 중 정확히 하나만 켜지는지 확인한다.
- 조합회로라 클록이 없다. TB의 10 ns 간격은 검사를 빠르게 하기 위한 값이며, 보드에서는 스위치를 손으로 바꾸므로 **입력 조합**으로만 동작을 해석한다.
- 사진에는 보드 연결과 입력·출력 위치가 함께 보이게 촬영한다.
- 조교가 무작위로 고른 2개 실험 번호는 출석부에 기록된다: ☐ 선정됨 ☐ 시연 완료

## 8. 제출 점검

- [x] 진리표(2절), 코드·XDC 설명(3·4절), 사전 시뮬레이션 로그(5절)
- [ ] 사전 파형 캡처(5절), 수정 전후 실험(6절)
- [ ] `reports/pre/`에 저장 후 commit·push
