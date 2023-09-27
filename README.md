# CoT-llama2 (Not Update;)
![CoT-llama2](./CoT-llama.png)  
**Chain-of-thought 방식을 활용하여 llama2를 fine-tuning**   

**CoT-llama-2k-7b:** [![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue)](https://huggingface.co/kyujinpy/CoT-llama-2k-7b)   
**KoCoT-2000:** [![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue)](https://huggingface.co/datasets/kyujinpy/KoCoT_2000)   
  
# Introduction
- LLM 관련 프로젝트를 진행하면서, "LLM의 근본적인 성능의 문제는 파라미터보다 데이터셋의 품질에 더 의미가 있지 않을까?"라는 생각을 하게 되었습니다.
- [Kaist-CoT](https://huggingface.co/datasets/kaist-ai/CoT-Collection)와 [LIMA](https://arxiv.org/abs/2305.11206)를 통해서 데이터의 개수보다 품질이 모델의 성능에 어느정도 기여한다는 사실을 깨닫게 되었습니다.

- 이것을 동기부여로 삼아서, LIMA에서 1000~2000개의 데이터셋을 활용한 것을 기반으로 Kaist-CoT 데이터셋 중 2000개를 추출하여 학습하기로 하였습니다!🙂🙂
- DeepL를 활용하여 번역하였고, 그 이후에 따로 직접적인 전처리를 하진 않고 바로 훈련에 도입했습니다.
- 이렇게 만들어진 **🥮KoCoT-2000🥮** 데이터셋을 활용하여 beomi님의 **llama-2-ko** 모델을 fine-tuning 하였습니다.
  
- 결과적으로 **CoT-llama-2k-7b** 모델을 만들게 되었고✌, 성능평가를 위해 Polyglot-Ko와 llama-2-ko 모델과 비교를 진행했습니다.🙂🙃  
- 본 연구는 (주)마커와 (주)미디어그룹사람과숲의 오픈소스 LLM 연구 컨소시엄에서 진행되었습니다.  

# Model Description  
### CoT-llama2-2k-7b
- **llama-2-ko-7B를 fine-tuning한 모델**
- **🥮CoT-llama2-2k-7b🥮** 모델은 LIMA의 방법론을 이용하여 Kaist에서 제작한 데이터셋을 기반으로 Chain-Of-Thought를 잘 수행할 수 있도록 튜닝한 모델입니다✌✌  

### Training Hyperparameters (as LIMA)  
| Hyperparameters | Value |  
| --- | --- |  
| batch_size | `64` |   
| micro_batch_size | `1` |  
| Epochs | `15` |  
| learning_rate | `1e-5` |  
| cutoff_len | `2048` |  
| lr_scheduler | `linear` |  
| base_model | `beomi/llama-2-ko-7b` |  

# Quick start
Colab Code: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1lRDSnHHVIzRW7RYRV3rha2QL2aMETRp7?usp=sharing)
  
# Datasets
[KoCoT_2000](https://huggingface.co/kyujinpy/CoT-llama-2k-7b)  
> Using DeepL Pro API, translate about [Kaist-CoT](https://huggingface.co/datasets/kaist-ai/CoT-Collection).  

# Performance
When I evaluated Ko-Platy, I used this [repo](https://github.com/Beomi/ko-lm-evaluation-harness).  
And, implement below code.
```
# In colab,
!python main.py \
    --model gpt2 \ 
    --model_args pretrained=..your_model_name.. \
    --tasks kobest_hellaswag,kobest_copa,kobest_boolq,kobest_sentineg \
    --device cuda:0 \
    --num_fewshot 0 # 5, 10, 25, ...
```
  
### COPA (F1)
| Model | 0-shot | 5-shot | 10-shot | 50-shot |
| --- | --- | --- | --- | --- |
| [Polyglot-ko-1.3b](https://huggingface.co/EleutherAI/polyglot-ko-1.3b) | 0.7196 | 0.7193 | 0.7204 | 0.7206 |
| [Polyglot-ko-3.8b](https://huggingface.co/EleutherAI/polyglot-ko-3.8b) | 0.7595 | 0.7608 | 0.7638 | 0.7788 |
| [Polyglot-ko-5.8b](https://huggingface.co/EleutherAI/polyglot-ko-5.8b) | 0.7745 | 0.7676 | 0.7775 | 0.7887 |
| [Polyglot-ko-12.8b](https://huggingface.co/EleutherAI/polyglot-ko-12.8b) | 0.7937 | 0.8108 | 0.8037 | 0.8369 |
| [Llama-2-Ko-7b 20B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.7388 | 0.7626 | 0.7808 | 0.7979 |
| [Llama-2-Ko-7b 40B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.7436 | 0.7927 | 0.8037 | 0.8259 |  
| [KO-platypus2-7B-EX(ours)](https://huggingface.co/kyujinpy/KO-Platypus2-7B-ex) | 0.7509 | 0.7899 | 0.8029 | 0.8290 |  
| **CoT-llama-2(ours)** | 0.7528 | 0.7888 | 0.7998 | 0.8210 |  
  
### HellaSwag (F1)
| Model | 0-shot | 5-shot | 10-shot | 50-shot |
| --- | --- | --- | --- | --- |
| [Polyglot-ko-1.3b](https://huggingface.co/EleutherAI/polyglot-ko-1.3b) | 0.5247 | 0.5260 | 0.5278 | 0.5427 |
| [Polyglot-ko-3.8b](https://huggingface.co/EleutherAI/polyglot-ko-3.8b) | 0.5707 | 0.5830 | 0.5670 | 0.5787 |
| [Polyglot-ko-5.8b](https://huggingface.co/EleutherAI/polyglot-ko-5.8b) | 0.5976 | 0.5998 | 0.5979 | 0.6208 |
| [Polyglot-ko-12.8b](https://huggingface.co/EleutherAI/polyglot-ko-12.8b) | 0.5954 | 0.6306 | 0.6098 | 0.6118 |
| [Llama-2-Ko-7b 20B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.4518 | 0.4668 | 0.4726 | 0.4828 |
| [Llama-2-Ko-7b 40B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.4562 | 0.4657 | 0.4698 | 0.4774 |   
| [KO-platypus2-7B-EX(ours)](https://huggingface.co/kyujinpy/KO-Platypus2-7B-ex) | 0.4571 | 0.4461 | 0.4371 | 0.4525 |  
| **CoT-llama-2(ours)** | 0.4543 | 0.4554 | 0.4606 | 0.4579 | 
  
### BoolQ (F1)
| Model | 0-shot | 5-shot | 10-shot | 50-shot |
| --- | --- | --- | --- | --- |
| [Polyglot-ko-1.3b](https://huggingface.co/EleutherAI/polyglot-ko-1.3b) | 0.3552 | 0.4751 | 0.4109 | 0.4038 |
| [Polyglot-ko-3.8b](https://huggingface.co/EleutherAI/polyglot-ko-3.8b) | 0.4320 | 0.5263 | 0.4930 | 0.4038 |
| [Polyglot-ko-5.8b](https://huggingface.co/EleutherAI/polyglot-ko-5.8b) | 0.4356 | 0.5698 | 0.5187 | 0.5236 |
| [Polyglot-ko-12.8b](https://huggingface.co/EleutherAI/polyglot-ko-12.8b) | 0.4818 | 0.6041 | 0.6289 | 0.6448 |
| [Llama-2-Ko-7b 20B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.3607 | 0.6797 | 0.6801 | 0.6622 |
| [Llama-2-Ko-7b 40B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.5786 | 0.6977 | 0.7084 | 0.7144 |  
| [KO-platypus2-7B-EX(ours)](https://huggingface.co/kyujinpy/KO-Platypus2-7B-ex) | 0.6028 | 0.6979 | 0.7016 | 0.6988 |  
| **CoT-llama-2(ours)** | 0.5852 | 0.6947 | 0.7059 | 0.7213 | 
  
### SentiNeg (F1)
| Model | 0-shot | 5-shot | 10-shot | 50-shot |
| --- | --- | --- | --- | --- |
| [Polyglot-ko-1.3b](https://huggingface.co/EleutherAI/polyglot-ko-1.3b) | 0.6790 | 0.6257 | 0.5514 | 0.7851 |
| [Polyglot-ko-3.8b](https://huggingface.co/EleutherAI/polyglot-ko-3.8b) | 0.4858 | 0.7950 | 0.7320 | 0.7851 |
| [Polyglot-ko-5.8b](https://huggingface.co/EleutherAI/polyglot-ko-5.8b) | 0.3394 | 0.8841 | 0.8808 | 0.9521 |
| [Polyglot-ko-12.8b](https://huggingface.co/EleutherAI/polyglot-ko-12.8b) | 0.9117 | 0.9015 | 0.9345 | 0.9723 |
| [Llama-2-Ko-7b 20B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.4855 | 0.8295 | 0.8711 | 0.8513 |
| [Llama-2-Ko-7b 40B](https://huggingface.co/beomi/llama-2-ko-7b) | 0.4594 | 0.7611 | 0.7276 | 0.9370 |  
| [KO-platypus2-7B-EX(ours)](https://huggingface.co/kyujinpy/KO-Platypus2-7B-ex) | 0.5821 | 0.7653 | 0.7991 | 0.8643 |  
| **CoT-llama-2(ours)** | 0.5045 | 0.8054 | 0.7942 | 0.9446 | 
   
# References  
[Kaist-COT](https://huggingface.co/datasets/kaist-ai/CoT-Collection)
[CoT-llama](https://huggingface.co/kyujinpy/CoT-llama-2k-7b)  
[KoCoT-2000](https://huggingface.co/datasets/kyujinpy/KoCoT_2000)  
[llama-2-ko](https://huggingface.co/beomi/llama-2-ko-7b)   
[ko-lm-evaluation-harness](https://github.com/Beomi/ko-lm-evaluation-harness)    
  
# TODO
- [x] Make CoT-llama2-7b 
- [x] Share huggingface repo
- [x] Share evaluation results
- [x] Share sample code
  
