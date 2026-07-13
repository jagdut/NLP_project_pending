# NLP_project: Analiza książki "Chłopi. Lato" Władysława Reymonta

**Autorki:** Gabriela Marchwica, Jagoda Dutkiewicz

Projekt NLP analizujący czwartą (ostatnią) część *Chłopów* Władysława Reymonta pod kątem sieci powiązań między bohaterami oraz generowania i oceny alternatywnych zakończeń powieści przy pomocy dużych modeli językowych.

## 🎯 Cel projektu

Celem projektu była analiza tomu *Lato* pod względem:

- powiązań bohaterów i tworzonych przez nich społeczności (graf współwystępowania postaci),
- wygenerowania alternatywnych zakończeń książki przy użyciu **Bielik-4.5B-v3.0** oraz **OpenAI GPT-4o mini**,
- porównania wygenerowanych tekstów z oryginałem (podobieństwo cosinusowe, metryka ROUGE, bogactwo językowe),
- porównania wygenerowanych tekstów między sobą (Bielik vs OpenAI) – podobieństwo cosinusowe, liczba słów, podobieństwo Jaccarda,
- analizy wygenerowanych zakończeń za pomocą Wordcloud.

## 📚 Dane

Tekst czwartej części *Chłopów* W. Reymonta pobrany ze strony [wolnelektury.pl](https://wolnelektury.pl/) w formacie `.txt`.

## 🛠️ Zastosowane techniki i narzędzia

### Generowanie grafu powiązań bohaterów
- **spaCy** (model `pl_core_news_md`) – rozpoznawanie wystąpień postaci w tekście
- **NetworkX** – budowa grafu współwystępowania
- **Matplotlib** – wizualizacja grafu
- **python-louvain** (`community_louvain`) – wykrywanie społeczności w grafie

### Przygotowanie tekstu do generowania (Bielik i GPT)
- czyszczenie tekstu z opisów przyrody (usunięcie linijek niezawierających imion postaci),
- podział tekstu i ustalenie punktu odcięcia (usunięcie oryginalnego zakończenia od wskazanego zdania, tak by modele nie znały dalszego ciągu).

### Generowanie alternatywnych zakończeń – Bielik-4.5B-v3.0
Wygenerowano trzy warianty przy różnych instrukcjach systemowych i parametrach:

| Wariant | temperature | top_p | repetition_penalty | max_new_tokens |
|---|---|---|---|---|
| Wersja 1 (+) | 1.1 | 0.92 | 1.15 | 1000 |
| Wersja 2 (+) | 0.6 | 0.9 | 1.15 | 1000 |
| Wersja 3 (–) | 0.75 | 0.95 | 1.15 | 1000 |

### Generowanie alternatywnych zakończeń – OpenAI GPT-4o mini

| Wariant | temperature | top_p | presence_penalty | max_tokens |
|---|---|---|---|---|
| Wersja pozytywna | 0.6 | 0.9 | 0.3 | 1000 |
| Wersja negatywna | 0.6 | 0.95 | 0.3 | 1000 |

### Analiza porównawcza
- **TF-IDF + podobieństwo cosinusowe** (n-gramy znakowe 3–5) – porównanie z oryginałem oraz modeli między sobą
- **ROUGE-1 / ROUGE-2 / ROUGE-L** – porównanie z oryginalnym zakończeniem
- **TTR (Type-Token Ratio)** – bogactwo językowe wygenerowanych tekstów
- **Podobieństwo Jaccarda** – pokrycie unikalnych słów między modelami
- **WordCloud** – wizualizacja najczęstszych słów (po lematyzacji i usunięciu stopwords)

## 📊 Wyniki

### Graf powiązań bohaterów
Wygenerowany graf pokazuje dwie wyraźne społeczności: jedną skupioną wokół Jagny (Jagna, Dominikowa, Jasio), drugą złożoną z pozostałych głównych bohaterów. Grubość połączeń odzwierciedla częstość wspólnego występowania postaci – najsilniejsze powiązanie to para Jagna–Antek, zgodnie z fabułą książki.

### Podobieństwo cosinusowe do oryginału

| Model | Wariant | Podobieństwo do Reymonta |
|---|---|---|
| Bielik-4.5B-v3.0 | Wersja 1 (+) | 0.4880 |
| Bielik-4.5B-v3.0 | Wersja 2 (+) | 0.4832 |
| Bielik-4.5B-v3.0 | Wersja 3 (–) | 0.4922 |
| GPT-4o mini | Wersja 1 (+) | 0.5424 |
| GPT-4o mini | Wersja 2 (–) | 0.5457 |

### Metryka ROUGE (względem oryginalnego zakończenia)

| Model | Wariant | ROUGE-1 | ROUGE-2 | ROUGE-L |
|---|---|---|---|---|
| Bielik-4.5B-v3.0 | Wersja 1 (+) | 0.1336 | 0.0225 | 0.0594 |
| Bielik-4.5B-v3.0 | Wersja 2 (+) | 0.1240 | 0.0226 | 0.0608 |
| Bielik-4.5B-v3.0 | Wersja 3 (–) | 0.1050 | 0.0229 | 0.0565 |
| GPT-4o mini | Wersja 1 (+) | 0.1260 | 0.0240 | 0.0522 |
| GPT-4o mini | Wersja 2 (–) | 0.1232 | 0.0251 | 0.0530 |

### Bogactwo językowe (TTR)

| Tekst | TTR |
|---|---|
| Oryginał (Reymont) | 0.53 |
| Bielik – Wersja 1 (+) | 0.85 |
| Bielik – Wersja 2 (+) | 0.80 |
| Bielik – Wersja 3 (–) | 0.80 |
| GPT-4o mini – Wersja 1 (+) | 0.77 |
| GPT-4o mini – Wersja 2 (–) | 0.79 |

### Bielik vs OpenAI (wersje odpowiadające sobie)

| Porównanie | Podobieństwo cosinusowe | Liczba słów | Podobieństwo Jaccarda |
|---|---|---|---|
| Bielik Wersja 2 (+) vs OpenAI Wersja 1 (+) | 0.25 | 453 vs 470 | 0.07 |
| Bielik Wersja 3 (–) vs OpenAI Wersja 2 (–) | 0.27 | 402 vs 459 | 0.08 |

## 💡 Wnioski

- Mimo dużego uproszczenia sposobu wyznaczania współwystępowania bohaterów, otrzymany graf społeczności ma sens fabularny – oddaje podział na dwie grupy postaci oraz siłę relacji Jagna–Antek.
- **Bielik**, poproszony o alternatywne zakończenie, domyślnie generował wersję pozytywną i dopiero po wyraźnym wskazaniu tworzył wersję negatywną. **GPT-4o mini** odwrotnie – domyślnie generował zakończenie negatywne, którego dramatyzm można było dodatkowo wzmocnić.
- Przy mniej precyzyjnych promptach Bielik potrafił wstawiać w tekst elementy niepasujące stylistycznie (np. emotikony) – dopiero doprecyzowanie instrukcji poprawiło jakość generowanych tekstów.
- GPT-4o mini częściej kończył tekst w połowie zdania, podczas gdy Bielik kończył go sensownie, kropką.
- GPT-4o mini osiągnął nieznacznie wyższe podobieństwo cosinusowe do oryginału (~0.54) niż Bielik (~0.48).
- Pod względem bogactwa językowego (TTR) najlepiej wypadł Bielik (0.80–0.85), następnie GPT-4o mini (0.77–0.79), a najniżej – sam oryginał (0.53). Może to oznaczać, że modele generują bardziej zróżnicowany leksykalnie tekst niż Reymont lub nie odwzorowują w pełni powtarzalności charakterystycznej dla stylizowanej gwary.
- Bardzo niskie wartości ROUGE-2 (~0.02) dla wszystkich wariantów wskazują, że modele nie kopiują fraz z oryginału, lecz tworzą własny tekst.
- Podobieństwo Jaccarda między odpowiadającymi sobie wersjami Bielika i GPT (0.07–0.08) pokazuje, że oba modele – mimo identycznych instrukcji i podobnych parametrów – wygenerowały treściowo odmienne zakończenia.

## 📁 Struktura repozytorium

```
.
├── README.md
├── NLP_Peasants_alternative.ipynb        # notebook z pełnym kodem analizy
├── graph_peasants.png                    # graf przedstawiający powiązania bohaterów
└── generated_endings/                     # wygenerowane alternatywne zakończenia
```

## ⚙️ Wymagania

Notebook korzysta m.in. z następujących bibliotek:

```
spacy
pl_core_news_md (model spaCy dla języka polskiego)
networkx
matplotlib
python-louvain
transformers
torch
huggingface_hub
openai
scikit-learn
pandas
rouge-score
wordcloud
nltk
```

Instalacja modelu spaCy dla języka polskiego:

```bash
python -m spacy download pl_core_news_md
```

## 🔑 Klucze API

Do uruchomienia notebooka potrzebne są własne klucze/tokeny:
- token Hugging Face (do pobrania modelu Bielik-4.5B-v3.0),
- klucz API OpenAI (do modelu GPT-4o mini).

