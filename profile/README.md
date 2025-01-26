# CapEasy, 동영상 하나로 만드는 공간 이미지
홍익대학교 2024학년도 제 21회 소프트웨어융합학과 학술제 팀 프로젝트, VR 공간 이미지 제작 서비스

---

## 프로젝트 개요

- 본 프로젝트는 사용자가 특정 공간에서 촬영한 동영상을 활용하여 모든 방향에서 해당 공간을 볼 수 있는 “파노라마 형태의 구(球)형 이미지”로, 가상 현실의 배경으로 적합한 “공간 이미지”를 생성하는 서비스입니다.
  - 해당 서비스는 웹 애플리케이션으로 제공되며, 사용자가 생성한 공간 이미지를 손쉽게 공유하고 다른 사용자와 상호작용할 수 있는 기능을 지원합니다.
- 공간 이미지를 생성하는 과정은 동영상에서 추출된 프레임에 자체 개발한 이미지 초해상화 모델을 적용한 후 이미지 스티칭 기술을 사용하여 이루어집니다.
  - 해당 모델은 기존 SRCNN 모델에 추가적인 레이어를 적용하고 더 많은 데이터를 학습하여 개발되었으며, 이를 통해 이미지 스티칭 품질을 개선하여 보다 왜곡이 최소화된 고해상도 공간 이미지를 제공하는 것에 기여한다.

---

## 기대 효과

1. 사용자는 스마트폰으로 촬영한 동영상만으로 고가의 장비 없이 손쉽게 공간 이미지를 제작할 수 있습니다.
2. 사용자가 생성한 공간 이미지는 가상 현실 플랫폼의 배경으로 활용될 수 있습니다.
3. 문화체육관광부 및 지방자치단체와의 협업을 통해 관광객 유치 등을 통한 지역 사회 발전을 도모할 수 있습니다.
4. 화재 현장의 발화 지점에 대한 의견 공유 시스템과 같이 사건 현장 철수 후의 재확인을 위한 서비스에 사용될 수 있습니다.

---

## 프로젝트 팀원

| [권덕재(B989003)](https://github.com/juintination) | [강보찬(B989001)](https://github.com/WellshCorgi) | [김기현(B989009)](https://github.com/FIFLove) | [이승호(B989037)](https://github.com/iseungho) |
| :--: | :--: | :--: | :--: |
|![권덕재](https://github.com/user-attachments/assets/4eb9689b-8ac9-4df7-a11a-d83ace99a776)|![강보찬](https://github.com/user-attachments/assets/b7d1eaa2-6aeb-48a8-826b-2372b00021aa)|![김기현](https://github.com/user-attachments/assets/277bf4da-04a7-4f0a-97bc-0dea5d9202bf)|![이승호](https://github.com/user-attachments/assets/ef8bf72d-ad98-4fba-8564-2d9303a37502)|

---

## 목차

- [프로젝트 내용](#프로젝트-내용)
- [실제 실행 화면 예시](#실제-실행-화면-예시)
- [System Architecture](#System-Architecture)
- [E-R 다이어그램](#E-R-다이어그램Peter-Chen-표기법)
- [관련 대외 활동](#관련-대외-활동)

---

## 프로젝트 내용

<div align="center" >

### 이미지 스티칭이란?

![009](https://github.com/user-attachments/assets/acc0f05e-6cb9-4e86-adba-7867399fea7f)

![010](https://github.com/user-attachments/assets/a3dbcd36-0879-4c4f-9de2-2ced856e67da)

![011](https://github.com/user-attachments/assets/a99d36ed-87a7-4aa1-b9e8-f8c51699e780)

---

### SRCNN 기반 자체 제작 모델, SRCNN-PANO

![014](https://github.com/user-attachments/assets/c5b58bd9-4b0d-438a-b0ff-d36659fb1476)

![015](https://github.com/user-attachments/assets/77fb6ae8-3714-4604-b417-413728c970a0)

---

### 관련 성능 평가

![016](https://github.com/user-attachments/assets/20b827e2-0df3-439f-9aa2-5bcd42e14657)

![017](https://github.com/user-attachments/assets/413fd0aa-be0c-4f2f-a0f7-4998efa4b980)

![018](https://github.com/user-attachments/assets/186d3103-0078-410f-8080-53d2a1bd9b2f)

![019](https://github.com/user-attachments/assets/dda58ec1-0bac-45d7-b93e-590c9ed52d64)

</div>

---

### 동영상으로 입력받는 이유

- 실제로 여러장의 이미지를 입력하여 공간 이미지를 생성했을 때 다양한 원인으로 인한 스티칭 오류가 발생할 수 있습니다.
  - 입력받은 이미지들의 특정 부분에 공통된 특징점이 포함되지 않은 경우
  - 특정 공간의 사진이 누락된 경우
  - 손떨림 등으로 인해 각도가 틀어진 경우 등

- 그래서 저희는 동영상도 연속되는 수많은 이미지들일 뿐이라는 아이디어에서 시작하여 하나의 동영상으로부터 각 프레임 이미지들을 추출하는 방법을 고안하였습니다.
  - 동영상에서 추출된 프레임 이미지들은 연속되는 이미지들이기 때문에 자연스럽게 이어지고, 사진들의 각도가 일정하게 유지된다는 장점이 있습니다.
  - 그렇기 때문에 누락되는 공간이 최소화되고 위의 문제들을 해결할 수 있습니다.

---

## 실제 실행 화면 예시

![024](https://github.com/user-attachments/assets/9902ed12-1944-4d12-a7ed-56dabbaf855c)

![025](https://github.com/user-attachments/assets/2e3a1b30-5074-4103-a1a9-3f965cc95fea)

![026](https://github.com/user-attachments/assets/2ea92230-58c0-4e51-9c8f-fb0234ac4973)

![027](https://github.com/user-attachments/assets/4b62529e-c3d1-4ea7-af63-08968c5a12d8)

![028](https://github.com/user-attachments/assets/8e2a2d7c-79d4-4776-8a61-398dbeb97bd4)

---

## System Architecture

![SystemArchitecture(no EB) (2)](https://github.com/user-attachments/assets/5b86957e-f18d-4060-a198-85691b13962b)

---

## E-R 다이어그램(Peter Chen 표기법)

![vperd(drawio)](https://github.com/user-attachments/assets/eae239eb-9f2f-41f4-838d-a8f0ce96521c)

## E-R 다이어그램(IE 표기법)

![vperd(dbdiagramio)](https://github.com/user-attachments/assets/ab43aa15-3bc8-479a-a6b4-dddeb87aa455)

---

## 관련 대외 활동

![031](https://github.com/user-attachments/assets/7f57be15-1767-4d09-bcd3-f336432a8216)

![032](https://github.com/user-attachments/assets/faaabb88-928f-4402-8073-16425206b135)

![037](https://github.com/user-attachments/assets/41c7dc81-95d9-4ee0-934d-b8ec1c585b78)

![035](https://github.com/user-attachments/assets/38a6fe14-626d-43f4-b710-079f72bdb88f)

![038](https://github.com/user-attachments/assets/81e9aabe-3d52-436a-8180-d078047114b7)

---

## 기타 사항

- FE 웹 애플리케이션 사용언어 및 개발환경 : <img src="https://img.shields.io/badge/Visual Studio Code-007ACC?style=for-the-badge&logo=Visual Studio Code&logoColor=white"> <img src="https://img.shields.io/badge/WebStorm-000000?style=for-the-badge&logo=WebStorm&logoColor=white"> <img src="https://img.shields.io/badge/Node.js-5FA04E?style=for-the-badge&logo=nodedotjs&logoColor=white"> <img src ="https://img.shields.io/badge/HTML5-E34F26.svg?&style=for-the-badge&logo=HTML5&logoColor=white"/> <img src ="https://img.shields.io/badge/JavaScriipt-F7DF1E.svg?&style=for-the-badge&logo=JavaScript&logoColor=black"/> <img src ="https://img.shields.io/badge/CSS3-1572B6.svg?&style=for-the-badge&logo=CSS3&logoColor=white"/> <img src ="https://img.shields.io/badge/Tailwind CSS-06B6D4.svg?&style=for-the-badge&logo=Tailwind CSS&logoColor=white">
- BE API 서버 사용언어 및 개발환경 : <img src="https://img.shields.io/badge/IntelliJ IDEA-000000?style=for-the-badge&logo=IntelliJ IDEA&logoColor=white"> <img src="https://img.shields.io/badge/java-007396?style=for-the-badge&logo=OpenJDK&logoColor=white"> <img src="https://img.shields.io/badge/Spring-6DB33F?style=for-the-badge&logo=spring&logoColor=white"> <img src="https://img.shields.io/badge/Spring Boot-6DB33F?style=for-the-badge&logo=spring boot&logoColor=white"> <img src="https://img.shields.io/badge/Spring Security-6DB33F?style=for-the-badge&logo=spring security&logoColor=white"> <img src="https://img.shields.io/badge/json web tokens-000000?style=for-the-badge&logo=json web tokens&logoColor=white"> <img src="https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white"> <img src="https://img.shields.io/badge/Junit5-25A162?style=for-the-badge&logo=junit5&logoColor=white"> 
- 스티칭 서버 사용언어 및 개발환경 : <img src="https://img.shields.io/badge/Visual Studio Code-007ACC?style=for-the-badge&logo=Visual Studio Code&logoColor=white"> <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white"> <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white"> <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white"> <img src="https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white"> <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white"> <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white">

## 더 자세한 내용 보기

- fe-web-application: [iseungho/Capeasy](https://github.com/comprehensive-giftset/fe-web-application)
- be-api-server: [juintination/vp-api-server](https://github.com/comprehensive-giftset/be-api-server)
- stitching-api-server: [WellshCorgi/stitching-api-server](https://github.com/comprehensive-giftset/stitching-api-server)
