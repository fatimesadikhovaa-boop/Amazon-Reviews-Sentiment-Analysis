# Amazon Rəylərinin Sentiment Təhlili

Amazon rəylərini pozitiv/negativ olaraq ayıran bir NLP layihəsi. Məqsəd rəyin mətninə baxıb məhsulun bəyənilib-bəyənilmədiyini modelin özünün tapması idi.

## Dataset haqqında

3 sütun var: `P/N` (1 = negativ, 2 = pozitiv), `Title` və `Review`. Tam dataset olduqca ağır olduğu üçün 120k sətirlik nümunə götürdüm, 100k train, 20k test üçün. Dataset özü ölçüsünə görə repoda yoxdur, əlavə etmək istəsən mənbə linkini burda paylaş: [link].

## Metod

Title və Review-i birləşdirib TF-IDF ilə vektorlaşdırdım (unigram+bigram, max 50k feature). Sonra bir neçə model sınadım:

- Logistic Regression
- Linear SVM
- KNN (k=15)
- Random Forest (RandomizedSearchCV ilə tuning)

Bunlardan LR, SVM və RF-i birgə bir Voting Classifier-ə də saldım, görüm birlikdə daha yaxşı işləyirmi.

## Nəticələr

SVM: 91.53%
Logistic Regression: 91.47%
Voting: 91.46%
Random Forest: 82.38%
KNN: 77.68%

SVM ilə LR demək olar eyni yerdə bitirdi və ən yaxşı nəticə onlardan gəldi. Voting gözlədiyimdən yaxşı çıxmadı, RF-in zəif tərəfi ümumi nəticəni bir az aşağı çəkdi deyəsən. KNN isə açıq şəkildə geri qaldı, yüksək ölçülü seyrək datada bu metod elə də yaxşı işləmir.

Növbəti dəfə TF-IDF əvəzinə embedding-lərlə (məs. word2vec və ya kiçik bir transformer) sınamaq maraqlı olardı, RF üçün də feature sayını azaltmaq performansı yaxşılaşdıra bilər.

## İşə salmaq

```bash
pip install -r requirements.txt
jupyter notebook amazon_reviews_sentiment_analysis.ipynb
```

Python, pandas, scikit-learn, matplotlib, seaborn istifadə olunub.
