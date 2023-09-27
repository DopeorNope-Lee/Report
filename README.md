# Report
Report for LLM research
---

# 1. `IA3방식`으로 KoAlpaca를 fine tuning한 한국어 LLM모델 : K(G)OAT
 
  - 훈련 경과 표 ⏳
훈련방식 | 파라미터수 | 훈련 소요시간 | Inference time
-- | -- | -- | --
LoRA | 3670016 | 203 min | 2.1 sec
**IA3** | **802816** | **226 min** | **1.7 sec**

 - 훈련데이터셋 📚

  - 훈련에 Dataset은 기본적으로 KoAlpaca와 성능 비교를 위해 Beomi님의 KoAlpacav1.1 데이터셋을 활용

  - Fewshot Learning 평가 : NSMC 데이터셋
프롬프트별 정확도는 다음과 같음

모델명 | 프롬프트1 | 프롬프트2
-- | -- | --
polyglot-5.8b| 0.556 | 0.680
koalpaca-polyglot-5.8b | 0.664 | 0.748
**K(G)OAT-polyglot** | **0.712** | **0.810**

 - 다만 한국어 LLM 평가 ko-eval-harness에서는 좋지 못한 평가 결과를 얻었음 -> 원작자 beomi도 koalpaca 평가결과를 올리지 않았으며, 이러한 이유가 데이터셋에 대한 편향으로 보임


# 2. `IA3방식`으로 Lamma2를 fine tuning한 영어 LLM모델 : A3Duck

- Platypus2는 Lamma2 기반의 모델로, 훈련당시, LORA방식을 활용하여 평가지표에서 좋은 성능을 가지고 있었음

- A3Duck은 lamma2-13b가 베이스 모델이며, Tokenizer는 lamma2의 Tokenizer를 사용하였음

 - 훈련데이터셋 📚

  - 훈련용 Dataset은 기본적으로 [Open-Platypus](https://huggingface.co/datasets/garage-bAInd/Open-Platypus)로, Platypus 원작자 데이터를 활용하였음

 
- Fewshot Learning 평가
 - 프롬프트별 정확도는 다음과 같음 (평가는 Platypus가 설명하는 결과 값 토대로 집결)

모델명 | MMLU (5-shot) | ARC (25-shot) | HellaSwag (10-shot) | TruthfulQA (0-shot) | AVG 
-- | -- | -- | -- | -- | -- 
Platypus2-13B | 59.5 | 62.88 | 83.19 | 52.69 | 64.56 
A3Duck-13b | 56.2 | 55.2 | 62.29 | 25.95 | 49.82 
 

- 결과론 적으로, 기존의 Platypus보다 A3 Duck은 낮은 성능 결과를 기록하였음.
  
- 이에 대한 이유로, 당시 훈련리소스 문제로 훈련 스텝이 기존보다 작았으며, 공개되지 않은 데이터셋에 대해서도 추가적으로 훈련이 필요하였지만, 그러지 못하였다고 생각되었음.


# 3. `COT프롬프트`르 활용하여 Lamma2-7B 기반의 `Tulpar-7b-v1`을 훈련시킨 COLAM


 - Tulpar-7b-v1는 Lamma2기반의 Small LLM중 평균 60%정도의 스코어를 기록하고 있는 모델임.

 - 다만, 모델 사이즈는 7b가 아닌 13b로 확인됨.

 - 이 모델을 가지고 LORA방식으로 샘플링한 데이터셋을 활용하여 훈련하였음.

 - 훈련데이터셋 📚
  - 훈련용 데이터셋은 HuggingFace에 공개된 KAIST의 COT프롬프트로 구성된 COT-Collection 을 기반으로 훈련하였음.(https://huggingface.co/datasets/kaist-ai/CoT-Collection)
  
  - 다만 180만개 가량 되는 데이터를 샘플링 하여 데이터를 재 구성하였으며, 샘플링 방식은 다음과 같음

   - 이 데이터셋은  216개의 여러 task에 대한 평가 데이터에서 수집된 데이터로 총 1,837,928개 가량으로 구성되어 있음.

   - 이를 샘플링 하기 위해서, 각각의 task들중 100개가 넘지 않는 데이터는 그대로 가져오고, 넘는 데이터는 각각 Task에서 무작위 추출하여 데이터를 구성하였음.

   - 이러한 과정을 걸쳐 총 21,297개의 데이터로 샘플링 진행하여 훈련시켰음.
  
   



- Fewshot Learning 평가
모델명 | MMLU (5-shot) | ARC (25-shot) | HellaSwag (10-shot) | TruthfulQA (0-shot) | AVG 
-- | -- | -- | -- | -- | -- 
Platypus2-13B | 59.5 | 62.88 | 83.19 | 52.69 | 64.56 
COLAM-13b | 56.2 | 55.24 | 62.28 | 37.63 | 49.82 
