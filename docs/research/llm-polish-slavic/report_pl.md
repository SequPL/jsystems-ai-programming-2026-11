# Jak dobrze modele LLM radzą sobie z językiem polskim i innymi językami słowiańskimi w porównaniu z angielskim?
### Benchmarki, tokenizacja i koszt w latach 2023–2026: raport oparty na zweryfikowanych źródłach

*Opracowano 24 listopada 2026 r. Każda liczba pochodzi ze źródła pierwotnego: recenzowanego artykułu, preprintu z arXiv, oficjalnego raportu technicznego lub raportu z zadań współzawodnictwa (shared task). Każdą odczytał agent badawczy, a następnie niezależnie sprawdził agent weryfikujący. Oznaczenie „wyliczone” oznacza, że wartość obliczyliśmy na podstawie liczb ze źródła i nie jest w nim wydrukowana.*

---

## Najważniejsze wnioski

1. **Na tych samych pytaniach czołowe modele mają dziś niemal taki sam wynik po polsku i po angielsku.** Na Lekarskim Egzaminie Końcowym (LEK) w wersji polskiej i w angielskim tłumaczeniu GPT-4o uzyskał **89,96% po polsku wobec 88,69% po angielsku**. W 2023 r. GPT-4 miał **79,7% w obu językach**. [S1, S2]
2. **Różnica pojawia się w mniejszych modelach i modelach otwartych.** Na tych samych pytaniach LEK:
   - Llama-3.1-8B: 51,25% po polsku wobec 64,69% po angielsku (−13,4 pkt, wyliczone).
   - Qwen2.5-7B: 51,92% wobec 67,73% (−15,8 pkt).
   - Llama-3.1-8B zdała **1 z 13** sesji egzaminu po polsku i **13 z 13** po angielsku. Zastrzeżenie: zestawy polskie i angielskie pochodzą z różnych edycji egzaminu. [S2]
3. **Różnica zależy od zadania.** Llama-3.1-70B-Instruct w przetłumaczonych benchmarkach (EU21) traci po polsku tylko 0,04 na MMLU (0,78 wobec 0,82), ale **0,18 na zadaniach matematycznych GSM8K** (0,69 wobec 0,87). [S7]
4. **Kompetencje kulturowe i językowe różnicują modele silniej niż wiedza ogólna.** W benchmarku PLCC (600 pytań o polską kulturę, historię i język):
   - najlepsze modele zamknięte mają 81–83% (Gemini-Exp-1206 83,00; Claude-3.5-Sonnet 82,67; GPT-4o 81,33),
   - pierwotny GPT-4 ma 59,50,
   - najlepszy model otwarty (DeepSeek-V3) ma 69,17,
   - polski Bielik-11B ma 62,17. [S3]
5. **W długich kontekstach polski wypada najlepiej z 26 języków.** W zadaniach typu „igła w stogu siana” (OneRuler, konteksty 64K–128K tokenów) polski uzyskał **średnio 88% dla wszystkich modeli**. Angielski był szósty (83,9%). [S8]
6. **Polski zużywa więcej tokenów.** Obecne tokenizery dzielą tekst równoległy na średnio **2,25 tokena na słowo po polsku wobec 1,23 po angielsku**. Języki zachodnioeuropejskie mieszczą się w przedziale 1,46–1,76, języki słowiańskie w przedziale 2,25–2,66, a najgorzej wypada ukraiński. [S10] W tokenizerze GPT-3.5/GPT-4 (cl100k_base) ten sam tekst po polsku wymaga **1,91×** więcej tokenów niż po angielsku. [S11]
7. **Za liczbą tokenów idzie koszt.** Ceny API są liczone za token, bez względu na język. Ta sama treść po polsku kosztuje więc około **1,6–1,9×** więcej i proporcjonalnie szybciej zapełnia okno kontekstu (wyliczone na podstawie S11 i S12). Tokenizer zoptymalizowany pod polski (Bielik APT4) mniej więcej **o połowę** zmniejsza liczbę tokenów w polskim tekście. [S12]
8. **W innych językach słowiańskich wzorzec jest podobny.**
   - Rosyjski: na MMLU traci względem angielskiego od 0,05 do 0,11 pkt. [S14]
   - Czeski: traci na zadaniach z rozumowania; GPT-3.5 ma na GSM8K 64,2 wobec 83,1 po angielsku. [S17]
   - Bułgarski: dalsze trenowanie modelu na danych bułgarskich (BgGPT) dodało ok. 6 pkt po bułgarsku bez straty po angielsku. [S15]
9. **W wielojęzycznych raportach producentów często brakuje polskiego.** Nie ma go w MMMLU OpenAI (14 języków), w tabeli Global-MMLU Anthropic (15 języków) ani w MMLU-ProX (29 języków). Nie ma go też na liście oficjalnie wspieranych języków Llama 3.1. Dane o polskim pochodzą więc głównie z niezależnych i polskich prac naukowych. [S20–S23]

---

## 1. Pytanie badawcze i metoda

**Pytanie:** jak duże modele językowe radzą sobie z językiem polskim (i innymi słowiańskimi) w porównaniu z angielskim? Porównujemy jakość w benchmarkach, efektywność tokenizacji i wynikający z niej koszt w latach 2023–2026.

**Przebieg:**
- **Sześciu agentów badawczych** pracowało w osobnych obszarach:
  1. polskie benchmarki,
  2. tokenizacja i koszt,
  3. źródła polskojęzyczne, wyszukiwane po polsku (6 rund),
  4. pozostałe języki słowiańskie, wyszukiwane po czesku, słowacku, ukraińsku, rosyjsku, słoweńsku, chorwacku, serbsku, macedońsku i bułgarsku (6 rund),
  5. benchmarki wielojęzyczne i dane producentów,
  6. uzupełnianie luk dla języków południowosłowiańskich i czeskiego (5 rund).
- **Dwóch niezależnych agentów weryfikujących** ponownie otworzyło źródła pierwotne i sprawdziło 80 kluczowych wartości. Wszystkie wartości z benchmarków potwierdzono. W tokenizacji poprawiono jedną etykietę wiersza, a dwóch wartości nie udało się potwierdzić, więc je wykluczyliśmy.
- **Zasady dotyczące źródeł:** liczby pochodzą wyłącznie ze źródeł pierwotnych. Media, blogi i fora służyły tylko jako tropy. Pominęliśmy wartości sprzeczne między wersjami źródła albo odczytane tylko raz i niepotwierdzone (zob. sekcja 8).

---

## 2. Ten sam egzamin w dwóch językach: najczystszy test

Najmocniejsze dowody dają badania, w których model dostaje **te same pytania** po polsku i po angielsku.

**Tabela 1. Lekarski Egzamin Końcowy (LEK): trafność (%) na tych samych pytaniach po polsku i po angielsku** [S2, tab. 4]

| Model | Polski | Angielski | Różnica (PL − EN, wyliczona) |
|---|---:|---:|---:|
| GPT-4o (2024-08-06) | 89,96 | 88,69 | +1,27 |
| Llama-3.1-70B-Instruct | 80,94 | 82,60 | −1,66 |
| Qwen2.5-72B-Instruct | 76,87 | 81,93 | −5,06 |
| Mistral-Large-Instruct-2407 | 76,75 | 82,40 | −5,65 |
| GPT-4o-mini | 75,88 | 75,92 | −0,04 |
| Bielik-11B-v2.2-Instruct (polski) | 62,36 | 56,98 | **+5,38** |
| Qwen2.5-7B-Instruct | 51,92 | 67,73 | **−15,81** |
| Llama-3.1-8B-Instruct | 51,25 | 64,69 | **−13,44** |

**Punkt odniesienia z 2023 r.** (Scientific Reports, ten sam LEK w obu językach): GPT-3.5 miał średnio **54,8% po polsku wobec 60,3% po angielsku**, a GPT-4 **79,7% w obu językach**. [S1]

**Co z tego wynika:**
- Duże czołowe modele praktycznie nie tracą na zmianie języka w egzaminach sprawdzających wiedzę.
- Małe modele wielojęzyczne tracą po polsku 13–16 pkt.
- Bielik, trenowany z myślą o polskim, radzi sobie lepiej po polsku niż po angielsku.

**Egzaminy tylko po polsku (bez wersji angielskiej):**
- GPT-4-0125-preview zdał **222 z 297 (75%)** egzaminów specjalizacyjnych PES. GPT-4-0613 zdał 184 (62%), a GPT-3.5-turbo żadnego. [S4]
- ChatGPT-4.0 uzyskał 77,55% na LEK z wiosny 2021 r. przy progu zdania 56%. [S5]

---

## 3. Benchmarki wielojęzyczne: miejsce polskiego wśród języków

**Tabela 2. Belebele, rozumienie tekstu: trafność (%), angielski wobec języków słowiańskich** [S6, tab. 7]

| Model | EN | PL | CS | RU | UK | Różnica PL |
|---|---:|---:|---:|---:|---:|---:|
| GPT-3.5-turbo (0-shot) | 87,7 | 74,7 | 76,9 | 78,4 | 72,8 | −13,0 |
| Llama-2-chat 70B (0-shot) | 78,8 | 61,7 | 65,0 | 67,0 | 65,7 | −17,1 |
| Llama 2 70B (5-shot) | 90,9 | 79,2 | 81,1 | 82,7 | 80,8 | −11,7 |

W modelach z 2023 r. różnica dla polskiego wynosiła 10–17 pkt i była większa niż dla czeskiego czy rosyjskiego w tych samych modelach.

**Tabela 3. Pakiet przetłumaczonych benchmarków EU21, Llama-3.1-70B-Instruct (trafność 0–1)** [S7, tab. 1]

| Zadanie | EN | PL | CS |
|---|---:|---:|---:|
| MMLU | 0,82 | 0,78 | 0,78 |
| ARC | 0,80 | 0,72 | 0,72 |
| HellaSwag | 0,86 | 0,74 | 0,72 |
| GSM8K (matematyka) | 0,87 | 0,69 | 0,75 |
| TruthfulQA | 0,60 | 0,59 | 0,58 |
| **Średnia** | **0,79** | **0,70** | **0,71** |

**OneRuler** (długi kontekst, 26 języków) [S8]:
- Polski jest **najlepszym językiem** w zadaniach „igła w stogu siana”: średnio 88% dla wszystkich modeli. Angielski jest szósty (83,9%).
- Dla o3-mini-high przy kontekście 128K: polski 92%, angielski 67%.
- Uwaga: to test **wyszukiwania informacji w długim dokumencie**, a nie ogólnej jakości. W polskich mediach wynik 88% często podaje się bez tego zastrzeżenia.

**INCLUDE** (wiedza regionalna z egzaminów krajowych, GPT-4o):

| Język | Wynik |
|---|---:|
| polski | 78,8% |
| rosyjski | 75,0% |
| ukraiński | 85,6% |
| bułgarski | 90,7% |
| chorwacki | 88,4% |
| serbski | 91,6% |

Pytania są inne w każdym kraju, więc tych wyników nie da się porównywać między językami. [S9]

---

## 4. Polskie benchmarki

**Tabela 4. PLCC: polskie kompetencje językowe i kulturowe (trafność %, 600 pytań)** [S3, tab. 1]

| Model | Wynik |
|---|---:|
| Gemini-Exp-1206 | 83,00 |
| Claude-3.5-Sonnet | 82,67 |
| GPT-4o | 81,33 |
| DeepSeek-V3 (najlepszy otwarty) | 69,17 |
| Bielik-2.3 (11B, polski) | 62,17 |
| Llama-3.1-405B | 60,00 |
| GPT-4 | 59,50 |

**Inne polskie benchmarki:**
- **LLMzSzŁ** (ok. 19 tys. pytań z polskich egzaminów państwowych, wyłącznie modele otwarte): Mistral-Large-Instruct-2407 uzyskał 67,17%. [S13]
- **Open PL LLM Leaderboard** (5-shot, dane z raportu Bielik v2):

| Model | Wynik |
|---|---:|
| Mistral-Large-Instruct-2411 | 69,84 |
| Llama-3.3-70B-Instruct | 66,40 |
| Bielik-11B-v2.3-Instruct | 65,71 |

Badanie PLCC nie obejmowało modeli zamkniętych nowszych niż z początku 2025 r., więc w tabeli brakuje obecnych czołowych modeli.

---

## 5. Tokenizacja, czyli „podatek tokenowy” od polszczyzny

Modele językowe czytają tekst w **tokenach**, czyli fragmentach słów. Tokenizery trenuje się głównie na danych z przewagą angielskiego. Polskie słowa, z bogatą fleksją i znakami diakrytycznymi, są więc dzielone na więcej fragmentów.

**Tabela 5. Średnia „płodność” tokenizera (tokeny na słowo) na tekście równoległym SIB-200, średnia z 6 aktualnych modeli** [S10, tab. 4]

| Język | Tokeny/słowo | Względem EN (wyliczone) |
|---|---:|---:|
| angielski | 1,23 | 1,00 |
| hiszpański | 1,46 | 1,19 |
| francuski | 1,52 | 1,24 |
| włoski | 1,67 | 1,36 |
| niemiecki | 1,76 | 1,43 |
| **polski** | **2,25** (zakres 1,88–2,60) | **1,83** |
| czeski | 2,28 | 1,85 |
| słoweński | 2,28 | 1,85 |
| chorwacki | 2,31 | 1,88 |
| bułgarski | 2,33 | 1,89 |
| słowacki | 2,45 | 1,99 |
| ukraiński | 2,66 (maks. 3,62) | 2,16 |

Uśredniono sześć modeli: Llama 4 Maverick, Llama 3.3 70B, Gemma 2 27B, GPT-4o, DeepSeek V3 i Qwen3 32B.

*Zastrzeżenia:*
- Płodność to miara w przeliczeniu na słowo. Ta sama treść po polsku zwykle mieści się w mniejszej liczbie słów niż po angielsku, więc narzut liczony na cały tekst jest nieco niższy niż ten współczynnik.
- Preprint, z którego pochodzą dane, podaje sprzeczne liczby języków (24 i 25) i modeli.

**Ten sam tekst, liczba tokenów (narzut na tekst):**
- Na zdaniach równoległych FLORES-200 tokenizer GPT-3.5/GPT-4 (cl100k_base) potrzebuje dla polskiego **1,91×** więcej tokenów niż dla angielskiego. Dla porównania: czeski 2,11×, rosyjski 2,49×, bułgarski 2,64×. Dla tokenizera GPT-2 polski wymagał 2,69×. [S11, dodatek A]
- Petrov i in. raportują między językami różnice **do 15×**. [S11]

**Tabela 6. Preambuła Konstytucji RP i jej oficjalne tłumaczenie angielskie: liczba tokenów** [S12, tab. 1]

| Tokenizer | Tokeny PL | Tokeny EN | PL/EN (wyliczone) |
|---|---:|---:|---:|
| Bielik 11B v3, oryginalny (bazujący na Mistralu, słownik 32k) | 747 | 408 | 1,83 |
| Llama 3.2 / SmolLM3 | 653 | 371 | 1,76 |
| Qwen3 | 625 | 373 | 1,68 |
| Gemma 3 | 510 | 383 | 1,33 |
| EuroLLM | 437 | 404 | 1,08 |
| **APT4 (zoptymalizowany pod polski)** | **375** | 631 | 0,59 |

To jeden krótki tekst, więc traktujmy go jako ilustrację. Pokazuje jednak trzy rzeczy:
- Nowsze tokenizery z dużym słownikiem i nastawieniem na języki europejskie (Gemma 3, EuroLLM) mocno zmniejszają narzut dla polskiego.
- Dedykowany polski tokenizer odwraca proporcję, ale kosztem angielskiego.
- Przejście Bielika na APT4 lekko obniżyło jakość: 64,11 wobec 65,93 w Open PL leaderboard. [S12]

Najmocniej obrywa ukraiński (cyrylica). GPT-4o potrzebuje 1,98 tokena na słowo ukraińskie wobec 1,07 na angielskie. Starszy tokenizer GPT-3.5/4 potrzebował 3,32 wobec 1,08. [S16]

---

## 6. Konsekwencje dla kosztu i kontekstu

Ceny API są liczone za token, niezależnie od języka. Wynika z tego (wyliczenia):

- **Koszty wejścia i wyjścia rosną proporcjonalnie do narzutu tokenów.** Zadanie, które po angielsku kosztuje 100 $, po polsku kosztuje:
  - **ok. 160–190 $** przy tokenizerach ogólnego przeznaczenia (narzut 1,6–1,9×),
  - ok. 108 $ przy tokenizerach klasy EuroLLM,
  - mniej niż po angielsku przy tokenizerze zoptymalizowanym pod polski.
- **Okno kontekstu jest w praktyce mniejsze.** W oknie 128K tokenów mieści się ok. 55% tego, co po angielsku (wg współczynnika płodności 1,23/2,25).
- **Odpowiedzi powstają dłużej.** Tokeny wyjściowe generowane są po kolei, więc odpowiedź tej samej długości trwa dłużej mniej więcej o współczynnik narzutu.
- **Wybór modelu ma znaczenie.** Nowsze tokenizery (Gemma 3, EuroLLM) i polskie modele (Bielik v3 z APT4) wyraźnie zmniejszają różnicę.

Źródła: S10, S11 i S12; mnożniki kosztu są wyliczone. Nie znaleźliśmy pierwotnego badania, które przeliczałoby polski narzut tokenowy na złotówki lub dolary, więc powyższe kwoty to nasze własne wyliczenia na podstawie współczynników tokenów.

---

## 7. Pozostałe języki słowiańskie

**Rosyjski** [S14]
- MMLU (5-shot), angielski wobec rosyjskiego:
  - Llama 3.1 (8B): 0,682 wobec 0,569,
  - Qwen 2.5 (7B): 0,710 wobec 0,632,
  - GigaChat 2 Pro: 0,821 wobec 0,775.
- MERA (wynik łączny): GigaChat 2 MAX 0,67, GPT-4o 0,642.

**Czeski** [S17, S18, S7]
- CzechBench, czeski wobec angielskiego:
  - GPT-3.5 Turbo: MMLU 58,0 wobec 64,9; GSM8K 64,2 wobec 83,1,
  - Claude 3 Haiku: MMLU 67,3 wobec 73,0; GSM8K 78,6 wobec 89,0,
  - Llama 3 8B: ARC-C 64,3 wobec 78,5.
- Zadanie edukacyjne (tutoring), angielski → czeski: Mistral 82,0 → 2,9; GPT-4o 94,7 → 43,8.
- Średnia EU21 dla Llama-3.1-70B: 0,71 wobec 0,79.

**Bułgarski** [S15]
- BgGPT (Gemma-2 9B, dalej trenowana na danych bułgarskich): średnia bułgarska 55,36 → 61,27, angielska 70,92 → 71,53.
- Wersja 27B: bułgarska 60,51 → 64,67, angielska 75,08 → 75,12.

**Ukraiński** [S19]
- Ekspercki benchmark gramatyczny (347 pytań): najlepszy model ukraiński, Lapa 12B IT, uzyskał 42,1%.
- Język polecenia zmienia wyniki w obie strony: Gemma 3 4B ma 33,6% z poleceniem po ukraińsku i 25,6% z poleceniem po angielsku.

**Języki południowosłowiańskie (hr, sr, bs, sl, mk)** [S24]
- Klasyfikacja tekstów: najlepszą średnią pozycję w rankingu ma GPT-5 (angielski 1,33, języki południowosłowiańskie 2,55).
- Różnica macro-F1 dla analizy sentymentu między angielskim a językami południowosłowiańskimi: GPT-5 0,02, GPT-4o 0,04.

**Macedoński** [S25]
- Siedem przetłumaczonych benchmarków (bez pary z angielskim): Llama 3.3 70B 0,545; domestic-yak 8B 0,535; Mistral Nemo 12B 0,493.

**Słoweński** [S26, S27]
- GaMS3-12B wygrywa ponad 60% porównań z GPT-4o w słoweńskiej arenie LLM (dane z abstraktu).
- Słoweński tokenizer zmniejszył liczbę tokenów w korpusie z 47,44 mld do 28,13 mld.

Mimo 11 rund wyszukiwania w językach narodowych **nie znaleźliśmy wyników per model z porównaniem do angielskiego osobno dla chorwackiego, serbskiego ani bośniackiego**. Jedyne sparowane dane dla tych języków pochodzą ze zbiorczego badania południowosłowiańskiego [S24].

---

## 8. Ograniczenia i dane wykluczone

- **Metoda odczytu.** Wszystkie wartości odczytano narzędziem, które pobiera strony i streszcza ich treść. Kluczowe liczby potwierdził drugi, niezależny odczyt. Nie porównywano ich znak po znaku z plikami PDF.
- **Wykluczone:**
  - wyniki słowiańskie z Global-PIQA (wersje v1 i v2 artykułu są ze sobą sprzeczne),
  - narzut Petrova dla ukraińskiego (3,00) i wartość BLOOM dla polskiego (weryfikator ich nie potwierdził),
  - wynik GPT-4 dla polskiego w przetłumaczonym MMLU (jest tylko na obrazku wykresu),
  - płatny „polski test AI” Rzeczpospolitej z 2026 r. (bez opublikowanych wyników),
  - nieoficjalne kopie rankingów.
- **Sprzeczności w samych źródłach:**
  - raport Bielik v3 podaje PLCC dla Bielika v2.6 jako 44,88 w tabeli i 65,50 w tekście,
  - artykuł LLMzSzŁ podaje wynik Bielika v2.1 jako 57,52 w tekście i 57,36 w tabeli,
  - preprint Tokenizer Tax podaje 24 lub 25 języków oraz 6, 9 lub 10 modeli.
- **Porównywalność.** Ten sam test w obu językach porównują tylko tabele 1–3 oraz wiersze CzechBench, EU21 i rosyjskiego MMLU. Wyników z rankingów wyłącznie polskich nie można zestawiać z wynikami angielskimi.
- **Aktualność.** Niewiele źródeł pierwotnych ocenia po polsku czołowe modele z 2026 r. Najnowsze dowody z parą PL/EN dotyczą GPT-4o i modeli klasy Llama-3.1.

---

## Źródła

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
