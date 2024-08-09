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
![image](https://github.com/user-attachments/assets/ef5d8d6c-f3b7-478f-92aa-8ff557811d90)
<br>
- 1500k img결과 눈, 코, 입의 위치와 대략적인 형태만 생성
- Pixel2style2pixel로 super resolution 돌린 결과 서양인에 특화되어 나타남.
- 8개의 GPU로 돌렸을 때 의미있는 이미지를 얻으려면 5000 kimg필요<br>
![image](https://github.com/user-attachments/assets/1219ed02-2d2a-4c00-a8cb-e42ad270740b)
<br>
- 증명사진을 데이터셋에 추가하여 모델 다시 학습
- Face alignment조정을 통해 모든 사진 alignment 조정
- 사전학습모델의 가중치만을 가져와서 사용
- Freeze-D 사용하여 판별자층을 얼려서 이전의 데이터를 최대한 활용
<br><br><br>
-최종결과<br>
<img src=img/356433456-a72b1b83-8f77-4146-854c-638a879b6cbe.png>
---
**발표자료**
- Colab Notebook(https://github.com/phdo94/Stylebot_project/blob/master/AI_02_%EC%8A%A4%ED%83%80%EC%9D%BC%EB%B4%87_project_%EB%B0%95%ED%97%8C%EB%8F%84.ipynb)
- pptx(https://github.com/phdo94/Stylebot_project/blob/master/AI_02_%EC%8A%A4%ED%83%80%EC%9D%BC%EB%B4%87_project_%EB%B0%95%ED%97%8C%EB%8F%84.pptx)
- 영상(https://github.com/phdo94/Stylebot_project/blob/master/AI_02_%EC%8A%A4%ED%83%80%EC%9D%BC%EB%B4%87_project_%EB%B0%95%ED%97%8C%EB%8F%84.mp4)


