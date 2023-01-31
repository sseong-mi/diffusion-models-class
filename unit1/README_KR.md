# 단원 1: 디퓨전 모델에 대한 소개

허깅페이스의 디퓨전 모델 코스 제 1 단원에 오신것을 환영합니다! 1 단원에서는 디퓨전 모델의 기본 작동법과
🤗 Diffusers 라이브러리로 여러분만의 디퓨전 모델을 만드는 방법에 대해 알아봅니다.

## 단원의 시작 :rocket:

이 단원은 다음의 단계로 구성됩니다

- [이 코스에 가입](https://huggingface.us17.list-manage.com/subscribe?u=7f57e683fa28b51bfc493d048&id=ef963b4162)합니다. 그래야 새로운 자료가 추가되었을 때 그 소식을 받아볼 수 있습니다.
- 흥미로운 추가 자료를 직접 찾아보는 것과 함께, 아래의 소개용 자료를 읽어봅니다.
- 이론이 실제 코드로 옮겨지는 과정을 🤗 Diffusers 라이브러리로 작성된 _**Introduction to Diffusers**_ 노트북을 통해 확인합니다.
- 노트북 또는 링크로 제공되는 디퓨전 모델을 학습시키는 스크립트를 사용해 여러분 만의 디퓨전 모델을 학습시키고 공유합니다.
- (옵션) 밑바닥에서 부터 최소한의 코드를 구현하며 설계상 관련된 서로 다른 결정 사항에 관심이 있다면,  _**Diffusion Models from Scratch**_ 노트북을 통해 그 내용을 깊게 이해해 봅니다.
- (옵션) 이 단원의 자료를 릴리즈 하기 전 리허설한 [이 비디오](https://www.youtube.com/watch?v=09o5cv6u76c)를 시청합니다.

:loudspeaker: [디스코드 서버](https://huggingface.co/join/discord)에 가입하는 것을 잊지 마세요. `#diffusion-models-class` 채널에서 이 코스가 제공하는 자료를 사람들과 토론하고, 여러분이 만든 결과물을 공유해보기 바랍니다.
 
## 디퓨전 모델이란 무엇일까요?

디퓨전 모델은 '생성 모델'로 알려진 알고리즘 그룹에, 비교적 최근에 추가된 모델입니다. 생성 모델의 목표는 주어진 학습용 데이터셋에 기반하여, 이미지 또는 오디오 같은 데이터의 **생성** 방법을 터득하는 것입니다. 잘 만들어진 생성 모델은 그대로 복사하지 않고도, 학습용 데이터와 닮은 **다양한** 출력을 생성할 수 있습니다. 디퓨전 모델은 이 목표를 어떻게 달성할 수 있을까요? 명확한 이해를 위해서 이미지 생성에 대한 경우에 집중해보죠.

<p align="center">
    <img src="https://user-images.githubusercontent.com/10695622/174349667-04e9e485-793b-429a-affe-096e8199ad5b.png" width="800"/>
    <br>
    <em> DDPM 논문에서 발췌한 그림 (https://arxiv.org/abs/2006.11239). </em>
<p>

디퓨전 모델이 성공한 이유, 그 비밀은 디퓨전 과정이 가진 반복적인 특성 입니다. 생성은 임의의 노이즈에서 시작하지만, 출력 이미지가 드러날 때까지 여러 단계를 거쳐 개선/정제되어 나갑니다. 각 단계마다 모델은 현재 입력이 어떻게 노이즈가 완전히 걷힌 상태로 갈 수 있을지를 추정합니다. 그러나 매 단계마다 약간의 변화만 만들기 때문에, 초기 단계(최종 출력의 예측이 매우 어려운 단계)에서의 이 추정의 오차는 이후 갱신에서 수정될 수 있습니다.

모델을 학습시키는 과정은 다른 생성 모델 대비, 비교적 간단한 편입니다. 다음 과정을 반복해서 수행하죠
1) 학습용 데이터에서 일부 이미지를 불러옵니다
2) 서로 다른 양의 노이즈를 추가합니다. 우리가 원하는 것은 매우 노이즈가 심한 이미지와 완변게 가까운 이미지 모두를 '고치는'일을 잘 하는 모델이라는 것을 기억하세요.
3) 모델에 노이즈가 낀 버전의 입력을 주입합니다.
4) 모델이 입력의 노이즈를 얼마나 잘 걷어내는지 평가합니다.
5) 이 정보를 사용해서 모델의 가중치를 갱신합니다.

학습된 모델로 새로운 이미지를 생성하려면, 완벽한 임의의 입력(노이즈)로 시작해서, 이를 모델에 반복적으로 주입합니다. 매 반복마다 모델의 예측에 따라 약간씩 노이즈를 갱신해 나가죠. 곧 보겠지만 가능한 적은 단계(반복)으로 질 좋은 이미지를 생성할 수 있도록, 이 과정을 간소화해주는 다양한 샘플링 기법이 존재합니다.

1 단원에서는 각 단계를 실습용 노트북을 통해 하나씩 확인합니다. 그리고 2 단원에서는 이 과정을 수정하고, 추가적인 조건(클래스 레이블 등)이나 유도/가이던스(guidance)같은 기법을 통해 모델 출력을 제어하는 추가적인 방법을 살펴봅니다. 마지막 3, 4 단원에서는 텍스트에 알맞은 이미지를 생성할 수 있는 매우 강력한 Stable Diffusion 이라는 모델을 탐구합니다.

## 실습용 노트북

이제 제공되는 노트북을 시작하기에 충분한 지식을 갖췄습니다! 이 단원이 제공하는 두 노트북은 같은 내용을 다른 방식으로 바라봅니다.
 
| 노트북 이름                                     | 콜랩                                                                                                                                                                                               | 캐글                                                                                                                                                                                                   | Gradient                                                                                                                                                                               | 아마존 스튜디오 랩                                                                                                                                                                                                   |
|:--------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Introduction to Diffusers                                | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/huggingface/diffusion-models-class/blob/main/unit1/01_introduction_to_diffusers.ipynb)              | [![Kaggle](https://kaggle.com/static/images/open-in-kaggle.svg)](https://kaggle.com/kernels/welcome?src=https://github.com/huggingface/diffusion-models-class/blob/main/unit1/01_introduction_to_diffusers.ipynb)              | [![Gradient](https://assets.paperspace.io/img/gradient-badge.svg)](https://console.paperspace.com/github/huggingface/diffusion-models-class/blob/main/unit1/01_introduction_to_diffusers.ipynb)              | [![Open In SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/huggingface/diffusion-models-class/blob/main/unit1/01_introduction_to_diffusers.ipynb)              |
| Diffusion Models from Scratch                                | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/huggingface/diffusion-models-class/blob/main/unit1/02_diffusion_models_from_scratch.ipynb)              | [![Kaggle](https://kaggle.com/static/images/open-in-kaggle.svg)](https://kaggle.com/kernels/welcome?src=https://github.com/huggingface/diffusion-models-class/blob/main/unit1/02_diffusion_models_from_scratch.ipynb)              | [![Gradient](https://assets.paperspace.io/img/gradient-badge.svg)](https://console.paperspace.com/github/huggingface/diffusion-models-class/blob/main/unit1/02_diffusion_models_from_scratch.ipynb)              | [![Open In SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/huggingface/diffusion-models-class/blob/main/unit1/02_diffusion_models_from_scratch.ipynb)              |

In _**Introduction to Diffusers**_, we show the different steps described above using building blocks from the diffusers library. You'll quickly see how to create, train and sample your own diffusion models on whatever data you choose. By the end of the notebook, you'll be able to read and modify the example training script to train diffusion models and share them with the world! This notebook also introduces the main exercise associated with this unit, where we will collectively attempt to figure out good 'training recipes' for diffusion models at different scales - see the next section for more info.

In _**Diffusion Models from Scratch**_, we show those same steps (adding noise to data, creating a model, training and sampling) but implemented from scratch in PyTorch as simply as possible. Then we compare this 'toy example' with the diffusers version, noting how the two differ and where improvements have been made. The goal here is to gain familiarity with the different components and the design decisions that go into them so that when you look at a new implementation you can quickly identify the key ideas.

## Project Time

Now that you've got the basics down, have a go at training one or more diffusion models! Some suggestions are included at the end of the _**Introduction to Diffusers**_ notebook. Make sure to share your results, training recipes and findings with the community so that we can collectively figure out the best ways to train these models.

## Some Additional Resources
 
[The Annotated Diffusion Model](https://huggingface.co/blog/annotated-diffusion) is a very in-depth walk-through of the code and theory behind DDPMs with 
 maths and code showing all the different components. It also links to a number of papers for further reading.
 
Hugging Face documentation on [Unconditional Image-Generation](https://huggingface.co/docs/diffusers/training/unconditional_training) for some examples of how to train diffusion models using the official training example script, including code showing how to create your own dataset. 

AI Coffee Break video on Diffusion Models: https://www.youtube.com/watch?v=344w5h24-h8

Yannic Kilcher Video on DDPMs: https://www.youtube.com/watch?v=W-O7AZNzbzQ

Found more great resources? Let us know and we'll add them to this list.
