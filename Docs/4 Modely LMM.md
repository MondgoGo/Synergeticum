Áno. Spravím ti to ako **4 modely × odporúčaný git projekt × poznámka k 3 režimom**.

Môj cieľ tu nie je „najkrajší research stack“, ale **najrýchlejšie použiteľný základ**, ktorý sa dá nasadiť na mobile a neskôr zapojiť do lokálneho učenia a občasného syncu.

## 1. Vision model

**Model:** MobileNetV3 Small
**Git / zdroj:**

* TensorFlow MobileNetV3 API: `https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNetV3Small` ([TensorFlow][1])
* referenčný model card / parametre: `https://github.com/openvinotoolkit/open_model_zoo/blob/master/models/public/mobilenet-v3-small-1.0-224-tf/README.md` ([GitHub][2])

**Približná veľkosť:**

* ~2.5M parametrov, typicky cca **2.5–5 MB** po mobile exporte/optimalizácii; OpenVINO model card uvádza **2.537 MParams**. ([GitHub][2])

**Prečo sem patrí:**

* je to veľmi dobrý default na mobile pre obrazovú klasifikáciu a feature extraction. TensorFlow ho drží ako oficiálnu architektúru a je cielený na low-resource použitie. ([TensorFlow][1])

**3 režimy:**

* **Solo:** len lokálna inferencia a jemné dolaďovanie
* **Trusted Circle:** sync len s whitelist skupinou
* **Community Assist:** radšej zdieľať len embeddingy / prototypy, nie celé raw dáta

---

## 2. Text model

**Model:** TinyBERT
**Git / zdroj:**

* oficiálny repo: `https://github.com/huawei-noah/Pretrained-Language-Model`
* TinyBERT README: `https://github.com/huawei-noah/Pretrained-Language-Model/blob/master/TinyBERT/README.md` ([GitHub][3])

**Približná veľkosť:**

* typicky rátaj približne **5–15 MB** podľa variantu a kvantizácie. Repo potvrdzuje, že ide o TinyBERT vetvu v oficiálnom Huawei Noah pretrained model repozitári, ale presná veľkosť závisí od konkrétneho checkpointu a exportu. ([GitHub][3])

**Prečo sem patrí:**

* je to realistický malý NLP model na intent klasifikáciu, krátke texty, tagovanie a lightweight personal NLP. Nie je to najnovší hype model, ale je bližšie k mobilnej realite než plný BERT. ([GitHub][3])

**3 režimy:**

* **Solo:** text zostáva úplne lokálny
* **Trusted Circle:** môžeš syncovať len textový modul s rovnakou architektúrou
* **Community Assist:** zdieľaj skôr logits/embeddingy alebo distilled knowledge, nie citlivý text

---

## 3. Health model

**Model:** LightGBM
**Git / zdroj:**

* oficiálny repo: `https://github.com/lightgbm-org/LightGBM` ([GitHub][4])

**Približná veľkosť:**

* veľmi závisí od počtu stromov a feature priestoru, ale v praxi sa dá často držať niekde okolo **100 KB až 2 MB** pri menších mobilných tabuľkových modeloch. LightGBM je navrhnutý ako efektívny a pamäťovo úsporný gradient boosting framework. ([GitHub][4])

**Prečo sem patrí:**

* na health / biometrické / tabuľkové dáta je to často lepšia voľba než pchať tam neurónku len preto, že „AI“. LightGBM je rýchly, efektívny a veľmi silný na structured data. ([GitHub][4])

**3 režimy:**

* **Solo:** podľa mňa default pre health
* **Trusted Circle:** sync iba ak má skupina podobné feature schémy a podobný typ dát
* **Community Assist:** zdieľať len agregované model updates alebo anonymizované štatistiky

---

## 4. Behavior model

Tu by som bol k tebe úprimný: **na behavior sekvencie nemáš jeden jasný hotový „svätý grál“ git projekt**, ktorý by bol súčasne malý, mobilný, predtrénovaný a okamžite ideálny. Najpraktickejší stack je skôr:

**Model:** malý GRU / LSTM / 1D sequence model
**Git / zdroj:**

* PyTorch examples: `https://github.com/pytorch/examples` ako východisko pre sekvenčné modely ([GitHub][5])
* runtime/deployment vrstva: `https://github.com/microsoft/onnxruntime` pre mobile inferenciu a cross-platform nasadenie ([GitHub][5])

**Približná veľkosť:**

* pri malom GRU alebo 1D CNN typicky **200 KB až 2 MB**, podľa počtu vrstiev a hidden units. ONNX Runtime podporuje cross-platform inferenciu a dokonca aj training acceleration, ale veľkosť je už na tebe podľa architektúry. ([GitHub][5])

**Prečo takto:**

* pri behavior modeli je často dôležitejšie mať **správnu sekvenčnú reprezentáciu a dobré okná dát** než nájsť jeden univerzálny pretrained repo. Tu by som nehral divadlo, že existuje perfektný hotový model pre všetkých. Neexistuje.

**3 režimy:**

* **Solo:** veľmi silný fit, lebo behavior je vysoko osobné
* **Trusted Circle:** len ak ide o podobnú komunitu a podobný pattern použitia
* **Community Assist:** skôr zdieľanie pattern embeddings alebo trendových parametrov než plných sekvenčných updateov

---

## Moje finálne odporúčané párovanie

**Vision**
MobileNetV3 Small
Git:

* `https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNetV3Small`
* `https://github.com/openvinotoolkit/open_model_zoo/blob/master/models/public/mobilenet-v3-small-1.0-224-tf/README.md` ([TensorFlow][1])

**Text**
TinyBERT
Git:

* `https://github.com/huawei-noah/Pretrained-Language-Model`
* `https://github.com/huawei-noah/Pretrained-Language-Model/blob/master/TinyBERT/README.md` ([GitHub][3])

**Health**
LightGBM
Git:

* `https://github.com/lightgbm-org/LightGBM` ([GitHub][4])

**Behavior**
Small GRU / 1D sequence model + ONNX Runtime deployment
Git:

* `https://github.com/pytorch/examples`
* `https://github.com/microsoft/onnxruntime` ([GitHub][5])

---

## Tvrdé poradie podľa praktickosti

Ak by si to chcel rozbehnúť s najvyšším ROI, tak poradie by som dal takto:

1. **Health / LightGBM** — najnižšia bolesť, najlepší pomer sila/jednoduchosť. ([GitHub][4])
2. **Vision / MobileNetV3 Small** — veľmi silný mobilný default. ([TensorFlow][1])
3. **Text / TinyBERT** — použiteľné, ale už ťažšie. ([GitHub][3])
4. **Behavior / small GRU + ONNX Runtime** — najsilnejšie dlhodobo, ale najviac závislé od tvojho návrhu dát a sekvencií. ([GitHub][5])

Najdôležitejšia poznámka:
**federovať by som na začiatku nechal len Health a možno Vision.**
Text a hlavne Behavior by som najprv držal viac osobné a až potom riešil sync.

Ak chceš, ďalší krok ti z toho spravím ako čistú tabuľku:
**model | git | veľkosť | mobil fit | federovať áno/nie | odporúčaný režim**.

[1]: https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNetV3Small?utm_source=chatgpt.com "tf.keras.applications.MobileNetV3Small | TensorFlow v2.16.1"
[2]: https://github.com/openvinotoolkit/open_model_zoo/blob/master/models/public/mobilenet-v3-small-1.0-224-tf/README.md?utm_source=chatgpt.com "mobilenet-v3-small-1.0-224-tf"
[3]: https://github.com/huawei-noah/pretrained-language-model?utm_source=chatgpt.com "huawei-noah/Pretrained-Language-Model"
[4]: https://github.com/lightgbm-org/LightGBM?utm_source=chatgpt.com "lightgbm-org/LightGBM: A fast, distributed ..."
[5]: https://github.com/microsoft/onnxruntime?utm_source=chatgpt.com "microsoft/onnxruntime: ONNX Runtime: cross-platform ..."
