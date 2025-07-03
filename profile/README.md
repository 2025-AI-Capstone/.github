# FOCUS

고령자 및 단독 생활자의 증가와 함께 주거 공간 내 낙상 사고는 주요 사회적 위험으로 부각되고 있습니다. 2023년 기준, 전체 낙상 사고의 약 48.9%가 주택 내에서 발생하며, 특히 고령자의 경우 신속한 대응이 없을 시 생명에 큰 위협이 될 수 있습니다. 또한 산업 현장에서도 낙상은 빈번하게 발생하며, 산업재해에서 가장 큰 비중을 차지합니다.

하지만 기존의 낙상 대응 시스템은 대부분 사후 대응에 초점을 맞추고 있어, 실시간 감지 및 빠른 조치가 어려운 한계가 있습니다. 이에 따라, AI 기반 실시간 자세 감지 및 위험 알림 시스템 구축이 필수적입니다.

본 프로젝트는 비전 기반 시스템을 통해 낙상을 실시간으로 감지하고 즉각 경고 및 알림을 제공하는 기술적 솔루션을 통해, 자동화된 대응 시스템을 통해 낙상 상황에서의 빠르고 정확한 대처를 목표로 합니다. 또한 AI Agent 대화 기능, 사용자 맞춤 알림 기능으로 포함해 평상시에서의 유용성을 높이기 위해 노력했습니다.

--- 
## 👥 Team Members
| <img src="https://github.com/RohKJ.png" width="100px"> | <img src="https://github.com/owenminjong.png" width="100px"> | <img src="https://github.com/JoYerin1226.png" width="100px"> | <img src="https://github.com/ssoeun-y.png" width="100px"> | <img src="https://github.com/suwdle.png" width="100px"> |
| :-----------------------------------------------------: | :----------------------------------------------------------: | :------------------------------------------------------------: | :--------------------------------------------------------: | :-----------------------------------------------------: |
| [노경준](https://github.com/RohKJ) <br> [`Backend`](https://github.com/2025-AI-Capstone/Back) | [정민종](https://github.com/owenminjong) <br> [`Frontend`](https://github.com/2025-AI-Capstone/front) | [조예린](https://github.com/JoYerin1226) <br> [`Frontend`](https://github.com/2025-AI-Capstone/front) | [천소은](https://github.com/ssoeun-y) <br> [`AI`](https://github.com/2025-AI-Capstone/fall-detection) | [송석준](https://github.com/suwdle) <br> [`ROS2 Pipeline`](https://github.com/2025-AI-Capstone/ros2-pipeline) |


## 🔍 System Overview

FOCUS는 원격 낙상 감지 시스템으로, 각 기능의 구현에 여러 기술이 포함되지만 이를 유기적으로 연결해 실시간으로 동작하도록 만들었습니다. 낙상 상황을 빠르게 감지하고, 사용자 응답 여부에 따라 보호자에게 자동으로 문자 알림을 보내는 메인 기능을 포함해 웹 대시보드에서 카메라를 실시간으로 관찰할 수 있고, 여러 설정 및 사용자 상호작용이 가능하도록 개발했습니다. 이를 구현하는 것에 필요한 데이터 송수신은 ROS2 토픽, HTTP, WebSocket 등을 조합해 처리했습니다.


## 🖥️ 웹 대시보드

React로 개발된 웹 대시보드는 실시간 데이터를 시각화하고, 사용자와 상호작용할 수 있는 기능을 제공합니다.

* 카메라 영상 스트리밍
* 각 노드의 상태 확인
* 낙상 이벤트 및 음성 대화 기록 조회
* 하루 통계
* 낙상 알림 On/Off 상태 전환

ROS2 시스템과 WebSocket 및 REST API로 연결되어 실시간 동기화를 유지합니다.

## 📡 긴급 문자 전송 흐름

낙상 발생 시 시스템은 다음과 같은 절차로 문자를 전송합니다.

1. 에이전트가 사용자에게 상태를 묻습니다.
2. 일정 시간 동안 응답이 없거나 위험 상황으로 판단되면,
3. 에이전트가 상황에 맞는 메시지를 작성합니다.
4. 작성된 메시지는 서버를 통해 Solapi로 전달되고,
5. 등록된 보호자 연락처로 긴급 문자가 발송됩니다.

## 🧠 낙상 감지 모델

영상에서 추출한 keypoint를 그래프 형태로 구성하고, GAT 기반 모델을 이용해 낙상 여부를 예측합니다. Pose estimation model이 전달한 human skeleton keypoint를 grpah 데이터로 변환해 데이터의 구조적 특징을 모델이 더 반영할 수 있도록 설계했습니다.

* 구조: PyTorch Geometric 기반 GATConv
* 입력: 17개 keypoint (x, y, confidence)와 관절 간 edge 정보
* 특징: 거리, 방향각, confidence 평균을 edge feature로 사용
* 보완: 실제 환경을 반영해 일부 관절이 누락된 상태를 학습 시 반영 (PointOut)

## 💡 주요 기능 요약

* ROS2 기반 모듈형 구조로 기능별 분리 및 병렬 처리
* 사용자 음성 응답을 통한 상황 판단 및 실시간 정보 제공
* Redis로 알림 설정 상태를 관리하고 전체 시스템에 반영
* 상황에 맞는 경고 메시지를 자동 생성하고 보호자에게 문자 전송
* 직관적인 실시간 대시보드 제공

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge\&logo=python\&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge\&logo=pytorch\&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge\&logo=opencv\&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge\&logo=fastapi\&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-cc0000?style=for-the-badge\&logo=databricks\&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-000000?style=for-the-badge\&logo=graphql\&logoColor=white)
![ROS2](https://img.shields.io/badge/ROS2-22314E?style=for-the-badge\&logo=ros\&logoColor=white)
![WebSocket](https://img.shields.io/badge/WebSocket-010101?style=for-the-badge\&logo=websockets\&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge\&logo=react\&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge\&logo=typescript\&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-06B6D4?style=for-the-badge\&logo=tailwindcss\&logoColor=white)





> 본 프로젝트는 성공회대학교 인공지능학과 2025년도 캡스톤디자인으로 진행된 프로젝트입니다.



