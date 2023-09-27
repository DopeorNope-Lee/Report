# Report
Report for LLM research
---
- 1. `IA3방식`으로 KoAlpaca를 fine tuning한 한국어 LLM모델
 
  - 훈련 경과 표 ⏳
훈련방식 | 파라미터수 | 훈련 소요시간 | Inference time
-- | -- | -- | --
LoRA | 3670016 | 203 min | 2.1 sec
**IA3** | **802816** | **226 min** | **1.7 sec**

 - Dataset 💾

 - 훈련데이터셋 📚

  - 훈련에 Dataset은 기본적으로 KoAlpaca와 성능 비교를 위해 Beomi님의 KoAlpacav1.1 데이터셋을 활용

  - Fewshot Learning 평가
프롬프트별 정확도는 다음과 같습니다.

모델명 | 프롬프트1 | 프롬프트2
-- | -- | --
polyglot-5.8b| 0.556 | 0.680
koalpaca-polyglot-5.8b | 0.664 | 0.748
**K(G)OAT-polyglot** | **0.712** | **0.810**
