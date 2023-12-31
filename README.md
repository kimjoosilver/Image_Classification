# [오늘의 집] 사진을 이용한 유사 상품 검색 모델


프로젝트 구성원: 김주은, 송기훈, 이준석

프로젝트 기간: 23.03.13-23.03.31

작성자: 김주은


## 목차

[배경](#배경)

[데이터 소개 및 전처리](#데이터-소개-및-전처리)

[모델링](#모델링)
[Inference](#Inference)
[프로젝트를 마치며](#프로젝트를 마치며)


# 배경

![이미지 분류 pptx](https://github.com/kimjoosilver/Image_Classification/assets/87303227/b39dc0d6-bcf4-4936-91d1-9afa5379dde7)

패션 매거진 '메종마리에끌레르'에 따르면 올해 공간 트렌드는 커스터마이징 설계를 통해 자신의 취향과 개성을 반영하는 공간을 만드는 것이라고 합니다.

이에 트렌드를 잘 반영하고 7초에 하나씩 가구가 팔리는 플랫폼이자 인테리어계의 쿠팡이라고 불리는 오늘의 집 플랫폼을 주제로 선정하였습니다.

![이미지 분류 pptx](https://github.com/kimjoosilver/Image_Classification/assets/87303227/6e447b28-89ed-442d-97a3-d76bb2320c4c)

저희는 오늘의 집 어플을 보면서 “오프라인에서 (상품명을 모르는)마음에 드는 물건을 발견했을 때, 온라인에서 쉽게 찾는 방법이 없을까?” 생각을 하였습니다.

![이미지 분류 pptx (1)](https://github.com/kimjoosilver/Image_Classification/assets/87303227/2126613c-ec45-47e8-acc1-ed54400bee68)

이를 해결하기 위해 사진으로 원하는 물건을 검색함으로써 소비자에게 편리성을 제공하는 모델을 만들기로 하였습니다.

![이미지 분류 pptx (2)](https://github.com/kimjoosilver/Image_Classification/assets/87303227/50b630ad-747b-4651-9bc9-6bc262718c1a)

이 모델을 통해 얻을 수 있는 효과는 다음과 같습니다.

1. 편리하게 원하는 물건을 검색 이를 통한 고객 충성도 향상

2. 어플 사용자를 증가, e-커머스 업계에서 경쟁 우위를 확보


# 데이터 소개 및 전처리

### 데이터 출처 및 정제

데이터 수집은 오늘의집 홈페이지 6개의 카테고리(가구, 패브릭, 가전 디지털, 주방용품, 데코, 식물, 생활용품)에서 상위 20개 상품을 대상으로 진행하였습니다.

상품의 사진을 모으기 위해 사용자 후기에 있는 사진을 크롤링 하였고, 총 54개(사용자 후기가 부족한 상품 제외)의 상품에 대해 진행하였습니다.
![이미지 분류 pptx (3)](https://github.com/kimjoosilver/Image_Classification/assets/87303227/2843d85d-f683-4830-b960-421b035f59c9)


아래의 사진에서 볼 수 있는 것처럼, 사용자 후기 사진에 항상 정제된 데이터가 들어있지는 않았습니다.

데이터를 정제할 필요성을 느껴 데이터 선별 기준을 세우고 데이터 정제를 하였습니다.

데이터 선별 기준은 다음과 같습니다.

1. 사진 내에서 제품의 비율이 사진의 50% 이상을 차지할 것

2. 상품이 2개 이상 있는 경우 제외

ex. 매트리스의 사진에서 매트리스 위에 침구류가 올라가 있는 경우

3. 중복 사진은 제외
![이미지 분류 pptx (5)](https://github.com/kimjoosilver/Image_Classification/assets/87303227/8c5f8e5b-5270-4984-8c9b-c7f5206066fb)

세 명의 팀원이 며칠간 수작업으로 데이터 검수를 진행하였습니다😅

정제 후 최종 데이터의 규모와 형태는 다음과 같습니다.

![이미지 분류 pptx (6)](https://github.com/kimjoosilver/Image_Classification/assets/87303227/0d9412e0-69ed-473c-93a3-007591b4b3c4)

### 데이터 전처리

사용한 데이터 전처리 기법은 아래와 같습니다

1. 데이터 크기 조정 및 패딩

2. Zero-centering

라벨 별 데이터 수가 적지 않다고 생각하여 augmentation 과정은 생략하였습니다.


# 3. 모델링

### 모델
3가지 모델을 소프트 보팅으로 앙상블 하는 방법을 사용하여 상품 분류 모델을 만들었습니다.

3가지 모델은 다음과 같습니다.

#### VGG19; 

비교적 구조가 간단하고 사용이 편리해 ResNet 다음으로 많이 사용되는 모델입니다.

레퍼런스를 찾으면서 G*켓에서 유사 이미지 추천을 위해 vgg19를 사용한다는 것을 알게 되었습니다.

구조는 단순하지만 좋은 성능을 내는 모델입니다.

#### ResNet50

2015년 이미지넷에서 우승한 모델로 사람의 분류 성능을 뛰어넘은 모델입니다.

#### MobileNet V3

2017년 구글이 발표한 MobileNet은 모바일 기기에서 돌아갈 수 있을 만큼 경량화 된 모델입니다.

VGG16보다 32배 가볍고 27배 연산량이 적지만 성능은 비슷하다고 알려져 있습니다.

분류 성능을 올리기 위해 유명한 3가지 모델을 앙상블 하였지만, 상용화가 된다면 모바일에서 실행될 수 있어야 하기에 가벼운 MobileNet을 사용하는 것이 좋다고 생각합니다.

이 프로젝트에서는 물건의 종류(이불, 도마,..)를 구분하는 문제가 아니라 같은 종류의 물건이더라도 디자인을 구분하는 것이 핵심입니다.

예를 들어, 해당 이미지가 이불인지 책상인지 구분하는 문제가 아닌 꽃무늬 이불과 줄무늬 이불을 구분하는 것이 중요합니다.

따라서 이미지넷(약 1,400만개의 이미지, 21,000개의 라벨)으로 학습이 되어있는 VGG19, ResNet50, MobileNet V3 모델을 분류기를 재학습 시키는 Fine Tuning을 하였습니다.

### 파라미터 결정

Batch size(64, 128,256), Learning Rate(LearningRateScheduler, ReduceLROnPlateau), Dropout(Dropout 없이 학습, (Dense Layer) Dropout, Conv Layer Dropout)등의 하이퍼 파리미터를 조절해가며 최적의 성능을 보이는 모델을 찾았습니다.

### 결과

![1)](https://github.com/kimjoosilver/Image_Classification/assets/87303227/5d1d74c5-ca22-417e-8519-94bdfc699e2a)


VGG19, MobileNetV3, ResNet50 모델의 결과로 얻은 각 범주에 대한 확률을 평균내어(Soft Voting) 앙상블을 한 모델의 Top3 Accuracy가 가장 높게 나왔습니다.

Top1 Acuuracy 뿐만 아니라 Top3 Accuracy도 함께 본 이유는, 일치하는 항목 뿐만 아니라 비슷한 물건을 함께 추천하여 고객의 만족도를 높이기 위함입니다.


# 4. Inference

![슬라이드25](https://github.com/kimjoosilver/Image_Classification/assets/87303227/65415f95-6878-45d4-80eb-8362dfa58f46)



코랩 링크: https://drive.google.com/file/d/1h7okgIv83bBd9Z9C08hRauE3FHXR6SuN/view?usp=sharing



# 5. 프로젝트를 마치며

한계점 및 느낀점
1. 크롤링과 데이터 선별에 시간이 많이 소요되어 일부 항목으로 제한시켰습니다. 이를 모든 상품으로 일반화 시킬 필요성이 있습니다.

2. 이미지 분류를 위해 제품이 2개 이상 포함된 이미지를 제거하였습니다. 이미지 분류가 아닌 객체인식을 사용하여 여러 물건을 동시에 검색할 수 있도록 만드는 것이 서비스 측면에서 더 좋지 않을까 생각이 들었습니다.

![슬라이드28](https://github.com/kimjoosilver/Image_Classification/assets/87303227/3c9cc8c7-e9ab-4a24-bdb3-422ce9f6580d)

<출처>

p.10 [오늘의 집 어플 사진] https://www.designtool.org/ppt/?idx=8494379&bmode=view

오늘의집 홈페이지 사진: 오늘의 집
