# 💊 RAAPS – AI 기반 협동로봇 약 조제 어시스턴트 시스템
## ROKEY B-1조 협동-2 Project
---

### 🔗 출처 및 라이선스

이 프로젝트는 **두산로보틱스(Doosan Robotics Inc.)**에서 배포한 ROS 2 패키지를 기반으로 합니다.  
해당 소스코드는 [BSD 3-Clause License](https://opensource.org/licenses/BSD-3-Clause)에 따라 공개되어 있으며,  
본 저장소 또한 동일한 라이선스를 따릅니다. 자세한 내용은 `LICENSE` 파일을 참고하시기 바랍니다.

> ⚠️ 본 저장소는 두산로보틱스의 공식 저장소가 아니며, 비공식적으로 일부 수정 및 구성을 포함하고 있습니다.  
> 공식 자료는 [두산로보틱스 공식 홈페이지](http://www.doosanrobotics.com/kr/)를 참고해 주세요.   
> github (https://github.com/DoosanRobotics/doosan-robot2)

## 1. 📌 프로젝트 개요

**RAAPS (Rokey AI Automatic Pharmacy System)**는 사람의 생명과 직결된 **약 조제의 정확성과 안전성 확보**를 위해 개발된 **AI+로봇 기반 복약 보조 시스템**입니다.

### 🎯 개발 목적

- **약물 사고 예방**: 유사 약물 이름/포장, 비닐 삽입 오류 방지
- **약사 업무 부담 완화**: AI 음성 안내를 통해 증상 기반 일반약 추천 및 복용 설명
- **고령자/장애인 지원**: 스스로 복약이 어려운 사용자 대상 음성 안내 시스템 구축

---

## 2. 👥 팀 구성 및 역할

| 이름 | 역할 |
|------|------|
| 백홍하 (팀장) | Vision (약 탐지/좌표 추정), 로봇 이동 제어, 통합 |
| 서형원 | Robot control(일반 의약품)|
| 정민섭 | Voice node, 초음파 센서 기반 손님 인식 시스템 구축 및 통합|
| 정서윤 | Vision (QR, 선반, 약 탐지) 및 전문약 통합 |
| 멘토: 이충현 수석님 | 로봇 환경 및 알고리즘 지도 |

---

## 3. 🧪 프로젝트 구성 요소

### 🧰 하드웨어

- Intel RealSense D435i
- Logitech C270 HD Webcam
- Onrobot RG2 Gripper
- Doosan M0609 협동로봇
- Raspberry Pi 4 (4GB)
- HC-SR04 초음파 센서 x 2
- 블루투스 스피커 + 마이크

### 💊 약 분류

| 증상 | 약 리스트 |
|------|-----------|
| 감기 | panstar_tab, amoxicle_tab |
| 피부염 | monodoxy_cap, ganakan_tab, sudafed_tab |
| 소화불량 | nexilen_tab, medilacsenteric_tab, magmil_tab |
| 설사 | octylonium_tab, famodine, otilen_tab |

---

## 4. 🧠 AI 모델 구성

| Task | Model | 성능 |
|------|-------|------|
| 텍스트 OCR | YOLO11n | mAP = 0.995 |
| 이미지 분류 | ResNet18 | Accuracy = 1.00 |
| 증상별 Segmentation (cold, dermatitis, dyspepsia, diarrhea) | YOLO11s | mAP ≈ 0.992 |

---

## 5. 🗂 시스템 구성 (System Flow)

### 🧾 전문의약품 시스템 flow



### 🧾 전문의약품 시나리오

1. 초음파 센서로 사용자 감지
2. QR 코드 스캔 → 처방전 파싱
3. 약 서랍 인식 및 이동
4. 약 탐지 후 포즈 추정 (타원형/원형 알약)
5. 약 집기 → 봉투 담기
6. 약 설명 음성 출력

### 🛒 일반의약품 시스템 flow



### 🛒 일반의약품 시나리오

1. “hello rokey” 웨이크워드 감지
2. 증상 입력 → GPT-4o로 약 추천
3. 의약품 수락 후 선반에서 꺼내기
4. 사용자 흔들기 → 약 놓기 + 음성 설명

---

## 6. 🗣️ Voice AI 시스템

| 구성 | 사용 기술 |
|------|-----------|
| Wakeword 감지 | OpenWakeWord (TFLite) |
| 음성 인식 | Whisper-1 |
| 의미 분석/추천 | GPT-4o |
| 음성 출력 | Edge TTS (ko-KR-SunHiNeural) |

---

## 7. 🤖 로봇 시스템 구조

```text
[Doosan Robot Controller]
        ⇅ TCP/IP
[ROS2 노드: dsr_example_py, dsr_service 등]
        ⇅ ROS 2 Topics / Services
[사용자 제어 로직 또는 RViz2]
