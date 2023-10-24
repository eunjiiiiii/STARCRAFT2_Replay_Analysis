# STARCRAFT2_Replay_Analysis
통계학과 데이터마이닝 수업 프로젝트

## 1. 프로젝트 주제 선정 및 목적
---
‘StarCraft2ReplayAnalysis’ 데이터 셋을 통해 게임 내 랭크에 영향을 요인들에 대하여 탐구하고 통계적 분석을 실시하였습니다.

분석의 방향은 다음과 같습니다.<br>
**게이머의 객관적 지표 개발 -> 랭크 예측 -> 고객의 유형과 특성 파악, 유지·보수 및 업데이트, 새로운 유저 창출을 위한 마케팅 활동**<br>
스타크래프트와 같은 RTS 장르는 '더 다양한 전략'과 '더 강한 유닛'을 뽑고 이를 컨트롤하는 '실력'이 게임을 플레이하는 주된 방법이기 때문에 여러 작업을 동시에 수행할 수 있는 능력이 중요합니다. <br>
따라서 게임에 큰 영향을 미치는 요소는 인지(지각, 행동, 주기)라는 것을 통해 본 조는 **각 플레이어의 인지능력**에 중점을 두고 분석을 진행하였습니다.<br>

데이터셋의 사용자의 인지 부하 능력을 나타낼 수 있는 **PAC(Perceptio Action Cycle, 인지를 시작하고 행동하기까지의 동작)**라는 변수를 데이터셋 내의 다른 행동 척도를 나타내는 변수들과 함께 분석에 주로 사용하였습니다. <br><br>

## 2. 데이터 탐색과 전처리
---
SkillCraft2 데이터는 서로 다른 분위에서 플레이된 스타크래프트2 게임 데이터셋입니다.
1:1 모드에 대한 데이터만 기록되었고, 게임을 진행하는 맵 또한 동일한 사이즈입니다.
온라인 설문조사를 통해 초보자부터 전문가에 이르는 8개로 나누어진 게임 내의 리그에서 3,395명의 각기 다른 분위에 속한 플레이어들에 대한 데이터를 모집했습니다.<br><br>

변수 중 일부 데이터는 타임스탬프라는 시간 단위를 사용했는데, 이는 1초가 88.5타임스탬프를 의미합니다. 데이터셋 내에 타임스탬프당 변수들과 분당 변수들이 혼재되어있으므로 이를 1초 단위로 통일시켰습니다. <br>

**1) 데이터 탐색** 
<br>
(1) 반응속도와 명령어 입력 관련 변수들(APM, NumberOfPACs, GapBetweenPACs,       ActionsInPAC)<br>
정규분포에 가깝게 분포되어 있으며, 타겟 변수인 LeagueIndex와 높거나 낮은 상관관계를 가집니다. RTS 장르는 전략과 전술이 중요하므로 이 변수들은 그러한 특성을 나타내는 변수들이기에 상관관계가 높게 나타난다고 판단했습니다.  <br>

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/047e48bd-822a-46ff-86f8-de6e3dd707b9)


 
![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/037bd532-53b6-4914-b5c6-785b3fed7b61)

<br>
(2) Hotkeys(단축키) 관련 변수들 (SelectByHotkeys, AssignToHotkeys, UniqueHotkeys)<br>
AssignToHotkeys와 UniqueHotkeys는 어느 정도 정규분포의 형태를 띠고 있으나 left skewed 되어 있습니다. <br> <br>

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/a997ebbc-0c09-4ec8-aa23-fda2e6cc681c)

 <br>

(3) Map 관련 변수 (MinimapAttacks, MinimapRightClicks, TotalMapExplored)<br>
MinimapAttacks와 MinimapRightClicks는 대부분의 값이 0에 매우 가깝게 분포한 것으로 보입니다. 그에 반해 TotalMapExplored는 정규분포에 가까운 분포를 보입니다.
<br> 

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/a2c2e198-ae43-434b-b472-d9ba39f7e044)


<br>
(4) 그 외 변수들<br>
Unit관련 변수들은 대체로 정규분포에 가까우면서도 left skewed 되어있는 형태를 가지고 있습니다. 게임플레이 시간을 표시한 변수들(HoursPerWeek, TotalHours)은 max 값이 비정상적으로 큰 값인 것으로 보입니다. <br> 

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/c0fb0484-2cc6-455b-b72c-e61bb7ff2572)


<br>
**2) 데이터 전처리 **
 <br>
(1) 이상치 제거

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/2faac914-4153-4371-8127-1078bb6e5aae) 

<br>
MaxTimeStamp의 범위가 가장 넓고, TotalHours에서 매우 큰 이상치가 존재함을 확인했습니다. <br>  <br>

각 변수의 왜도, 첨도를 확인한 결과, West et al(1995)의 정규분포 기준에 따라 왜도, 첨도가 높은 변수들을 다음과 같이 추렸습니다. <br>
![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/9024f182-b63a-4e6c-9ff0-10fc15934fb6)
 <br>
![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/3ac564a4-4830-49c7-ad32-6f76cb357049)
 <br>
우리는 이상치를 판단하는 것이 목적이므로 첨도가 큰 변수들이 이상치를 가지고 있을 가능성이 크다고 판단하여 첨도가 큰 7개의 변수들을 boxplot을 그려보며 의미와 더불어 이상치를 판단했습니다. 

- TotalHours – 첨도 : 3321.688
   TotalHours의 boxplot을 그려봤을 때, 다른 값들의 분포와 아주 동떨어진 이상치가 존재함을 확인했습니다. 평균이 960.4218, Q1, Median, Q3가 각각 300, 500, 800인 것에 비해, max값이 1,000,000으로 매우 차이가 크게 났습니다. TotalHours의 75%, 80%, 90%, 95%, 99% 값을 확인해본 결과, 99%의 값도 2520이므로, max값이 이상치임을 확인할 수 있었습니다. 따라서 우리는 데이터의 양이 적기 때문에 우선적으로 max값만 이상치 처리하여 제거하였습니다.
   TotalHours의 max값을 제거한 후 boxplot을 그려본 결과 이전보다 비교적 4분위수 범위가 잘 보임을 확인했으나 여전히 분포가 치우쳐져 있으므로 로그 변환을 진행하여 범위를 좁히기로 하였습니다.

- MinimapAttacks–첨도 : 45.542
MinimapAttacks의 boxplot을 그려봤을 때, 다른 값들의 분포와 조금 동떨어진 이상치가 존재함을 확인했습니다. 평균이 0.000098, Q1, Median, Q3가 각각 0.000166, 0.00, 0.000040, 0.000119인 것과 비교했을 때, max값이 0.003019로 차이는 나지만 애초에 범위 값이 적었습니다. 
 MinimapAttacks의 75%, 80%, 90%, 95%, 99% 값을 확인해본 결과, 99%의 값이 0.000769로 max값이 큰 이상치가 아님을 확인하였습니다. 따라서 이상치 제거를 진행하지 않고, 추후에 모델링을 통해 정규성을 만족하지 않을 경우 이상치 제거를 추가로 진행하기로 결정했습니다.
 ComplexAbilityUsed, MinimapRightClicks,SelectByHotkeys, GapBetweenPACs변수들도 MinimapAttacks의 경우와 비슷하므로 이상치 제거를 진행하지 않고 추후에 모델링 과정에서 필요하면 이상치 제거를 추가로 진행하는 것으로 결정했습니다.

- HoursPerWeek– 첨도 : 16.737
  HoursPerWeek의 boxplot을 그려봤을 때, 0인 값들이 존재함을 확인할 수 있었습니다. HoursPerWeek는 일주일에 게임에 쓴 시간으로 0인 값이 존재할 수 없기 때문에, 이상치로 판단하였다. 평균이 15.910752, Q1, Median, Q3가 각각 8, 12, 20인 것과 비교했을 때, max값이 168로 차이가 크게 남을 확인할 수 있었습니다.
 HoursPerWeek의 75%, 80%, 90%, 95%, 99% 값을 확인해본 결과, 99%의 값이 56이므로, max값 168이 이상치임을 확인할 수 있었습니다. 따라서, TotalHours와 마찬가지로 데이터의 양이 적기 때문에 우선적으로 max값과 0인 값만 이상치 처리하여 제거하였습니다.
 HoursPerWeek의 max값을 제거한 후 boxplot을 그려본 결과 범위가 많이 좁혀졌고, 왜도도 2.22정도로 작아졌기 때문에 HoursPerWeek는 max값과 0만 제거하는 걸로 이상치 처리를 진행했습니다.

- 그 외 0인 값이 존재하는 변수들
  그 외 0인 값이 존재하는 변수들을 확인했을 때, 의미상 0인 값이 존재할 수 있으므로 0인 값을 이상치처리하지 않았습니다.
  
