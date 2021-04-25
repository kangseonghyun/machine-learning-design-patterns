Hash feature를 사용할 경우 
- OOV (out-of-vocabluary) 문제를 해결? 할 수 있고
- hash buckets를 설정하여 메모리 절약이 가능 & 모델 크기 절약 가능
- cold start 문제 해결 가능 = ovv 랑 같은 의미?

발생할 수 있는 문제
- bucket collsion: 위의 문제를 해결하기 위해 정확도가 낮아진다. 따라서 위의 문제가 없는 경우 hash feature를 사용하지 않는다.
- skew: 왜도가 큰 경우 정확도가 낮아짐

추가
- aggregate feature를 추가해 모델의 인풋에 추가하면 더 높은 정확도를 기대할 수 있음
- bucket의 숫자를 튜닝해서 적절한 값을 찾아갈 수 있음
- md5 형식의 hash algorithm을 사용하지 말고 fingerprint hashsing algorithm을 사용해라(farm_fingerprint)
- empty bucket이 생길 수 있으므로 L2 정규화를 사용한다면 모델이 수치적으로 불안정해지지 않음



# Embedding
높은 카디널리티 데이터를 저차원 공간에 매핑한 데이터

- embedding은 pca를 대체할 수도 있음
- 항목의 친밀성과 맥락에 대한 정보를 얻음
- 고차원에서 저차원으로 표현된 것이기 때문에 정보의 손실이 있음
- 너무 작은 차원의 출력은 컨텍스트가 손실될 수 있고, 너무 많은 차원은 원-핫 인코딩과 비슷해짐
- 유니크한 카테고리 사이즈 제곱근의 1.6배를 임베딩 사이즈로 정할 수 있다. (실험적으로 구할 수 있으나 빠르게 확인하려면 이렇게 해보는것도 추천)
- 625 유니크 사이즈가 있다면, 25 * 1.6 = 40, 40으로 임베딩 사이즈를 임의로 지정
- autoencoder를 통해 embedding 차원을 구할 수 있음.


# featrue cross (fc)

- 두 개 이상의 범주 특성을 연결하여 형성된 특성
- 모델 학습 속도를 높이고, 모델 복잡성 줄일 수 있음
- numerical feature를 사용하려면 bucketize로 버켓팅을 한 뒤 fc 가능
- tensorflow에서 feature_column을 활용해서 가능
- fc를 수행하면 sparse한 vector가 생성됨. 따라서 fc한 컬럼에 embedding layer를 통해 피쳐를 다시 저차원으로 매핑하는것이 유용할 수 있음
- 정규화가 필요할 수 있음 (L1, L2), L2: 오버핏팅 제한 , L1: 희소 데이터 가중치 제한
- 두개의 특징이 상관 관계가 높은 경우 fc는 의미 없음
- 
