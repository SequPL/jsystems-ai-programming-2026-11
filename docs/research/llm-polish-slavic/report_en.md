# How well do LLMs handle Polish and other Slavic languages compared with English?
### Benchmarks, tokenization and cost, 2023–2026: a source-verified research report

*Prepared 24 November 2026. Every number below comes from a primary source: a peer-reviewed paper, an arXiv preprint, an official technical report or a shared-task findings paper. Each number was read by a research agent and then re-checked by an independent verification agent. A figure is marked "derived" when it is arithmetic on source values rather than a number printed in the source.*

---

## Key findings at a glance

1. **On the same questions, frontier models are now roughly at parity between Polish and English.** On the Polish Medical Final Examination (LEK), given in Polish and in an English translation, GPT-4o scored **89.96% in Polish vs 88.69% in English**. In 2023, GPT-4 scored **79.7% in both languages**. [S1, S2]
2. **The gap shows up in smaller and open-weight models.** On the same LEK questions, Llama-3.1-8B scored 51.25% in Polish vs 64.69% in English (−13.4 points, derived), and Qwen2.5-7B scored 51.92% vs 67.73% (−15.8 points). Llama-3.1-8B passed **1 of 13** Polish exam sessions and **13 of 13** English ones. The Polish and English sessions come from different exam editions. [S2]
3. **The gap depends on the task.** Llama-3.1-70B-Instruct on translated benchmarks (EU21) trails English by only 0.04 on MMLU in Polish (0.78 vs 0.82), but by **0.18 on GSM8K math word problems** (0.69 vs 0.87). [S7]
4. **Polish cultural and linguistic competence separates models more than general knowledge does.** On the PLCC benchmark (600 questions on Polish culture, history and language), the top closed models score 81–83% (Gemini-Exp-1206 83.00, Claude-3.5-Sonnet 82.67, GPT-4o 81.33). Original GPT-4 scores 59.50, the best open model (DeepSeek-V3) 69.17, and Polish Bielik-11B 62.17. [S3]
5. **On long contexts, Polish is the best of 26 languages.** In the OneRuler needle-in-a-haystack tasks (64K–128K tokens), Polish averaged **88% across all models**, while English ranked 6th with 83.9%. [S8]
6. **Polish costs more tokens.** On parallel text, current tokenizers use a mean **2.25 tokens per Polish word vs 1.23 per English word**. Western European languages fall between 1.46 and 1.76, and the Slavic languages between 2.25 and 2.66, with Ukrainian the highest. [S10] With the GPT-3.5/GPT-4 tokenizer (cl100k_base), the same Polish text needs **1.91×** as many tokens as English. [S11]
7. **The cost follows the token count.** API prices are per token and do not depend on language. The same content in Polish therefore costs roughly **1.6–1.9×** as much and fills the context window about as much faster (derived from S11 and S12). A Polish-optimised tokenizer (Bielik APT4) roughly **halves** the Polish token count. [S12]
8. **Other Slavic languages show the same pattern.** Russian MMLU trails English by 0.05 to 0.11 points. [S14] Czech trails on reasoning: GPT-3.5 scores 64.2 vs 83.1 on GSM8K. [S17] Continued pretraining for Bulgarian (BgGPT) adds about 6 points in Bulgarian at no cost to English. [S15]
9. **Polish is often absent from vendor multilingual reports.** OpenAI's MMMLU (14 languages), Anthropic's Global-MMLU table (15 languages) and MMLU-ProX (29 languages) do not include Polish, and Llama 3.1 does not list it as a supported language. Evidence on Polish therefore comes mainly from independent and Polish academic work. [S20–S23]

---

## 1. Research question and method

**Question:** How well do large language models handle Polish (and other Slavic languages) compared with English, measured by benchmark quality, tokenization efficiency and resulting cost, in 2023–2026?

**Process:**
- **Six research agents** covered separate areas:
  - Polish benchmarks
  - Tokenization and cost
  - Polish-language sources, searched in Polish (6 rounds)
  - Other Slavic languages, searched in Czech, Slovak, Ukrainian, Russian, Slovene, Croatian, Serbian, Macedonian and Bulgarian (6 rounds)
  - Multilingual and vendor benchmarks
  - Gaps for South Slavic languages and Czech, as a follow-up (5 rounds)
- **Two independent verification agents** re-opened the primary sources and re-read 80 key values. All benchmark values were confirmed. The tokenization check corrected one row label and could not confirm two values, so those two are excluded.
- **Source rules:** numbers were taken only from primary sources. News, blogs and forums were used only as leads. Values that conflicted between versions of a source, or that came from a single unconfirmed reading, were left out (see Section 8).

---

## 2. Same exam, two languages: the cleanest test

The strongest evidence comes from studies that give **the same questions** to a model in Polish and in English.

**Table 1. Polish Medical Final Examination (LEK): accuracy (%) on the same questions in Polish and English** [S2, Table 4]

| Model | Polish | English | Gap (PL − EN, derived) |
|---|---:|---:|---:|
| GPT-4o (2024-08-06) | 89.96 | 88.69 | +1.27 |
| Llama-3.1-70B-Instruct | 80.94 | 82.60 | −1.66 |
| Qwen2.5-72B-Instruct | 76.87 | 81.93 | −5.06 |
| Mistral-Large-Instruct-2407 | 76.75 | 82.40 | −5.65 |
| GPT-4o-mini | 75.88 | 75.92 | −0.04 |
| Bielik-11B-v2.2-Instruct (Polish) | 62.36 | 56.98 | **+5.38** |
| Qwen2.5-7B-Instruct | 51.92 | 67.73 | **−15.81** |
| Llama-3.1-8B-Instruct | 51.25 | 64.69 | **−13.44** |

**2023 baseline** (Scientific Reports, same LEK in both languages): GPT-3.5 averaged **54.8% in Polish vs 60.3% in English**, and GPT-4 averaged **79.7% in both**. [S1]

**What this means:**
- Large frontier models show almost no language penalty on knowledge-heavy exams.
- Small multilingual models lose 13–16 points in Polish.
- Bielik, a model trained for Polish, does better in Polish than in English.

**Polish-only exams, no English version:**
- GPT-4-0125-preview passed **222 of 297 (75%)** Polish specialist board exams (PES). GPT-4-0613 passed 184 (62%), and GPT-3.5-turbo passed none. [S4]
- ChatGPT-4.0 scored 77.55% on the LEK of spring 2021, against a 56% pass threshold. [S5]

---

## 3. Multilingual benchmarks: where Polish stands among languages

**Table 2. Belebele reading comprehension: accuracy (%), English vs Slavic languages** [S6, Table 7]

| Model | EN | PL | CS | RU | UK | PL gap |
|---|---:|---:|---:|---:|---:|---:|
| GPT-3.5-turbo (0-shot) | 87.7 | 74.7 | 76.9 | 78.4 | 72.8 | −13.0 |
| Llama-2-chat 70B (0-shot) | 78.8 | 61.7 | 65.0 | 67.0 | 65.7 | −17.1 |
| Llama 2 70B (5-shot) | 90.9 | 79.2 | 81.1 | 82.7 | 80.8 | −11.7 |

For the 2023 generation of models, the Polish gap was 10–17 points. On the same models it was larger than the gap for Czech or Russian.

**Table 3. EU21 translated benchmark suite, Llama-3.1-70B-Instruct (accuracy 0–1)** [S7, Table 1]

| Task | EN | PL | CS |
|---|---:|---:|---:|
| MMLU | 0.82 | 0.78 | 0.78 |
| ARC | 0.80 | 0.72 | 0.72 |
| HellaSwag | 0.86 | 0.74 | 0.72 |
| GSM8K (math) | 0.87 | 0.69 | 0.75 |
| TruthfulQA | 0.60 | 0.59 | 0.58 |
| **Average** | **0.79** | **0.70** | **0.71** |

**OneRuler** (long context, 26 languages) [S8]:
- Polish is the **top language** on needle-in-a-haystack tasks, at 88% averaged across all models.
- English is 6th, at 83.9%.
- For o3-mini-high at a 128K context, Polish scored 92% and English 67%.
- These results measure **retrieval from long documents**, not overall quality. Polish media often report the 88% without this caveat.

**INCLUDE** (regional knowledge from exams in each country, GPT-4o): Polish 78.8%, Russian 75.0%, Ukrainian 85.6%, Bulgarian 90.7%, Croatian 88.4%, Serbian 91.6%. The questions differ by country, so these scores are not comparable across languages. [S9]

---

## 4. Polish-specific benchmarks

**Table 4. PLCC: Polish Linguistic and Cultural Competency (accuracy %, 600 questions)** [S3, Table 1]

| Model | Score |
|---|---:|
| Gemini-Exp-1206 | 83.00 |
| Claude-3.5-Sonnet | 82.67 |
| GPT-4o | 81.33 |
| DeepSeek-V3 (best open-weight) | 69.17 |
| Bielik-2.3 (11B, Polish) | 62.17 |
| Llama-3.1-405B | 60.00 |
| GPT-4 | 59.50 |

Other Polish benchmarks:
- **LLMzSzŁ** (about 19,000 questions from Polish state exams; open-weight models only): Mistral-Large-Instruct-2407 scored 67.17%. [S13]
- **Open PL LLM Leaderboard** (5-shot, from the Bielik v2 report): Bielik-11B-v2.3-Instruct 65.71, Llama-3.3-70B-Instruct 66.40, Mistral-Large-Instruct-2411 69.84. [S13]

The PLCC paper evaluated no closed model newer than early 2025, so current frontier models are not in this table.

---

## 5. Tokenization: the "Polish token tax"

LLMs read text as **tokens**, which are pieces of words. Tokenizers are mostly trained on English-heavy data, so Polish words, with their rich inflection and diacritics, get split into more pieces.

**Table 5. Mean fertility (tokens per word) on parallel SIB-200 text, average of 6 current models** [S10, Table 4]

| Language | Tokens/word | Relative to EN (derived) |
|---|---:|---:|
| English | 1.23 | 1.00 |
| Spanish | 1.46 | 1.19 |
| French | 1.52 | 1.24 |
| Italian | 1.67 | 1.36 |
| German | 1.76 | 1.43 |
| **Polish** | **2.25** (range 1.88–2.60) | **1.83** |
| Czech | 2.28 | 1.85 |
| Slovenian | 2.28 | 1.85 |
| Croatian | 2.31 | 1.88 |
| Bulgarian | 2.33 | 1.89 |
| Slovak | 2.45 | 1.99 |
| Ukrainian | 2.66 (max 3.62) | 2.16 |

The six models are Llama 4 Maverick, Llama 3.3 70B, Gemma 2 27B, GPT-4o, DeepSeek V3 and Qwen3 32B. *Caveat:* fertility is a per-word measure. The same content usually takes fewer words in Polish than in English, so the per-text premium is somewhat lower than this ratio. The source preprint also contradicts itself on the number of languages (24 vs 25) and models.

**Same text, token count (premium per text):**
- On FLORES-200 parallel sentences, the GPT-3.5/GPT-4 tokenizer (cl100k_base) needs **1.91×** as many tokens for Polish as for English. The same ratio is 2.11× for Czech, 2.49× for Russian and 2.64× for Bulgarian. For the GPT-2 tokenizer, the Polish ratio was 2.69×. [S11, Appendix A]
- Petrov et al. report differences of **up to 15×** between languages overall. [S11]

**Table 6. Polish Constitution preamble vs its official English translation: number of tokens** [S12, Table 1]

| Tokenizer | PL tokens | EN tokens | PL/EN (derived) |
|---|---:|---:|---:|
| Bielik 11B v3, original (Mistral-based, 32k vocab) | 747 | 408 | 1.83 |
| Llama 3.2 / SmolLM3 | 653 | 371 | 1.76 |
| Qwen3 | 625 | 373 | 1.68 |
| Gemma 3 | 510 | 383 | 1.33 |
| EuroLLM | 437 | 404 | 1.08 |
| **APT4 (Polish-optimised)** | **375** | 631 | 0.59 |

This is a single short text, so treat it as an illustration. It shows that newer tokenizers with large vocabularies and European focus (Gemma 3, EuroLLM) cut the Polish premium a lot. A dedicated Polish tokenizer reverses it, at a cost in English. Switching Bielik to APT4 also cost a little quality: Open PL leaderboard 64.11 vs 65.93. [S12]

Ukrainian (Cyrillic) is hit hardest. GPT-4o uses 1.98 tokens per Ukrainian word vs 1.07 per English word. The older GPT-3.5/4 tokenizer used 3.32 vs 1.08. [S16]

---

## 6. What it means for cost and context

API pricing is per token and does not depend on language. So (derived):

- **Input and output costs scale with the token ratio.** A workload that costs $100 in English costs about **$160–190 in Polish** with general-purpose tokenizers (1.6–1.9× premium). It costs about $108 with EuroLLM-class tokenizers and less than English with a Polish-optimised one.
- **The context window is effectively smaller.** A 128K-token window holds about 55% as much Polish text as English text, based on the fertility ratio 1.23/2.25.
- **Latency is longer.** Output tokens are generated one at a time, so a Polish answer of the same length takes about as much longer as its token ratio.
- **Choice of model matters.** Newer tokenizers (Gemma 3, EuroLLM) and Polish models (Bielik v3 with APT4) narrow the gap considerably.

Sources: S10, S11 and S12; the cost multipliers are derived. We did not find a primary study that converts the Polish token premium into PLN or USD, so the figures above are our own arithmetic on the token ratios.

---

## 7. Other Slavic languages

| Language | Evidence | Source |
|---|---|---|
| **Russian** | MMLU (5-shot), EN vs RU: Llama 3.1 (8B) 0.682 vs 0.569; Qwen 2.5 (7B) 0.710 vs 0.632; GigaChat 2 Pro 0.821 vs 0.775. MERA total: GigaChat 2 MAX 0.67, GPT-4o 0.642. | S14 |
| **Czech** | CzechBench, CZ vs EN: GPT-3.5 Turbo MMLU 58.0 vs 64.9, GSM8K 64.2 vs 83.1; Claude 3 Haiku MMLU 67.3 vs 73.0, GSM8K 78.6 vs 89.0; Llama 3 8B ARC-C 64.3 vs 78.5. Educational tutoring task: Mistral 82.0 (EN) → 2.9 (CZ); GPT-4o 94.7 → 43.8. EU21 average with Llama-3.1-70B: 0.71 vs 0.79. | S17, S18, S7 |
| **Bulgarian** | BgGPT (Gemma-2 9B with continued pretraining): Bulgarian average 55.36 → 61.27, English 70.92 → 71.53. 27B version: Bulgarian 60.51 → 64.67, English 75.08 → 75.12. | S15 |
| **Ukrainian** | Expert grammar benchmark (347 questions): best Ukrainian model, Lapa 12B IT, 42.1%. Prompt language changes results in both directions (Gemma 3 4B: 33.6% with a Ukrainian prompt vs 25.6% with English). | S19 |
| **South Slavic** (hr, sr, bs, sl, mk) | Text classification: GPT-5 has the best average rank (EN 1.33, South Slavic 2.55). The sentiment macro-F1 difference between English and South Slavic is 0.02 for GPT-5 and 0.04 for GPT-4o. | S24 |
| **Macedonian** | Seven translated benchmarks (no English pair): Llama 3.3 70B 0.545, domestic-yak 8B 0.535, Mistral Nemo 12B 0.493. | S25 |
| **Slovene** | GaMS3-12B wins more than 60% of comparisons against GPT-4o in a Slovene LLM arena (abstract). A Slovene tokenizer cut corpus tokens from 47.44B to 28.13B. | S26, S27 |

Even after 11 search rounds in national languages, we found **no per-model benchmark scores with an English comparison for Croatian, Serbian or Bosnian specifically**. The only paired data for these languages is the aggregated South Slavic study [S24].

---

## 8. Limitations and excluded data

- **Extraction method.** All values were read through a web-fetching tool that summarises pages. Key values were confirmed by a second, independent read. They were not compared character-by-character with the PDFs.
- **Excluded:**
  - Global-PIQA Slavic scores: v1 and v2 of the paper disagree.
  - Petrov's Ukrainian premium (3.00) and BLOOM Polish value: not confirmed by the verifier.
  - GPT-4's translated-MMLU value for Polish: it appears only in a chart image.
  - The paywalled Rzeczpospolita "Polish AI test" from 2026: no scores published.
  - Unofficial leaderboard mirrors.
- **Conflicts noted in the sources:**
  - The Bielik v3 report gives PLCC for Bielik-v2.6 as 44.88 in a table and 65.50 in the text.
  - The LLMzSzŁ paper gives Bielik-v2.1 as 57.52 in the text and 57.36 in a table.
  - The Tokenizer Tax preprint gives 24 vs 25 languages and 6, 9 or 10 models.
- **Comparability.** Only Tables 1–3 and the CzechBench, EU21 and Russian MMLU rows compare the same test in both languages. Polish-only leaderboards cannot be compared with English scores.
- **Recency.** Few primary sources evaluate 2026 frontier models in Polish. The newest paired PL/EN evidence covers GPT-4o and Llama-3.1-class models.

---

## Sources

- **S1** Rosoł et al., "Evaluation of the performance of GPT-3.5 and GPT-4 on the Polish Medical Final Examination," *Scientific Reports* 13:20512 (2023). https://www.nature.com/articles/s41598-023-46995-z
- **S2** Grzybowski et al., "Polish-English medical knowledge transfer: A new benchmark and results," Findings of EMNLP 2025; arXiv 2412.00559. https://arxiv.org/abs/2412.00559
- **S3** Dadas et al., "Evaluating Polish linguistic and cultural competency in large language models" (PLCC), arXiv 2503.00995 (2025). https://arxiv.org/abs/2503.00995
- **S4** "GPT-4 passes most of the 297 written Polish Board Certification Examinations," arXiv 2405.01589 (2024). https://arxiv.org/abs/2405.01589
- **S5** "Comparison of the Performance of Artificial Intelligence Versus Medical Professionals in the Polish Final Medical Examination," *Cureus* 16(8) (2024), DOI 10.7759/cureus.66011
- **S6** Bandarkar et al., "The Belebele Benchmark," ACL 2024; arXiv 2308.16884. https://arxiv.org/abs/2308.16884
- **S7** Thellmann et al., "Towards Multilingual LLM Evaluation for European Languages," arXiv 2410.08928 (2024). https://arxiv.org/abs/2410.08928
- **S8** Kim et al., "One ruler to measure them all: Benchmarking multilingual long-context language models," arXiv 2503.01996 (2025). https://arxiv.org/abs/2503.01996
- **S9** "INCLUDE: Evaluating Multilingual Language Understanding with Regional Knowledge," arXiv 2411.19799 (ICLR 2025). https://arxiv.org/abs/2411.19799
- **S10** Ovcharov, "The Tokenizer Tax Across 24 European Languages," arXiv 2605.24718 (2026, preprint). https://arxiv.org/abs/2605.24718
- **S11** Petrov et al., "Language Model Tokenizers Introduce Unfairness Between Languages," NeurIPS 2023; arXiv 2305.15425. https://arxiv.org/abs/2305.15425
- **S12** Bielik team, "Advancing Polish Language Modeling Through Tokenizer Optimization in the Bielik v3 7B and 11B Series," arXiv 2604.10799 (2026). https://arxiv.org/abs/2604.10799
- **S13** SpeakLeash, "Bielik 11B v2 Technical Report," arXiv 2505.02410 (2025), and "LLMzSzŁ," arXiv 2501.02266 (2025).
- **S14** GigaChat team, "GigaChat Family," arXiv 2506.09440 (2025). https://arxiv.org/abs/2506.09440
- **S15** INSAIT, "BgGPT 1.0: Extending English-centric LLMs to other languages," arXiv 2412.10893 (2024). https://arxiv.org/abs/2412.10893
- **S16** Maksymenko & Turuta, "Tokenization efficiency of current foundational LLMs for the Ukrainian language," *Frontiers in AI* (2025), DOI 10.3389/frai.2025.1538165
- **S17** Jirkovský, CzechBench (master's thesis, CTU Prague, 2024; not peer-reviewed). dspace.cvut.cz
- **S18** Educational tasks in Czech vs English, arXiv 2504.17720 (2025). https://arxiv.org/abs/2504.17720
- **S19** Galeshchuk et al., "Toward a Gold-Standard Benchmark for Evaluating Ukrainian Language Proficiency in LLMs," UNLP 2026. https://aclanthology.org/2026.unlp-1.12
- **S20** OpenAI MMMLU dataset card. https://huggingface.co/datasets/openai/MMMLU
- **S21** Anthropic, Multilingual support (Claude docs). https://platform.claude.com/docs/en/build-with-claude/multilingual-support
- **S22** MMLU-ProX, arXiv 2503.10497 (2025).
- **S23** Meta Llama 3.1 model card (supported languages).
- **S24** Kuzman Pungeršek et al., "State of the Art in Text Classification for South Slavic Languages," arXiv 2511.07989 (2025/2026). https://arxiv.org/abs/2511.07989
- **S25** Macedonian LLM evaluation (domestic-yak), arXiv 2506.09560 (2025). https://arxiv.org/abs/2506.09560
- **S26** Vreš et al., GaMS3-12B, arXiv 2603.01691 (2026).
- **S27** Vreš et al., "Generative Model for Less-Resourced Language with 1 Billion Parameters," arXiv 2410.06898 (2024).
