# CNF : https://ieeexplore.ieee.org/document/8014876

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Image Super Resolution Based on Fusing Multiple Convolution Neural Networks
--------------------------------------------------------------------------
* x는 LR Image를 의미하고 y는 HR Image를 의미하며 M은 네트워크의 수를 의미 
* 논문의 목표는 각각의 네트워크를 합치는 함수를 찾아 에러를 최소화 시키는 것

Pixel-Wise network Fusion(PWF)
------------------------------
![image](https://user-images.githubusercontent.com/61686244/94800220-41d66900-041f-11eb-9ede-b616653a2fcb.png)
* PWF 는 가장간단하게 생각이 된 방식
* 각각의 네트워크가 존재하고 LR Img가 각각의 네트워크를 통과해서 가중치가 곱해지고 더해진 뒤에 합이 이루어져 최종 output을 출력
* 논문의 저자는 3가지 방법 중 하나인 Pixel-Wise network Fusion 방법을 보여주었고 하지만 이 방법은 object detection 또는 분류같은 분야에 사용되는게 적합하고 더불어 복잡한 특징을 가지고 있어 적절한 방법이 아니라고 말을 해줌

Progressive Network Fusion(PNF)
-------------------------------
![image](https://user-images.githubusercontent.com/61686244/94800550-b6110c80-041f-11eb-863f-326920bf9077.png)
* Progressive Network Fusion, PNF로 첫번째 네트워크의 출력이 두번째 네트워크의 입력으로 들어가는 구조
* 위의 그림 구조가 PNF이며 2개의 각각의 네트워크를 융합하는 방법을 보여주고 있음
* . PNF는 각각의 네트워크에 들어가는 입력이 달라 다수의 트레이닝 셋이 필요하다는 점을 가지고 있음

Context-wise Network Fusion(CNF)
--------------------------------
![image](https://user-images.githubusercontent.com/61686244/94800582-c3c69200-041f-11eb-9e00-c06748072dcc.png)
* Context wise Network fusion 방법으로 이 융합 방법은 각각의 사전 학습된 네트워크 또는 사전 학습이 되지 않은 네트워크에 입력이 들어가고 나온 값을 추가적인 convolution 층인 Fusion Layer에 가우시안 분포에서 랜덤하게 초기화된 값을 곱하고 더한 뒤에 최종적으로 합을 통해 더해서 나오게됨
* 식 6처럼 표현을 할 수 있고 식 6의 Weight 와 Bias는 convolution 커널에서 나온 값
* 또한 개별 네트워크 부분을 사전학습한것과 사전학습하지 않은 부분을 비교해보면 논문에서 freezing이라고 표현을 한 개별 네트워크는 융합 층의 가중치만 학습을 하게되고 따라서 freezing이라고 표현한 개별 네트워크의 출력은 개별 네트워크를 통해서 나온 HR img의 합으로 표현을 할 수 있음
* Freezing이라고 효현이 안된 각각의 네트워크는 사전학습이 됨으로 식 6에 보이는 S가 변형이 돼서 사용이됨

사용된 SRCNN Layers
------------------
![image](https://user-images.githubusercontent.com/61686244/94800784-0be5b480-0420-11eb-8775-5f6ab1cbdd97.png)
* 본 논문의 저자는 사전 학습 된 SRCNN구조를 이용해서 개별네트워크에 적용
* 개별 네트워크들은 SRCNN구조를 이용하지만 이루고있는 층 수는 다르게 이루어져있음
* 기존 SRCNN은 patch extraction 9x9/ non-linear mapping 5x5/ reconstruction 5x5 3개의 층 구조로 이루어져 있음
* 중간에 3x3 커널을 가지는 층을 하나 더 추가해 4개의 층을 만들어 사용을 해보니 더 좋은 성능이 나오는 것을 확인했고 따라서 각각의 네트워크에 들어가는 SRCNN 의 층을 조금 더 깊게 만들어 사용을 해서 다양한 실험을 진행

Conclusions
-----------
![image](https://user-images.githubusercontent.com/61686244/94800932-451e2480-0420-11eb-8a91-7db48ebc39de.png)
* 3배 스케일의 이미지를 이용하여 3, 5 ,7 Layers를 가지는 각각의 네트워크를 사용하여 PWF, PNF, CNF, SRCNN을 비교한 결과
* PWF융합 방식은  SRCNN의 결과와 거의 차이가 나지 않은것으로 보아 논문의 저자가 말한 복잡한 특성으로 인해 적절하지 않은 융합 방식이라는 것을 알 수 있었고 PNF또한 각각의 네트워크를 이용하였을때 SRCNN비해 더 좋은 결과를 나타내는듯 싶었으나 7개의 층을 가지는 SRCNN보다 더 낮은 결과를 만들었음
* 최종적으로 논문에서 제안을해준 Context wise network fusion 방법을 이용했을때 각각의 네트워크를 사전학습 하거나 안한 두개의 결과 모두 다른 네트워크랑 비교했을때보다 더 좋은 성능을 만들어 내는 것을 확인 

![image](https://user-images.githubusercontent.com/61686244/94801122-8a425680-0420-11eb-8b79-4dff090ca825.png)
* 두번째 결과로 SRCNN과 다수의 네트워크를 CNF방법으로 융합한 것을 각각의 네트워크 층수를 다르게 비교한 결과
* SRCNN에서도 깊이가 길어질수록 더 좋은 성능을 나타낸 것처럼 CNF방법 또한 각각의 네트워크에 들어가는 SRCNN이 깊이가 길어질 수록 더 좋은 성능을 나타내는 것을 확인할 수 있음

![image](https://user-images.githubusercontent.com/61686244/94801201-a80fbb80-0420-11eb-9140-dbbd8edff661.png)
* CNF 융합 방법을 사용하되 가중치초기화를 어떻게 사용하는 지에 따라 비교한 것으로 사전학습방법을 사용한 CNF가 더 좋은 결과를 나타내는 것을 확인 

![image](https://user-images.githubusercontent.com/61686244/94801244-bb228b80-0420-11eb-9e65-dbdb5fbb4881.png)
* 다음으로는 다양한 SR 네트워크들은 3개의 데이터셋으로 비교해본 결과
* CNF 다수의 네트워크를 융합을 사용하는 방법이 우수한 결과를 낸것을 확인 할 수 있고 또한 VDSR같은경우 깊은 층을 만들어 성능을 높히는데 650000개의 파라미터를 사용한 반면 CNF 11+13+15 네트워크 경우 448059개의 파라미터수를 가져 훨씬 적은 파라미터수로 더 좋은 성능을 나타낸 것을 확인할 수 있음

![image](https://user-images.githubusercontent.com/61686244/94801359-e1482b80-0420-11eb-8034-6adc8b4a3600.png)
* 마지막으로 다양한 SR 네트워크 를 통해서 나온 3배 스케일의 IMG 비교한 사진으로 SRCNN은 블러한것을 확인할 수 있고 VDSR과 CNF 를 비교했을때 빨간색 점들을 더 CNF가 잘 표현한 것을 확인 할 수 있음

**다수의 Neural Networks 사용, 네트워크를 융합하는 방법 제안, 파라미터수 감소**
**각각의 네트워크를 사용한것 보다 CNF의 사용으로 정확도 향상**
**Deblur / Denoising에 사용 가능한 CNF**
**CNF 각각의 네트워크를 깊게 만들수록 더 좋은 결과 도출**
**Fusion layer를 사전 학습하는 방법이 더 좋은 결과 도출**

