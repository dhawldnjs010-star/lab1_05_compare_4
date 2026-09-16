# LAB1-05 4비트 크기 비교기 — 실험 후 레포트

- 과목: 전자전기컴퓨터설계실험Ⅱ / LAB1 조합논리 (교육 번호 05, 기존 번호 9)
- 작성자: 엄상혁 (학번 ______) / 조: ______ / 실험일: 2026-09-14 / 작성일: 2026-09-__

> 미수행·미확인 항목은 **미완료**로 표시했고 후속 확인을 적었다. bit 파일 생성만으로 보드 동작 성공을 선언하지 않는다.

## 1. 구현 환경 기록

| 항목 | 기록 |
|---|---|
| Vivado | 2026.1 (win64, Build 6511674) |
| part | xc7s75fgga484-1 (Spartan-7, fgga484, speed -1) |
| 설계 top | `compare_4` (Design Sources) |
| 시뮬레이션 top | `tb_compare_4` (`sim/tb_compare_4.sv`) |
| 핀 제약 | `constraints/pins.xdc`, IOSTANDARD LVCMOS33 |
| 소스 커밋 | `57fe948` — https://github.com/dhawldnjs010-star/lab1_05_compare_4/commit/57fe948e350462e2b855d6ffb6848f4a14982789 |
| Vivado 프로젝트 | 없음 |

## 2. VS Code(Icarus)와 Vivado 시뮬레이션 비교

두 실행은 같은 RTL과 같은 자기검사 TB(`tb_compare_4`)를 사용했다.

| 비교 항목 | VS Code (Icarus) | Vivado (xsim) |
|---|---|---|
| PASS 문구 | `LAB1_PASS compare_4 cases=256` | 미수행 |
| 검사 수 | 256개 | - |
| 종료 시각 | 2560 ns | - |
| 로그 위치 | `evidence/simulation.txt` | `-` |
| 입력·출력 | 진리표와 일치 (사전 레포트 2절) | - |

일치 여부와 차이 원인: ______

## 3. 합성·구현·비트스트림

| 단계 | 결과 |
|---|---|
| Vivado 프로젝트 | **없음** — 이 폴더에는 Vivado 프로젝트(`.xpr`)와 실행 기록이 없다. **미수행** |



현재 `constraints/pins.xdc` 상태: **정상** — 모든 포트(11개)에 PACKAGE_PIN과 IOSTANDARD(LVCMOS33)가 지정되어 있다.

> **수정 이력:** 끝줄이 잘려 있던 XDC를 완성했다(b[3:0]·o[2:0]의 IOSTANDARD 추가). 이제 Vivado 프로젝트를 만들어 구현·bit 생성·보드 확인을 진행하면 된다(**미완료**).

## 4. 실제 장치 기록 — 미완료

조건: Spartan-7 XC7S75 교육용 보드, Hardware Manager → Open target → Auto Connect → 장치 `xc7s75` 확인 → Program Device로 bit 기록.

- 장치 인식·기록 완료: ☐
- 조교 무작위 선정 여부와 출석부 기록: ☐ 선정 ☐ 미선정

| 번호 | 입력 | 예상 출력 | 실제 출력 | 사진·영상 |
|---|---|---|---|---|
| 1 | ______ | ______ | ______ | `evidence/board/photos/______` |
| 2 | ______ | ______ | ______ | `evidence/board/videos/______` |

## 5. 결과 해석

- 예상값·두 시뮬레이션·실측의 일치 또는 차이와 원인: ______
- 사전 수정 실험에서 배운 점(실패 원인·복구): ______
- 시험 조건(입력 범위·경계 조건)에 대한 판단: ______

## 6. 미수행 항목과 후속 확인

- [ ] Vivado 프로젝트 생성 → 시뮬레이션·합성·구현·bit 생성
- [ ] 보드 기록·사진·영상 및 조교 확인(4절)

## 7. 제출 점검

- [ ] Vivado 버전·part·top·핀 제약·커밋 기록(1절)
- [ ] VS Code/Vivado 비교(2절), 합성·구현·bit(3절)
- [ ] 장치 기록·사진·영상(4절), 해석(5절), 미완료 항목(6절)
- [ ] `reports/post/`, `evidence/`에 저장 후 push, GitHub 웹에서 사진·영상 확인
