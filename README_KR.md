
# 허깅페이스 디퓨전 모델 코스

[![License](https://img.shields.io/static/v1?label=License&message=Apache&color=<Yellow>)](https://github.com/huggingface/diffusion-models-class/blob/main/LICENSE) &nbsp;
[![GitHub forks](https://img.shields.io/github/forks/huggingface/diffusion-models-class.svg?style=social&label=Fork&maxAge=2592000)](https://github.com/dhakalnirajan/diffusion-models-class) &nbsp;
[![Made with Jupyter](https://img.shields.io/badge/Made%20with-Jupyter-red?style=flat-square&logo=Jupyter)](https://jupyter.org/try) &nbsp;
![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=flat-square&logo=PyTorch&logoColor=white)

이 무료 강좌에서는
- 👩‍🎓 디퓨전 모델의 이론을 배웁니다
- 🧨 🤗의 인기쟁이 Diffusers 라이브러리로 이미지와 오디오를 생성하는 방법을 배웁니다
- 🏋️‍♂️ 여러분만의 디퓨전 모델을 밑바닥부터 학습시켜 봅니다
- 📻 새로운 데이터셋에 사전 학습된 디퓨전 모델을 파인튜닝해 봅니다
- 🗺 조건부(conditional) 생성 및 유도/가이던스(guidance)를 탐색해 봅니다
- 🧑‍🔬 여러분만의 커스텀 디퓨전 모델 파이프라인을 만들어 봅니다


**[가입 양식](https://huggingface.us17.list-manage.com/subscribe?u=7f57e683fa28b51bfc493d048&id=ef963b4162)** 을 통해 이 코스에 가입하고, **[디스코드 채널](https://discord.gg/aYka4Yhff9)** 에도 참여하여 재밌는 토론을 시작해 보기 바랍니다. 특정 주제/채널에 참여하는 방법은 **[여기](https://discord.com/channels/879548962464493619/1014509271255367701)** 를 참고하기 바랍니다.

## 과정

| 📆 발행일  | 📘 단원           | 👩‍💻 실습 내용 |
|---------------|----------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| 2022년 11월 28일  | [디퓨전 모델에 대한 소개](https://github.com/deep-diver/diffusion-models-class/blob/main/unit1/README_KR.md)| Diffusers 라이브러리와 디퓨전 모델을 밑바닥에서 부터 소개합니다 |
| 2022년 12월 12일  | [파인튜닝과 가이던스(Guidance)](https://github.com/huggingface/diffusion-models-class/tree/main/unit2) | 새로운 데이터로 디퓨전 모델을 파인튜닝하고, 가이던스를 추가해 봅니다 |
| 2022년 12월 21일  | [스테어블 디퓨전](https://github.com/huggingface/diffusion-models-class/tree/main/unit3) | 텍스트 조건에 따른 Latent 디퓨전 모델을 탐구합니다 |
| 2023년 1월 (TBC)  | [디퓨전으로 더 다양한 것 해보기](https://github.com/huggingface/diffusion-models-class/tree/main/unit4) | 디퓨전을 더 깊게 활용하기 위한 고급 기법을 살펴봅니다 |

더 많은 정보가 계속 추가될 예정입니다!


## 사전 지식
- 능숙한 파이썬 프로그래밍 🐍
- 딥러닝과 파이토치에 대한 기본 지식

만약 사전 지식을 충족하지 못한다면, 무료로 제공되는 다음 자료를 확인해보기 바랍니다
- 파이썬: https://www.udacity.com/course/introduction-to-python--ud1110
- 파이토치와 딥러닝: https://www.udacity.com/course/deep-learning-pytorch--ud188
- 60분안에 배우는 파이토치: https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html

## 질답
**이 코스는 무료인가요?**

- 넵, 완~전히 무료입니다 🥳.


**실습을 따라하려면 허깅페이스 계정이 필요한가요?**

- 넵, 여러분의 모델과 파이프라인을 허깅페이스 허브(Hub)에 업로드하려면 계정이 필요합니다(무료에요) 🤗.
- 아직 계정이 없다면 👉 [여기서](https://huggingface.co/join) 가입해 보세요

**이 코스는 어떤 식으로 구성되어 있나요?**

- 최소 **네 개**의 단원으로 구성됩니다. 그리고 계속해서 더 많은 내용이 추가될 예정입니다. 가령 _오디오 생성을 위한 디퓨전_ 같은 내용 말이죠.
- 각 단윈은 이론과 배경 지식, 그리고 하나 이상의 실습용 노트북으로 구성됩니다. 일부 단원은 여러분들이 직접 수행해볼 만한 프로젝트를 제안하기도 하고, 가장 멋진 파이프라인과 데모로 경쟁하는  대회를 개최하기도 할 예정입니다(더 자세한 내용은 추후 공지하겠습니다).
