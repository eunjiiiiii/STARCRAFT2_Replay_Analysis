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
<br>

**1) 데이터 탐색** 

<br>
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



**2) 데이터 전처리**

<br>
<br>

**(1) 이상치 제거**

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/2faac914-4153-4371-8127-1078bb6e5aae) 

<br>
MaxTimeStamp의 범위가 가장 넓고, TotalHours에서 매우 큰 이상치가 존재함을 확인했습니다. <br>  <br>

각 변수의 왜도, 첨도를 확인한 결과, West et al(1995)의 정규분포 기준에 따라 왜도, 첨도가 높은 변수들을 다음과 같이 추렸습니다. <br>
![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/9024f182-b63a-4e6c-9ff0-10fc15934fb6)
 <br>
![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/3ac564a4-4830-49c7-ad32-6f76cb357049)
 <br>
우리는 이상치를 판단하는 것이 목적이므로 첨도가 큰 변수들이 이상치를 가지고 있을 가능성이 크다고 판단하여 첨도가 큰 7개의 변수들을 boxplot을 그려보며 의미와 더불어 이상치를 판단했습니다. 
<br>
- TotalHours – 첨도 : 3321.688<br>
   TotalHours의 boxplot을 그려봤을 때, 다른 값들의 분포와 아주 동떨어진 이상치가 존재함을 확인했습니다. 평균이 960.4218, Q1, Median, Q3가 각각 300, 500, 800인 것에 비해, max값이 1,000,000으로 매우 차이가 크게 났습니다. TotalHours의 75%, 80%, 90%, 95%, 99% 값을 확인해본 결과, 99%의 값도 2520이므로, max값이 이상치임을 확인할 수 있었습니다. 따라서 우리는 데이터의 양이 적기 때문에 우선적으로 max값만 이상치 처리하여 제거하였습니다.<br>
   TotalHours의 max값을 제거한 후 boxplot을 그려본 결과 이전보다 비교적 4분위수 범위가 잘 보임을 확인했으나 여전히 분포가 치우쳐져 있으므로 로그 변환을 진행하여 범위를 좁히기로 하였습니다.<br>

- MinimapAttacks–첨도 : 45.542<br>
MinimapAttacks의 boxplot을 그려봤을 때, 다른 값들의 분포와 조금 동떨어진 이상치가 존재함을 확인했습니다. 평균이 0.000098, Q1, Median, Q3가 각각 0.000166, 0.00, 0.000040, 0.000119인 것과 비교했을 때, max값이 0.003019로 차이는 나지만 애초에 범위 값이 적었습니다. <br>
 MinimapAttacks의 75%, 80%, 90%, 95%, 99% 값을 확인해본 결과, 99%의 값이 0.000769로 max값이 큰 이상치가 아님을 확인하였습니다. 따라서 이상치 제거를 진행하지 않고, 추후에 모델링을 통해 정규성을 만족하지 않을 경우 이상치 제거를 추가로 진행하기로 결정했습니다.<br>
 ComplexAbilityUsed, MinimapRightClicks,SelectByHotkeys, GapBetweenPACs변수들도 MinimapAttacks의 경우와 비슷하므로 이상치 제거를 진행하지 않고 추후에 모델링 과정에서 필요하면 이상치 제거를 추가로 진행하는 것으로 결정했습니다.<br>

- HoursPerWeek– 첨도 : 16.737<br>
  HoursPerWeek의 boxplot을 그려봤을 때, 0인 값들이 존재함을 확인할 수 있었습니다. HoursPerWeek는 일주일에 게임에 쓴 시간으로 0인 값이 존재할 수 없기 때문에, 이상치로 판단하였다. 평균이 15.910752, Q1, Median, Q3가 각각 8, 12, 20인 것과 비교했을 때, max값이 168로 차이가 크게 남을 확인할 수 있었습니다.<br>
 HoursPerWeek의 75%, 80%, 90%, 95%, 99% 값을 확인해본 결과, 99%의 값이 56이므로, max값 168이 이상치임을 확인할 수 있었습니다. 따라서, TotalHours와 마찬가지로 데이터의 양이 적기 때문에 우선적으로 max값과 0인 값만 이상치 처리하여 제거하였습니다.<br>
 HoursPerWeek의 max값을 제거한 후 boxplot을 그려본 결과 범위가 많이 좁혀졌고, 왜도도 2.22정도로 작아졌기 때문에 HoursPerWeek는 max값과 0만 제거하는 걸로 이상치 처리를 진행했습니다.<br>

- 그 외 0인 값이 존재하는 변수들<br>
  그 외 0인 값이 존재하는 변수들을 확인했을 때, 의미상 0인 값이 존재할 수 있으므로 0인 값을 이상치처리하지 않았습니다.<br>
  

**(2) 결측치 대체**<br>
결측치는 모두 원데이터의 LeagueIndex가 8인 데이터였기 때문에, 바로 아래 단계인 LeagueIndex가 7인 데이터의 각 변수의 평균으로 대체했습니다. 
<br>
<br>
**(3) 로그변환**<br>
Hourperweek와 totalhours의 범위가 0과 1 사이인 다른 변수들에 비해 매우 크므로 로그변환을 통해 범위를 축소하였습니다.<br>

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/1d3c8365-3915-4bce-8ad9-d372882f3f3f)

그 다음 대부분의 변수가 조금 좌편향된 점을 반영해 0.05% 미만의 값까지 제거해주어 분포를 비슷하게 만들어주었습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/8305c665-3024-456d-86fb-a0e207b68ba7)

<br>

**(4) LeagueIndex 값 수정**<br>
로지스틱 회귀를 사용하기 위해 종속변수인 리그인덱스를 이변환하여 1~4까지는 0, 5~8까지는 1값으로 변환하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/efd30ec6-c97c-4169-a0c2-2284f34c918c)

<br>

**(5) 중복 행 제거**<br>
 GameID를 제외한 모든 변수값이 같은 행이 2개 존재하여 한 행만 남기고 제거하였습니다.
 <br>

 
**(6) 변수값 수정 및 파생변수 생성** <br>
변수들의 시간단위를 초로 통일하기 위해 PAC 단위의 변수들에는 88.5를 곱하고, 밀리세컨즈에는 1000을 나누고, APM은 60을 나누어 모두 초단위 값으로 변환하였습니다.
<br>
파생 변수는 총 4개를 생성하였습니다. 먼저 어느 액션이 게임에 영향을 미쳤는지 비교하기 위해 미니맵 입력, 단축키 입력을 각각 총 액션인 APS로 나누어 hotkeyrate, minimaprate를 생성하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/14eb56ff-257f-4bac-9883-797dbbd764ed)

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/3872e43f-a014-41c5-aaae-ee18f69b1d80)

<br>
pac란 인지를 시작하고 행동하기까지의 주기로, pac 자체보단 pac가 시행되는 시간이 중요하다고 판단하여 GapBetweenPACs와 ActionLatency를 합쳐 PACtime를 생성하였습니다. 이 변수를 통해 얼마나 빠르게 인지하고 행동했는지를 알 수 있게끔 하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/1f2ce4a6-9319-4119-9b9d-e951e1f58c27)

일반 상황에서 총 유닛 생성을 대표하는 변수를 생성하기 위해, 특수한 상황에서 생성되는 complexunitmade를 제외한 나머지 두 유닛생성변수를 합하여 Unitmade를 생성하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/b98eedf7-e3be-498e-bd6c-729bcf943015)

<br>
변수 삭제는 총 3개를 진행하였습니다. GameID는 모든 row에서 유니크한 인덱스 역할을 하기 때문에 , Age는 종속변수와의 매우 낮은 상관계수와 더불어 티어마다 고르게 분포됨, ComplexAbilitiesMade는 2/3가 0값을 가질뿐더러 티어마다 고르게 분포됨 고 같은 이유로 삭제하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/4b0e4414-2b69-4e2d-ab1f-6d952cc21f93)

<br>
<br>
<br>
## 3. 모델링
---
모델링 하기 전 최종 데이터의 왜도, 첨도를 확인한 결과 ComplexAbilitiesUsed를 제외한 나머지 변수들은 정규성을 만족하였습니다.
<br>
데이터 분할은 train 7, test 3 비율로 진행하였고, 로지스틱 회귀의 특성상 과적합 문제가 발생할 수 있기에 Standard Scaling을 진행하였습니다.
<br>
그리고 분류 정확도를 높이기 위해 함수를 생성하여 최적의 임계값을 찾아 정확도를 구하였습니다.
<br>
먼저 full model입니다. 종속 변수는 리그인덱스, 설명변수는 전처리 후 13개의 변수로 모델링을 진행하였습니다. 그 결과 최적 임계값인 0.558로 조정 후 정확도가 0.807, AUC는 0.89였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/66368d56-a892-446c-bb17-169104b04e98)

<br>
변수 중요도를 참고하여 총 세 개의 변수를 제외하였습니다. 먼저 totalmapExplored, 매우 낮은 변수 중요도와 더불어 게임마다 지형적 특징이 다를수 있다는 점을 감안해 제외하였습니다. hoursperweek, 역시 매우 낮은 변수 중요도와 함께 게임 시간을 의미하는 totalhours가 존재하여 제외하였습니다. 마지막으로 aps, hotkeyrate, minimapkeyrate 생성으로 인한 다중공선성과 더불어 이 두 변수가 높은 변수 중요도를 보이기에 제외하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/08a3c79b-abca-49be-aed8-98ef86e06504)

<br>
변수 선택을 진행 한 후 1차 모델링을 한 결과 최적 임계값 0.503로 조정 후 정확도가 0.804, AUC는 0.88이 나왔습니다. 

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/69605f29-d67a-46b5-9b30-074f3caa5225)

<br>
위 단계에서 변수 의미상 PAC 내에서의 Action은 통일된 PACtime에서 결정된 값이 아니기 때문에 변수중요도가 낮게 나왔다고 판단된 ActionsInPAC를 제외하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/95ab7cdf-b7e9-483e-9ba2-8c3b00af0274)

<br>
그 다음 독립변수 전체에 대한 10분위 분석을 진행한 결과 complexunitsmade가 단조 감소하지 않고 두 가지 정도의 범주로 분류할 수 있다고 판단하여 1~5분위까지 1, 6~10분위까지 2로 변수를 범주화 하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/db7392bc-e0c8-487a-93e1-31509b2a3592)

<br>
그 후, 2차 모델링을 한 결과 최적 임계값 0.490로 조정 후 정확도가 0.802, AUC는 0.88이 나왔습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/8928607b-5813-495c-b771-4b1a02ab55e8)

<br>
변수들을 계속 제외하였음에도 불구하고 정확도와 AUC가 유지되었고, confusion matrix 결과 0을 정확히 분류할 확률은 85퍼센트, 1을 정확히 분류할 확률은 76%를 기록하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/395a1bde-f69e-4ef6-bb88-544d003d22fb)

<br>
2차 모델로 10-fold cross-validation을 진행한 결과 정확도의 평균은 0.7992로 기존 정확도와 유사한 값을 확인하였습니다.
<br>
최종모델 변수 선택과 비교를 위해 full model에서의 후진 선택법 변수 선택을 진행한 결과  totalmapexplored -> hoursperweek -> aps 순으로 변수를 제거하였고, 조정 r squared 값 역시 증가하고 있는 양상을 보여 본 조의 변수 선택법과 매우 유사하다는 점을 확인하였습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/62fb81b6-8134-42c9-8048-895ae720722c)

<br>
<br>
<br>
## 4. 결론
---
최종모형의 오즈비를 토대로 각 변수 1단위 증가 시 상위랭크 확률 증가비율을 구할 수 있었고, 상위 5개 변수를 본 결과 PACtime은 증가할수록 상위 랭크일 확률이 낮아지며, totalhours, numberofpacs, hotkeyrate, unitsmade는 증가할수록 상위 랭크일 확욜이 높아짐을 확인할 수 있었습니다.
<br>
train data의 10분위 분석표를 살펴 본 결과 모든 분위에서 단조롭게 actual 값이 줄어들고 있으므로 모형은 train셋에 양호하게 작동됨을 알 수 있습니다

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/0525c6c2-6350-4739-906e-f603ffe2eaae)

<br>
test data 역시 같은 이유로 양호하게 작동됨을 알 수 있습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/80185991-7839-485a-a949-ea2d9a7637c9)

<br>
이익도표를 보면 5그룹까지는 구축된 모형이 평균모형보다 우수함을 볼 수 있고, 전체 데이터만의 50%만으로 모형을 구축하지 않았을 때모다 61% 더 많은 상위 랭크 게임 플레이어를 찾을 수 있었습니다.

![image](https://github.com/eunjiiiiii/STARCRAFT2_Replay_Analysis/assets/47842737/7aa00325-be37-4225-8dc7-558e88d2a727)

