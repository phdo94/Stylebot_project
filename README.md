# Stylebot_project
---
**프로젝트 목적**
- stylebot에서 운영하는 서비스에 필요한 안면이미지 생성.
- 기존 GAN 모델보다 안면이미지 생성에 더 특화되어있는 모델 필요.
- 또한 한국인의 얼굴에 특화되어 있는 모델이 필요하여 한국인 연예인 얼굴이미지로 강화학습 필요.
---
**사용 모델**
- StyleGAN v2:(https://github.com/NVlabs/stylegan2)
   - 기존의 GAN 모델은 메모리 제약 문제와 discriminator에서 과적합 문제가 발생함.
   - 이를 해결한 모델이 PGGAN인데, 저해상도 부터 고해상도까지 점차적으로 키워나가며 이미지를 학습하는 방식.
   - styleGAN은 PGGAN의 구조를 거의 그대로 사용하나, PGGAN과 다르게 Style-based generator를 사용함.
   - Style-based generator: Mapping Network와 Synthesis Network로 구성.

     - 1. Mapping Network<br>

      ![Alt text](https://miro.medium.com/v2/resize:fit:720/format:webp/0*6lEwRXKiA8WGRlEc.png)
       - PGGAN은 세부적인 attribute를 조절하지 못함. 이 단점을 해결하기 위해 latent space $Z$에서 sampling한
         $z$를 intermediate latent space $W$의 $w$로 mapping해주는 non-linear mapping network를 사용.
       - 이 mapping network는 8-layer MLP로 구성되고, 512차원 z를 512차원 w로 mapping해주는 역할을 함.
       - 이러한 mapping network를 사용하는 이유는 feature들의 disentanglement를 보장하기 위함.
     ![Alt text](https://velog.velcdn.com/images%2Fminjung-s%2Fpost%2Fe6367b47-18f4-4bbc-b53c-5c4d5b497ea0%2Fimage.png)<br>_이런 식으로 학습하기위해 non-linear mapping network 사용_<br>
      
      - 2. Synthesis Network

        - synthesis network는 4x4부터 1024x1024 resolution feature map을 만드는 총 9개의 style block으로 이루어짐.
        - 마지막에는 RGB로 바꿔주는 layer가 있어 이미지 channel에 맞춰주고 Style block의 input은 전 style block의 output인 featuremap이며 하나의 style block당 두번의 convolution을 진행.<br>
---
**프로젝트 결과**
- 
