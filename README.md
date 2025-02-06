# Model-Agnostic Meta Learning(MAML) Few-Shot Learning

## Ukratko

Ovaj projekat koristi 
**Model-Agnostičkog Meta-Učenja (MAML)** 
za treniranje modela uz malo instanci podataka sa ciljem analize
sentimenta u rečenici. Glavni model korišćen u ovom projektu je 
**DistilBERT**, koji je prvo istreniran na **IMDB skupu podataka** pa 
potom adaptiran za tri različita skupova podataka koristeći 
MAML: **Yelp**, **Amazon** i **Sentiment140**. 


## MAML (Model-Agnostičko Meta-Učenje)

**MAML** (Model-Agnostic Meta-Learning) je tehnika meta-učenja koja omogućava modelima da nauče kako da se brzo prilagode na nove zadatke sa minimalnim brojem trening primera čime se poboljšava efikasnost i ušteda resursa. MAML koristi dva ključna koraka:

1. **Unutrašnji korak (Inner loop)**: U ovom koraku, model se trenira na maloj količini podataka iz svakog zadatka. Model se prilagođava specifičnom zadatku tako što se ažuriraju njegovi parametri, ali samo za taj zadatak.

2. **Spoljni korak (Outer loop)**: Nakon što se model prilagodi na pojedinačne zadatke, parametri modela se ažuriraju na osnovu performansi modela na različitim zadacima. Ovaj korak omogućava da model generalizuje i bude sposoban da se brzo prilagodi novim, sličnim zadacima.

U ovom projektu, **MAML** je korišćen kako bi **DistilBERT** mogao brzo da se prilagodi različitim skupovima podataka (kao što su Amazon, Yelp i Sentiment140) sa samo nekoliko označenih primera, što omogućava efikasno učenje sa malim brojem trening podataka, a istovremeno omogućava modelu da postigne visoke performanse na novim zadacima.

## Skupovi podataka

Korišćeni podaci za analizu sentimenta su:

- **IMDB**: Recenzije filmova
- **Yelp Polarity**: Recenzije restorana
- **Amazon Polarity**: Recenzije proizvoda sa amazon-a
- **Sentiment140**: Skup podataka sa twitera(današnji X)

Model je prvo klasično istreniran na 
**IMDB** skupu, a zatim adaptiran za 
**Yelp**, **Amazon** i **Sentiment140** MAML-a.

### Odlike skupova i parametara
- Ovaj projekat raspolaže sa oko **5,3 miliona** instanci podataka.
- **meta_lr**: Stopa učenja. Koristi se za ažuriranje modela tokom meta-treninga.
- **inner_lr**: Stopa učenja za unutrašnju optimizaciju. Koristi se za treniranje modela unutar svake iteracije na svakom zadatku.
- **num_support**: Broj instanci u *support* skupu za svaki zadatak. Ovaj skup se koristi za obučavanje modela na svakom zadatku.
- **num_query**: Broj instanci u *query* skupu za svaki zadatak. Ovaj skup se koristi za evaluaciju modela na svakom zadatku.
- **inner_steps**: Broj koraka optimizacije unutar svakog zadatka. To određuje koliko puta će model učiti unutar svakog zadatka.
- **batch_size**: Broj zadataka koji se obrađuju u jednom koraku treniranja.
- **meta_epochs**: Broj epoha tokom meta-treninga. Ovo određuje koliko puta će ceo skup zadataka proći kroz obuku.
- Za obuku MAML modela koriste se samo **5 primera po zadatku (support set)** i **5 primera za testiranje (query set)**.
- Za treniranje ovog modela korišćeno je **800** instanci, odnosno **0.016%** podataka<br>
*Broj instanci = (Broj zadataka) * (Broj instanci u support skupu + Broj instanci u query skupu)*

## Fajlovi

### NLP
Fajl **NLP** sadrži implementaciju modela baziranog na **DistilBERT** 
koji je klasično istreniran za sentiment analizu na IMDB skupu podataka.
**DistilBERT** je efikasna i brza verzija **BERT** modela, koja je optimizovana za brže izvođenje bez značajnog gubitka u tačnosti. 
Ovaj fajl obuhvata učitavanje, treniranje i daje primere njegovog korišćenja.

### NLP_MAML
Fajl **NLP_MAML** sadrži implementaciju tehnike **MAML** za brzo 
prilagođavanje modela na različite zadatke. Ovaj fajl koristi
prethodno istreniran **DistilBERT** model i primenjuje MAML kako 
bi model bio sposoban da se brzo prilagodi na nove domene 
(kao što su Yelp, Amazon, i Sentiment140), koristeći samo 
nekoliko primeraka za svaki zadatak.

### Interface
Fajl **Interface** daje korisniku jednostavnu grafičku komponentu
gde može da ubaci svoju rečenicu vezanu za ova 4 domena i dobije rezultat.
Pogledati primere sa slikama.

### NLP_MAML_optuna

Ima istu funkciju kao i NLP_MAML. Koristi optuna algoritam, Bajesovsku optimizaciju,
kako bi pronašla najbolje parametre i dinamički ih menjala. 
I ako daje odlične rezultate, odlučio sam se ipak da je ne koristim
u finalnoj verziji projekta kako bih bolje prikazao moć MAML-a. 
Van demonstracije, ova tehnika ima odličnu primenu u praksi. 


## Rezultati

### 1. Amazon Polarity
- **Accuracy:** 0.8728
- **Classification Report:**
    ```
              precision    recall  f1-score   support

           0       0.95      0.79      0.86    200177
           1       0.82      0.96      0.88    200321

    accuracy                           0.87    400498
    macro avg      0.88      0.87      0.87    400498
    weighted avg   0.88      0.87      0.87    400498
    ```

### 2. Yelp Polarity
- **Accuracy:** 0.8758
- **Classification Report:**
    ```
              precision    recall  f1-score   support

           0       0.95      0.79      0.86    219177
           1       0.82      0.96      0.89    219321

    accuracy                           0.88    438498
    macro avg       0.89      0.88     0.87    438498
    weighted avg    0.89      0.88     0.87    438498
  ```
  
### 3. Sentiment140
- **Accuracy:** 0.7952
- **Classification Report:**
    ```
              precision    recall  f1-score   support

           0       0.80      0.57      0.66       177
           1       0.80      0.92      0.85       321

    accuracy                           0.80       498
    macro avg       0.80     0.74      0.76       498
    weighted avg    0.80     0.80      0.79       498
    ```
#### Na uticaj skupa sentiment140 uticalo je postojanje neutralne klase što je malo poremetilo model na ovom skupu, ali i sa tim problem se model odlično poneo na kraju.
## Instalacija biblioteke

Za instalaciju potrebnih biblioteka, koristite sledeću komandu:

```bash
pip install -r requirements.txt
```

## Logovanje
Korišćen je wandb sistem za logovanje. Pogledati sve u linku ispod:<br>
https://wandb.ai/kostic-stojan23-university-of-belgrade/projects<br>
Za prikaz svih grafika koji su nastali treniranjem na ovom projektu neophodan je wandb nalog.
## Zaključak

Ovaj projekat pokazuje moć učenja sa ograničenim instancama. 
MAML nije vezan ni za kakav model niti skup podataka već je 
univerzalna tehnika. U malo iteracija može da da odlične rezultate i time 
uštedi mnogo vremena kao i resursa.

## Primer mog projekta u praksi
![Screenshot 2025-02-06 144253.png](pictures/Screenshot%202025-02-06%20144253.png)
![Screenshot 2025-02-06 144346.png](pictures/Screenshot%202025-02-06%20144346.png)
![Screenshot 2025-02-06 144410.png](pictures/Screenshot%202025-02-06%20144410.png)
![Screenshot 2025-02-06 144428.png](pictures/Screenshot%202025-02-06%20144428.png)
