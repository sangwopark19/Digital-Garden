---
title: "Meta-Harness: End-to-End Optimization of Model Harnesses"
source: "https://yoonholee.com/meta-harness/"
author:
  - "[[Yoonho Lee]]"
published:
created: 2026-04-01
description: "Meta-Harness automatically optimizes model harnesses — the code determining what to store, retrieve, and present to an LLM — surpassing hand-designed systems on text classification, math reasoning, and agentic coding."
tags:
  - "clippings"
---
데스크톱 브라우저에서 보는 것이 가장 좋습니다. 마우스 커서를 올리면 상호작용 요소가 나타나며, 여백에 표시되는 숫자는 넓은 화면에 적합합니다.

## TerminalBench-2: 진화의 활용

이 실행은 최종적으로 보고된 메타 하네스를 생성하는 데 사용된 실행이 아닙니다. 이는 메타 하네스의 내부 작동 방식을 이해하는 데 특히 유용한 초기 소규모 탐색 실행입니다. 대부분의 에이전트가 어려움을 겪는 **19개 작업** 으로 구성된 난이도 높은 하위 집합(낮은 기준 점수 참조)을 의도적으로 선택하여 순수한 하네스 변경으로 인한 개선 사항이 명확하게 드러나도록 했습니다. Terminus-KIRA(28.5%)에서 시작하여 7번째 반복에서 **46.5%** 에 도달합니다.

제안자의 추론 과정을 단계별로 살펴보세요. 실행 추적 데이터를 기반으로 반사실적 진단을 수행하고, 파일 시스템을 통해 원시 로그를 읽어 특정 오류 모드를 식별하며, 목표에 맞는 수정 사항을 제안합니다. 각 제안은 이전 실행에서 얻은 구체적인 증거를 바탕으로 합니다. 89개 작업 전체에 대한 벤치마크 결과는 아래 [결과](https://yoonholee.com/meta-harness/#results) 섹션에서 확인할 수 있습니다. 점을 클릭하거나 화살표 키를 사용하여 코드 변경 사항을 살펴보세요.

부분집합 기준선

메타 하네스 46.5%

여자 이름 35.8%

터미너스-키라 28.5%

레타 코드 28.4%

클로드 코드 28.0%

팩토리 드로이드 27.4%

터미널 2 26.0%

거위 23.2%

기준선

출발점은 KRAFTON AI의 [Terminus-KIRA입니다](https://github.com/krafton-ai/KIRA/blob/main/terminus_kira/terminus_kira.py). 이 에이전트는 기본 도구 호출, 멀티미디어용 이미지 읽기 도구, 마커 기반 명령 폴링, 그리고 다양한 관점의 완료 체크리스트를 갖춘 잘 설계된 도구입니다. 대부분의 에이전트가 어려움을 겪는 까다로운 19개 작업 하위 집합에서 28.5%의 점수를 기록했습니다.

터미널\_키라 (28.5%)

![메타 하네스 방법 개요.](https://yoonholee.com/meta-harness/static/images/method.svg)

**메타 하네스 검색 루프.** (1) 에이전트는 이전 후보들의 소스 코드, 실행 추적 및 점수가 포함된 파일 시스템을 읽고 새로운 하네스를 제안합니다. (2) 제안된 하네스를 홀드아웃 작업에서 평가합니다. (3) 모든 로그는 파일 시스템에 저장되고 루프가 반복됩니다.

## 이것이 다른 점은 무엇일까요?

LLM 피드백을 활용하여 텍스트와 코드를 최적화하는 방법은 여러 가지가 있습니다. 핵심적인 차이점은 최적화 도구가 얼마나 많은 정보를 처리하는가에 있습니다. 기존의 대부분의 방법은 모든 정보를 간략한 요약, 스칼라 점수 또는 최근 후보들을 보여주는 슬라이딩 윈도우 형태로 압축합니다. 이는 작은 문제에는 효과적이지만, 복잡한 엔지니어링 작업에서는 원시 실행 추적 데이터를 확인하지 않고는 오류를 진단하기 어렵습니다.

Meta-Harness takes a different approach: it gives the proposer a filesystem containing the full source code, scores, and execution traces of every prior candidate. The proposer is a coding agent (Claude Code) that reads what it needs via `grep`, `cat`, and other standard tools. In practice, this means up to 10M tokens of diagnostic context per step, vs. at most 26K for all prior methods we surveyed. The result is that the proposer can trace a failure back to the specific harness decision that caused it, rather than guessing from a score. See [the paper](https://arxiv.org/abs/2603.28052) for details.

| Method | History | Log content | Mtok/iter ↑ |
| --- | --- | --- | --- |
| Self-Refine | Last | output + self-generated critique | 0.001 |
| OPRO | Window | past (solution, score) pairs | 0.002 |
| TextGrad | Last | LLM textual gradient | 0.015 |
| MIPRO | Summary | bootstrapped program traces | 0.003 |
| AlphaEvolve | Window | program database + eval. scores | 0.022 |
| GEPA | Summary | rollout traces (reasoning + tools) | 0.008 |
| Feedback Descent | Summary | pairwise comparison + feedback | 0.012 |
| TTT-Discover | Window | prev. solution fragment | 0.026 |
| **Meta-Harness** | **Full** | **all logs and scores** | **10.0** |

Context available per optimization step. Mtok/iter = estimated context per evaluation in each paper's largest setting. Hover a row for details.

## Results

### Text Classification

We follow the online text classification setup of ACE: an LLM receives labeled examples one at a time, updates its memory, and is evaluated on a held-out test set. We search over harnesses using three datasets — LawBench (215 classes), Symptom2Disease (22 classes), and USPTO-50k (180 classes) — with GPT-OSS-120B as the model. We ran 20 evolution iterations with two candidates per iteration, producing 40 candidate harnesses. All test sets are held out until the final evaluation.

The best discovered harness (*Label-Primed Query*) achieves **48.6%** vs. ACE’s 40.9% — a **7.7-point improvement** using **4× fewer context tokens**. Gains concentrate on tasks with large, confusable label spaces: LawBench (215 classes) sees +16 points, Symptom2Disease +9 points. None of the discovered harnesses require additional LLM calls beyond the main task-solving call.

| Harness | USPTO ↑ | S2D ↑ | Law ↑ | Acc ↑ | Ctx (K) ↓ |
| --- | --- | --- | --- | --- | --- |
| Zero-shot | 12.0 | 63.2 | 7.0 | 27.4 | 0 |
| Few-shot (N=4) | 11.0 | 68.4 | 18.0 | 32.5 | 4.0 |
| Few-shot (N=8) | 14.0 | 67.9 | 21.0 | 34.3 | 8.0 |
| Few-shot (N=16) | 15.0 | 67.0 | 20.0 | 34.0 | 15.4 |
| Few-shot (N=32) | 13.0 | 72.2 | 21.0 | 35.4 | 31.5 |
| Few-shot (all) | 15.0 | 78.3 | 29.0 | 40.8 | 49.3 |
| ACE | **16.0** | 77.8 | 29.0 | 40.9 | 203.0 |
| MCE | 14.0 | 83.0 | 23.0 | 40.0 | 114.0 |
| Meta-Harness (Ours) | 14.0 | **86.8** | **45.0** | **48.6** | 45.5 |

Test accuracy on three text classification benchmarks (GPT-OSS-120B). Ctx = average additional input context (thousands of characters).

We also directly compare Meta-Harness against two representative program-search methods, OpenEvolve and TTT-Discover (with PUCT selection), giving each the same proposer and evaluation budget. Meta-Harness matches their final accuracy with **10× fewer evaluations**, and its final accuracy surpasses theirs by more than 10 points. We attribute this to the filesystem-based interface: both OpenEvolve and PUCT compress history into a fixed prompt format, discarding the execution traces that Meta-Harness uses for targeted diagnosis.

![메타 하네스 학습 곡선과 기준선 비교](https://yoonholee.com/meta-harness/static/images/learning_curves.svg)

**Search progress over ~60 proposals.** Meta-Harness (red) outperforms all baselines and reaches the next-best optimizer’s final accuracy in just 4 iterations. TTT-Discover, Best-of-N, OpenEvolve, ACE, few-shot, and zero-shot shown for comparison.

### Math Reasoning

We study retrieval-augmented math solving: a language model is given retrieved examples from a large corpus before attempting each problem. Meta-Harness searches over retrieval programs that can implement arbitrary filtering, branching, and formatting logic using corpus metadata and BM25 scores. The corpus contains ≥500K problems drawn from eight open-source datasets. We evolve a single retrieval harness on a 250-problem search set, then evaluate it on 200 held-out IMO-level problems. The same harness is then tested on five models unseen during search, directly measuring transfer.

A single discovered retrieval harness improves accuracy by **+4.7 points** on average (34.1% → 38.8%) across five held-out models. It matches or exceeds the strongest fixed baselines on average, outperforming BM25 retrieval by 1.3 points overall.

| Method | GPT-5.4n ↑ | GPT-5.4m ↑ | Gem-3.1FL ↑ | Gem-3F ↑ | GPT-20B ↑ | Avg ↑ |
| --- | --- | --- | --- | --- | --- | --- |
| No Retriever | 23.0 | 28.8 | 28.6 | 42.6 | 47.6 | 34.1 |
| Random Few-shot | 23.1 (+0.1) | 24.5 (-4.3) | 31.0 (+2.4) | 40.4 (-2.2) | 41.8 (-5.8) | 32.2 (-1.9) |
| BM25 Retrieval | 30.2 (+7.2) | 29.2 (+0.4) | 32.8 (+4.2) | 46.6 (+4.0) | 48.9 (+1.3) | 37.5 (+3.4) |
| Meta-Harness (Ours) | 31.7 (+8.7) | 30.4 (+1.6) | 34.9 (+6.3) | 46.3 (+3.7) | 50.6 (+3.0) | **38.8** (+4.7) |

Retrieval-augmented math reasoning on 200 IMO-level problems (pass@1, avg. over 3 samples). Absolute improvement over no retriever in parentheses. The discovered Meta-Harness retrieval strategy improves reasoning across all five held-out models, with a 4.7-point average gain.

### Agentic Coding (TerminalBench-2)

TerminalBench-2는 코드 번역, 분산 머신러닝 설정, 시스템 프로그래밍, 생물정보학, 암호해독 등 89개의 도커화된 작업에서 LLM 에이전트의 성능을 평가합니다. 각 작업은 이진 합격/불합격으로 평가되며, 각 작업당 5회의 독립적인 실행이 수행됩니다. 이러한 작업은 복잡한 종속성, 제한된 터미널 출력, 그리고 상당한 도메인 지식 하에서 장기간에 걸친 완전 자율 실행을 요구하기 때문에 난이도가 높습니다.

Meta-Harness는 전체 코딩 하네스(시스템 프롬프트, 도구 정의, 완료 확인 로직 및 컨텍스트 관리)를 발전시킵니다. 제안자는 작업별 실행 추적(명령 로그, 오류 메시지, 시간 초과 동작)을 읽어 실패 모드를 진단하고 맞춤형 수정 사항을 제안합니다. 우리는 Terminus 2와 Terminus-KIRA라는 두 개의 강력한 공개 기준선을 기반으로 검색을 시작합니다.

**클로드 오푸스 4.6** 에서 Meta-Harness는 **76.4%의** 통과율을 달성하여 Terminus-KIRA(74.7%)를 능가하고 모든 오푸스 4.6 에이전트 중 **2위를 기록했습니다.** **클로드 하이쿠 4.5** 에서는 Meta-Harness가 **37.6%의** 통과율을 달성하여 다음으로 우수한 에이전트(Goose, 35.5%)를 앞섰고 모든 하이쿠 4.5 에이전트 중 **1위를 차지했습니다.**

| 클로드 작품 4.6 | 통과하다 % |
| --- | --- |
| 클로드 코드 | 58.0 |
| 터미널 2 | 62.9 |
| Mux | 66.5 |
| 팩토리 드로이드 | 69.9 |
| 통에이전트 | 71.9 |
| 마야-V2 | 72.1 |
| 터미너스-키라 | 74.7 |
| 카피 | 75.3 |
| 메타 하네스(당사 제품) | 76.4 |
| 포지코드 | **81.8** |

| 클로드 하이쿠 4.5 | 통과하다 % |
| --- | --- |
| 오픈핸즈 | 13.9 |
| 클로드 코드 | 27.5 |
| 터미널 2 | 28.3 |
| 미니 SWE 에이전트 | 29.8 |
| 터미너스-키라 | 33.7 |
| 거위 | 35.5 |
| 메타 하네스(당사 제품) | **37.6** |

TerminalBench-2의 통과율입니다. 다른 결과는 공식 순위표에서 가져왔습니다. **Meta-Harness는 모든 Opus 4.6 에이전트 중 2위, 모든 Haiku 4.5 에이전트 중 1위를 차지했습니다.**

---

## BibTeX

```
@inproceedings{lee2026metaharness,
  title={Meta-Harness: End-to-End Optimization of Model Harnesses},
  author={Lee, Yoonho and Nair, Roshen and Zhang, Qizheng and Lee, Kangwook and Khattab, Omar and Finn, Chelsea},
  booktitle={Preprint},
  year={2026}
}
```