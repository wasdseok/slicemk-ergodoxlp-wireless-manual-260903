# SliceMK ErgoDoxLP Wireless 사용설명서

> PCB Version 2022-07 Green + Raytac MDBT50Q-RX Green 동글 전용 개인 유지보수 스냅샷입니다.

이 저장소는 2026-09-03에 실제 키보드에 적용하고 전체 물리 키 입력을 확인한 구성, 한국어 사용설명서, 그리고 설정 과정의 공개용 대화 기록을 함께 보존합니다. SliceMK 또는 ZMK의 공식 저장소가 아니며 공식 지원이나 보증을 의미하지 않습니다.

## 빠른 링크

- [한국어 개인 사용설명서](docs/SliceMK_ErgoDoxLP_Wireless_개인_사용설명서.pdf)
- [공개용 전체 대화 기록](키세팅gpt대화.html)
- [최종 ZMK 키맵](config/slicemk_ergodox.keymap)
- [SliceMK Configurator 편집용 JSON](configurator/slicemk_keymap.json)
- [동글 설정](config/slicemk_ergodox_dongle.conf)
- [실제 설치한 동글 UF2](firmware/03-dongle-v20251223-recovered-3layer-keymap.uf2)
- [파일 체크섬](SHA256SUMS.txt)

## 현재 검증된 구성

| 항목 | 적용 상태 |
|---|---|
| 왼쪽 반쪽 | 공식 Peripheral 4.2.0 |
| 오른쪽 반쪽 | 공식 Peripheral 4.2.0 |
| 중앙 장치 | Raytac MDBT50Q-RX Green 동글 |
| 동글 앱 펌웨어 | Configurator v20251223 기반 |
| 키맵 | `main`, `fn`, `mouse` 3레이어 |
| 물리 키 | 레이어당 76키, 전체 물리 키 정상 입력 확인 |
| 동글 USER 버튼 | 각 레이어 첫 번째 `&bootloader` 바인딩, 물리 76키와 별도 |
| 충전 파란 LED | 기존 하드웨어 동작 유지 |
| RGB 언더글로우 | 평상시 꺼짐, 충전 상태 커스텀 표시 미적용 |
| 배터리 보고 | 좌우 실제 값을 동글에서 Windows로 전달하는 proxy 미적용 |
| NVS 초기화 | 하지 않음; 기존 split bond와 호스트 페어링 유지 |
| 부트로더 업데이트 | 하지 않음 |

`config/slicemk_ergodox_dongle.conf`가 0바이트인 것은 누락이 아닙니다. 마지막으로 적용한 구성에서 별도의 LED, RGB, 배터리 또는 동글 Kconfig 오버라이드를 사용하지 않았다는 뜻입니다.

## 키맵 데이터의 기준

- 실제 ZMK 빌드 기준: `config/slicemk_ergodox.keymap`
- Configurator에서 다시 편집할 때: `configurator/slicemk_keymap.json`
- GitHub Actions 빌드 대상: 루트 `build.yaml`
- 동글 전용 설정: `config/slicemk_ergodox_dongle.conf`

Configurator JSON은 물리 키 76개씩을 보관합니다. ZMK `.keymap`에는 물리 키와 별도인 동글 USER 버튼의 `&bootloader`가 앞에 추가되어 레이어당 77개 바인딩이 있습니다. JSON의 `Peripheral: 40100` 값은 Configurator 메타데이터이며, 실제 좌우 반쪽에 설치한 Peripheral 4.2.0 버전 문자열이 아닙니다.

## 레이어 개요

### Main

일반 문자, 숫자, 기호, 방향키, 한/영, 한자 입력을 사용합니다. 왼쪽 하단의 Fn과 오른쪽 하단의 Mouse는 누르고 있는 동안만 작동하는 키가 아니라 토글 방식입니다.

### Fn

F1-F24와 Bluetooth 프로필 0-4를 사용합니다. 왼쪽 맨 윗줄에서 `Esc`부터 `3`까지가 BT 0-4이며 `4` 위치는 현재 선택한 Bluetooth 프로필을 지웁니다. 왼쪽 하단의 같은 Fn 위치를 누르면 Main으로 돌아갑니다.

### Mouse

오른쪽 문자 영역에서 포인터 이동, 가로/세로 스크롤, 좌/중/우 클릭을 사용합니다. 오른쪽 하단의 같은 Mouse 위치를 누르면 Main으로 돌아갑니다.

자세한 물리 배열과 색상 범례는 PDF를 확인하십시오.

## 빌드 재현 범위

`config/west.yml`과 `.github/workflows/build.yml`은 확인 가능한 SliceMK ZMK 커밋 `71f8242b8ab2e9f9c86ebbb221ba15876413f295`에 고정했습니다. 앞으로 upstream `main`이 변경되어도 이 저장소의 빌드 입력이 자동으로 바뀌지 않게 하기 위한 조치입니다.

공식 Configurator가 표시했던 내부 커밋은 공개 SliceMK 저장소에서 조회되지 않았습니다. 따라서 GitHub Actions의 새 빌드는 같은 키맵 의미를 컴파일하지만, `firmware/`에 보관한 당시 설치 UF2와 bit-for-bit로 동일하다고 보장하지 않습니다. `firmware/`의 UF2가 실제로 설치하고 확인한 동글 파일입니다.

## 업데이트와 복구 절차

1. 현재 `CURRENT.UF2`, 이 저장소의 설정 파일과 체크섬을 별도로 백업합니다.
2. PCB가 `2022-07 Green`, 동글이 `Raytac MDBT50Q-RX Green`인지 다시 확인합니다.
3. Configurator JSON 또는 `.keymap`을 수정합니다.
4. Main/Fn/Mouse의 물리 키가 각각 76개인지, `.keymap`에는 별도 USER 버튼을 포함해 77개인지 확인합니다.
5. 동글 펌웨어를 빌드하고 동글에만 기록합니다. 좌우 Peripheral을 키맵 변경 때마다 다시 설치할 필요는 없습니다.
6. Main 문자 입력, Fn F1-F24/BT 기능, Mouse 이동/스크롤/클릭과 레이어 복귀를 실제 장치에서 검사합니다.
7. 설명서를 다시 만들고 전 페이지 렌더링을 확인한 뒤 `SHA256SUMS.txt`와 `CHANGELOG.md`를 갱신합니다.

## 중요한 플래시 주의사항

- 이 파일을 다른 PCB 색상, 연식 또는 다른 동글 모델에 기록하지 마십시오.
- 키보드 반쪽과 동글은 파일이 서로 다릅니다. Board-ID를 확인한 뒤 정확한 대상에만 기록하십시오.
- NVS 초기화는 배터리 보정이나 일반 업데이트 단계가 아닙니다. Bluetooth 프로필, bond와 split 설정을 지우는 마지막 복구 수단입니다.
- 부트로더 업데이트는 키맵, 배터리 보고 또는 무선 성능을 개선하는 일반 앱 업데이트가 아닙니다.
- Windows Bluetooth 화면의 `100%`는 현재 구성에서 좌우 배터리 실제 잔량, 평균 또는 최저값으로 확인된 수치가 아닙니다.
- 펌웨어 기록 중에는 USB 케이블을 분리하지 마십시오.

## 개인정보와 공개본

공개용 PDF에서는 세 장치의 USB serial을 `비공개`로 바꿨습니다. 대화 HTML에서는 이메일, 절대 로컬 사용자 경로, USB serial과 식별자를 제거했으며 개인정보나 로컬 파일 목록이 보이는 화면 캡처 4장을 안내문으로 대체했습니다. 나머지 첨부 이미지는 메타데이터를 제거한 뒤 HTML 안에 포함했습니다. 원본 Codex 로그, `CURRENT.UF2` 백업, 개인 화면 캡처와 작업용 디렉터리는 이 저장소에 포함하지 않습니다.

## 라이선스와 출처

이 저장소 전체에 대한 별도 재사용 라이선스는 부여하지 않았습니다. 공개 열람 가능하다는 사실만으로 저장소 내용 전체가 자동으로 오픈소스가 되지는 않습니다. ZMK, SliceMK 펌웨어와 문서는 각 원 프로젝트의 저작권과 라이선스를 따릅니다.

- [SliceMK 무선 키보드 안내](https://docs.slicemk.com/firmware/zmk/wireless/guide/)
- [SliceMK Bluetooth 안내](https://docs.slicemk.com/firmware/zmk/wireless/bluetooth/)
- [SliceMK Peripheral 안내](https://docs.slicemk.com/keyboard/ergodox/peripheral/)
- [SliceMK NVS 초기화 안내](https://docs.slicemk.com/firmware/zmk/wireless/nvsclear/)
- [ZMK Firmware](https://github.com/zmkfirmware/zmk)

자세한 비제휴 및 책임 고지는 [NOTICE.md](NOTICE.md)를 확인하십시오.
