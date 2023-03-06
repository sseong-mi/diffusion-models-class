# Unit 2: Fine-Tuning, Guidance and Conditioning

Hugging Face Diffusion Models Course의 두번째 유닛에 오신것을 환영합니다! 이번 유닛에서는, pre-train된 디퓨전 모델을 다양한 방법으로 어떻게 적용하는지를 학습합니다. 또한 생성 과정을 제어하기 위해 **조건부**로 추가 입력을 받는 디퓨전 모델을 만드는 방법도 살펴볼 수 있습니다.

## Start this Unit :rocket:

이 유닛의 단계는 다음과 같습니다:

- 새 자료가 출시될 때 알림을 받으려면 [이 코스에 등록하세요.](https://huggingface.us17.list-manage.com/subscribe?u=7f57e683fa28b51bfc493d048&id=ef963b4162)
- 이 유닛의 핵심 아이디어에 대한 개요를 보려면 아래 자료를 읽어보세요.
- 🤗 디퓨저 라이브러리를 사용하여 새 데이터 세트에서 기존 디퓨전 모델을 fine-tuning 하고 가이드를 사용하여 샘플링 절차를 수정하려면 _**Fine-tuning and Gudiance**_ 노트북을 확인하세요.
- 노트북의 예시를 따라 개인 커스텀 모델에 대한 Gradio 데모를 공유하세요.
- (옵션) _**Class-conditioned Diffusion Model Example**_ 노트북을 확인하여 생성 과정에 추가 제어 기능을 추가하는 방법을 알아보세요.
- (옵션) 이 유닛의 내용을 대략적으로 살펴보려면 [이 동영상](https://www.youtube.com/watch?v=mY20iKOQ2zw)을 확인하세요


:loudspeaker:[Discord](https://huggingface.co/join/discord)에 가입하여 '#diffusion-models-class'  채널에서 자료에 대해 토론하고 모델을 공유하는 것을 잊지 마세요!
 
## Fine-Tuning

유닛 1에서 보셨겠지만, 디퓨전 모델을 처음부터 훈련하는 것은 시간이 많이 소요될 수 있습니다!  특히 해상도가 높아질수록 모델을 처음부터 학습시키는 데 필요한 시간과 데이터는 비현실적으로 늘어날 수 있습니다. 다행히도 해결책이 있습니다. 이미 학습된 모델부터 시작하세요! 이렇게 하면 이미 어떤 종류의 이미지 노이즈 제거 방법을 학습한 모델에서 시작하기 때문에 무작위로 초기화된 모델에서 시작하는 것보다 더 나은 출발점을 제공할 수 있습니다. 

![LSUN Bedrroms 데이터셋으로 학습시키고, WikiArt 데이터셋으로 500번 fine-tuning된 모델이 생성한 이미지 예시](https://api.wandb.ai/files/johnowhitaker/dm_finetune/2upaa341/media/images/Sample%20generations_501_d980e7fe082aec0dfc49.png)

Fine-tuning은 일반적으로 새 데이터가 기본 모델의 원래 학습 데이터와 어느정도 유사한 경우 가장 효과적입니다. (예를 들어, 만화적 얼굴을 생성하려는 경우 얼굴에 대해 학습된 모델로 시작하는 것이 좋습니다.) 하지만 놀랍게도 도메인을 크게 변경해도 이점은 계속 유지됩니다. 위의 이미지는 [LUSN Bedroom 데이터셋을 기반으로 학습된 모델](https://huggingface.co/google/ddpm-bedroom-256)을 [the WikiArt dataset](https://huggingface.co/datasets/huggan/wikiart)으로 500번 fine-tuning 하여 생성했습니다. [학습 코드](https://github.com/huggingface/diffusion-models-class/blob/main/unit2/finetune_model.py)는 이 유닛의 노트북과 함께 참조용으로 포함되어 있습니다.

## Guidance

무조건부(Unconditional) 모델은 생성되는 내용을 제어할 수 있는 방법이 많지 않습니다. 생성 과정을 조정하는 데 도움이 되는 추가 입력을 받는 조건부(Conditional)모델(다음 섹션에서 자세히 설명)을 훈련할 수 있지만, 이미 훈련된 무조건부 모델을 사용하려는 경우 어떻게 해야 할까요? 생성 과정의 각 단계에서 모델 예측을 일부 가이드(guidance) 함수와 비교하여 평가하고 최종 생성된 이미지가 원하는 대로 수정되도록 하는 프로세스인 가이드(guidance)를 입력합니다.

![guidance의 예시 이미지](guidance_eg.png)

이 가이드 기능은 거의 모든 것이 될 수 있으므로 강력한 기술입니다! 노트북에서는 간단한 예제(위의 예시 출력에서 볼 수 있듯이 색상을 제어하는 것)부터 텍스트 설명을 기반으로 가이드 생성을 할 수 있는 CLIP이라는 강력한 pre-trained 모델을 활용하는 예제까지 구축해 보았습니다. 

## Conditioning

가이드는 무조건적 디퓨전 모델에서 추가 정보를 얻을 수 있는 좋은 방법이지만, 학습 중에 사용할 수 있는 추가 정보(예: 클래스 레이블 또는 이미지 캡션)가 있는 경우 이를 모델에 제공하여 예측에 사용할 수 있도록 할 수도 있습니다. 이렇게 하면 **조건부** 모델이 생성되며, 추론 시점에 조건으로 입력되는 내용을 제어하여 제어할 수 있습니다. 이 노트북에는 클래스 레이블에 따라 이미지를 생성하는 방법을 학습하는 클래스 조건부 모델의 예가 나와 있습니다. 

![conditioning의 예시](conditional_digit_generation.png)

이 조건부 정보를 전달하는 방법에는 다음과 같은 여러 가지가 있습니다.
- UNet에 대한 입력에서 추가 채널로 공급합니다. 이는 세그멘테이션(segmentation) 마스크, 뎁스 맵 또는 블러 이미지(복원/초해상도 모델의 경우)과 같이 컨디셔닝 정보가 이미지와 동일한 형태(shape)일 때 자주 사용됩니다. 다른 유형의 컨디셔닝에도 사용할 수 있습니다. 예를 들어 노트북에서는 클래스 라벨을 임베딩에 매핑한 다음 입력 이미지와 동일한 너비와 높이로 확장하여 추가 채널로 공급할 수 있습니다.
- 임베딩을 생성한 다음 하나 이상의 내부 레이어의 출력에서 채널 수와 일치하는 크기로 투영한 다음 해당 출력에 추가합니다. 예를 들어 타임스텝 컨디셔닝이 처리되는 방식입니다. 각 리셋 블록의 출력에는 예상 타임스텝 임베딩이 추가됩니다. 이는 클립 이미지 임베딩과 같은 벡터를 컨디셔닝 정보로 사용할 때 유용합니다. 주목할 만한 예로 [스테이블 디퓨전(Stable Diffusion)의 '이미지 변형'버전](https://huggingface.co/spaces/lambdalabs/stable-diffusion-image-variations)이 있습니다.
- 컨디셔닝으로 전달된 시퀀스에 '참석'할 수 있는 교차 어텐션(cross-attention) 레이어를 추가합니다. 이 기능은 컨디셔닝이 일부 텍스트 형태일 때 가장 유용합니다. 트랜스포머 모델을 사용하여 텍스트를 임베딩 시퀀스에 매핑한 다음 UNet의 교차 어텐션 레이어를 사용하여 이 정보를 노이즈 제거 경로에 통합할 수 있습니다. 3단원에서는 스테이블 디퓨전이 텍스트 컨디셔닝을 처리하는 방법을 살펴보면서 이 기능이 실제로 어떻게 작동하는지 살펴보겠습니다.


## Hands-On Notebook

| Chapter                                     | Colab                                                                                                                                                                                               | Kaggle                                                                                                                                                                                                   | Gradient                                                                                                                                                                               | Studio Lab                                                                                                                                                                                                   |
|:--------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fine-tuning and Guidance                                | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/huggingface/diffusion-models-class/blob/main/unit2/01_finetuning_and_guidance.ipynb)              | [![Kaggle](https://kaggle.com/static/images/open-in-kaggle.svg)](https://kaggle.com/kernels/welcome?src=https://github.com/huggingface/diffusion-models-class/blob/main/unit2/01_finetuning_and_guidance.ipynb)              | [![Gradient](https://assets.paperspace.io/img/gradient-badge.svg)](https://console.paperspace.com/github/huggingface/diffusion-models-class/blob/main/unit2/01_finetuning_and_guidance.ipynb)              | [![Open In SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/huggingface/diffusion-models-class/blob/main/unit2/01_finetuning_and_guidance.ipynb)              |
| Class-conditioned Diffusion Model Example                               | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/huggingface/diffusion-models-class/blob/main/unit2/02_class_conditioned_diffusion_model_example.ipynb)              | [![Kaggle](https://kaggle.com/static/images/open-in-kaggle.svg)](https://kaggle.com/kernels/welcome?src=https://github.com/huggingface/diffusion-models-class/blob/main/unit2/02_class_conditioned_diffusion_model_example.ipynb)              | [![Gradient](https://assets.paperspace.io/img/gradient-badge.svg)](https://console.paperspace.com/github/huggingface/diffusion-models-class/blob/main/unit2/02_class_conditioned_diffusion_model_example.ipynb)              | [![Open In SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/huggingface/diffusion-models-class/blob/main/unit2/02_class_conditioned_diffusion_model_example.ipynb)              |

이쯤 되면 함께 제공되는 노트북으로 시작하기에 충분합니다! 위의 링크를 사용해 원하는 플랫폼에서 노트북을 사용하세요. fine-tuning은 상당히 계산 집약적이므로, Kaggle이나 Google Colab을 사용하시는 경우 런타임 유형을 'GPU'로 설정해야 최상의 결과를 얻을 수 있습니다.

대부분의 자료는 _**Fine-tuning and Guidance**_ 에 있으며, 이 두가지 주제를 예제를 통해 살펴봅니다. 이 노트북에서는 새로운 데이터에 대해 기존 모델을 fine-tuning하고, 가이드를 추가하고, 그 결과를 Gradio 데모로 공유하는 방법을 보여줍니다. 다양한 fine-tuning을 쉽게 실험할 수 있는 스크립트 ([finetune_model.py](https://github.com/huggingface/diffusion-models-class/blob/main/unit2/finetune_model.py))가 제공되고, 🤗 스페이스에서 나만의 데모를 공유할 때 템플릿으로 사용할 수 있는 [예시 스페이스](https://huggingface.co/spaces/johnowhitaker/color-guided-wikiart-diffusion)를 제공합니다

_**Class-conditioned Diffusion Model Example**_ 에서는 MNIST 데이터 세트를 사용하여 클래스 레이블에 조건부 디퓨전 모델을 만드는 간단한 작업 예시를 보여줍니다. 모델에 노이즈 제거 대상에 대한 추가 정보를 제공함으로써 추론 시 생성되는 이미지의 종류를 나중에 제어할 수 있다는 핵심 아이디어를 최대한 간단하게 설명하는 데 중점을 두었습니다. 

## Project Time

 _**Fine-tuning and Guidance**_ 노트북의 예시를 따라 나만의 모델을 fine-tuning 하거나 기존의 모델을 선택해 새로운 가이드 기술을 보여줄 수 이는 Gradio 데모를 만들어 보세요. 여러분의 데모를 Discord, 트위터 등에 공유하여 다른 분들의 감탄을 자아내게 하는 것을 잊지 마세요!

## Some Additional Resources

[Denoising Diffusion Implicit Models](https://arxiv.org/abs/2010.02502) - DDIM 샘플링 방법 도입(DDIM 스케줄러 사용)
 
[GLIDE: Towards Photorealistic Image Generation and Editing with Text-Guided Diffusion Models](https://arxiv.org/abs/2112.10741) - 텍스트에 대한 디퓨전 모델 컨디셔닝 방법 도입

[eDiffi: Text-to-Image Diffusion Models with an Ensemble of Expert Denoisers](https://arxiv.org/abs/2211.01324) - 생성된 샘플의 종류를 더욱 세밀하게 제어하기 위해 얼마나 다양한 종류의 컨디셔닝을 함께 사용할 수 있는지 보여줍니다.

더 좋은 자료를 찾으셨나요? 알려주시면 이 목록에 추가해드리겠습니다.
