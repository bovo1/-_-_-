# 반려동물 흉부 질환 판단 모델
---
https://colab.research.google.com/drive/1mVTBbbPZBbaMP_j1T6c3WQS8jFaXhPEz?usp=drive_link

YOLOv8 의 segmentation 모델을 사용하여 흉부질환을 판단하는 모델을 제작했습니다.  
모델은 사이즈 별로 나눠져있는데 그 중에서도 Small 모델을 사용했습니다.  

데이터는 AIHub의 반려동물 흉부질환 데이터셋을 다운로드 받아 일부를 사용했습니다.  
이유는 데이터셋의 (X-ray, CT)이미지 특성 상 클래스가 다르지만 이미지의 유사도가 굉장히 높습니다.  
이러한 이유로 데이터를 수집하고 정제한 회사에서는 클래스별로 segmentaion 모델과 object detection 모델을 나눠서 한개의 클래스만 훈련시켰습니다.  

YOLOv8 segmentation 모델은 segment와 object detection을 동시에 할 수 있는 모델이기에 데이터셋 전부를 잘 활용하고자 선택했습니다.  
YOLO가 요구하는 데이터구조를 만든 다음 훈련시킨 결과 질환의 클래스는 많지만 클래스 간 이미지 유사도가 높아 feature를 잘 뽑아내지 못하여 훈련 성과가 좋지 않았습니다.  
더 높은 훈련성과를 위해 hyperparmeter tuning을 해봤으나 그렇다 할 변화는 없었습니다.  
클래스의 질환 부위를 bounding box로 저장된 클래스는 박스에 포함된 정보가 불필요한 부분이 많고 의학 분야이므로 임의적으로 polygon으로 변환하기가 어려웠기에 제외했습니다.  
또한 Ch01 심비대 클래스에서 비정상적인 심장의 크기를 측정하기 위해 척추 polygon도 포함되어 있습니다.  
그러나 클래스간 이미지 유사도가 높고 다른 클래스에는 척추 polygon이 표시되어 있지 않으며 불필요하기 때문에 척추 polygon을 제외했습니다.  

25519개의 데이터를 10epoch로 훈련한 결과 다음과 같습니다.  
---
![confusion_matrix_normalized](https://github.com/bovo1/-_-_-/assets/110110403/fec7a403-e3f1-45dd-8c1f-47a3f735f0d2)

confusion_matrix를 보면 척추 polygon을 제외한 Ch01 클래스의 정확도가 많이 낮은 걸 알 수 있습니다.  
Ch03 또한 정확도가 높지는 않으나 데이터를 더 많이 훈련시키면 해결될 수 있을 것 같습니다.  

