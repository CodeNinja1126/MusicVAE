# MusicVAE

<img width="442" alt="1" src="https://user-images.githubusercontent.com/56903243/186313907-b1ed5752-4784-4c92-a9cd-c1be291c9859.png">

- 입력 시퀀스를 bidirection lstm으로 인코딩해 latent 코드 생성
- Hierarchical구조를 이용해 긴 시퀀스에 대해 posterior collapse문제를 해결했다.
- 2단계의 계층 구조로 이루어 지며 첫 단계에서 1마디에 대한 vector를 출력, 두번째 단계에서 16비트로 쪼개 결과를 출력한다.

# VAE

<img width="853" alt="2" src="https://user-images.githubusercontent.com/56903243/186313759-d5a915f5-e5d9-4dd2-bfbc-6fdb1a3896b8.png">

- $\sigma$에 $\epsilon$을 곱해 z가 나옴 이는 z는 latent variable임
- VAE나 AE나 z(latent)로 압축되었다가(encoder) 다시 원래의 데이터로 재구축(decoder)하는 두 단계가 존재한다.
- free-bits와 max-beta를 이용해, KL Loss의 영향력을 조절할 수 있음. free-bits를 증가, 혹은 beta값을 감소시키면 KL Loss의 영향력이 줄어들고, 좀더 랜덤적인 값을 기대해 볼 수 있음. 역도 가능.

# MusicVAE의 입력 포맷

- (T, 130)로 이뤄져 있다.(128음, 쉼, 이전음 늘임)
- 드럼 비트의 경우는 클래스가 9개.
    - hit는 바이너리값 [0,1]
    - velocity는 [0,1]
    - offsets는 [-0.5, 0.5], 그리고 [-1, 1]로 리스케일 된다.
        - offset은 드럼 히트들이 비트에 정확하게 떨어지지 않기 때문에 디테일한 위치 조정을 위한 상대적인 값이다.

# 구현 내용

- config의 기술된 4마디 groovae의 디코더를 Hierarchical구조로 변경해 모델링했습니다. 학습은 시간 관계상 2000스텝으로 조정해 학습을 수행하였습니다. 나머지는 4마디 groovae의 설정과 동일합니다.
- 제공된 튜토리얼 jupyter notebook을 참고해 4마디 샘플을 모델로부터 추출하고 이를 재생할 수 있도록 하였습니다. 학습이 된 체크포인트와 학습이 되지 않은 체크포인트를 비교해 학습이 진행되었는지를 확인했습니다.
