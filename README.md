# Bandit Algoritmaları ile Reklam Tıklama Optimizasyonu (CTR)

Bu proje, çok kollu haydut (**multi-armed bandit**) problemine yönelik dört farklı algoritmanın aynı veri seti üzerinde uygulanmasını ve karşılaştırılmasını içerir. Amaç, 10 farklı reklamdan hangisinin en yüksek tıklama oranına (CTR) sahip olduğunu, mümkün olan en az kayıpla (regret) ve en verimli şekilde bulmaktır.

## İçerik

| Dosya | Açıklama |
|---|---|
| `Ads_CTR_Optimisation.csv` | 10.000 kullanıcı × 10 reklamdan oluşan veri seti. Hücre değeri `1` ise kullanıcı reklama tıklamıştır, `0` ise tıklamamıştır. |
| `analiz1abtest.ipynb` | A/B Testi implementasyonu |
| `analiz1egreedy.ipynb` | Epsilon-Greedy implementasyonu |
| `analiz1ucb.ipynb` | UCB (Upper Confidence Bound) implementasyonu |
| `analiz1thompsonsamp.ipynb` | Thompson Sampling implementasyonu |

## Problem Tanımı

Elimizde 10 reklam var ve her birinin gerçek tıklanma olasılığı bilinmiyor. Her kullanıcıya bir reklam gösterilecek ve tıklayıp tıklamadığı gözlemlenecek. Hedef, toplam tıklama sayısını maksimize edecek şekilde reklamları seçmektir. Bu, klasik **keşif (exploration) - kullanım (exploitation)** dengesi problemidir.

## Kullanılan Algoritmalar

### 1. A/B Testi
İlk 2.000 kullanıcıya reklamlar tamamen rastgele gösterilir, en yüksek ortalama tıklama oranına sahip reklam belirlenir ve kalan 8.000 kullanıcıya yalnızca bu reklam gösterilir. Keşif ve kullanım aşamaları kesin olarak birbirinden ayrılmıştır.

### 2. Epsilon-Greedy
Her adımda `%ε` (epsilon) olasılıkla rastgele bir reklam denenir (keşif), `%(1-ε)` olasılıkla o ana kadarki en iyi ortalamaya sahip reklam seçilir (kullanım). Bu projede `ε = 0.10` kullanılmıştır.

### 3. UCB (Upper Confidence Bound)
Rastgelelik içermeyen, tamamen deterministik bir yöntemdir. Her reklam için ortalama ödüle ek olarak, az denenmiş reklamlar için daha büyük olan bir "güven payı" hesaplanır. Seçim, bu üst güven sınırı en yüksek olan reklama göre yapılır.

### 4. Thompson Sampling
Bayesyen bir yaklaşımdır. Her reklamın tıklanma olasılığı bir Beta dağılımı ile modellenir ve her adımda bu dağılımlardan örnekleme yapılarak en yüksek örneğe sahip reklam seçilir.

## Sonuçlar

10.000 kullanıcı üzerinde çalıştırılan algoritmaların elde ettiği toplam tıklama sayıları:

| Yöntem | Toplam Tıklama | En Çok Seçilen Reklam |
|---|---|---|
| A/B Testi | 2.388 | Reklam 5 |
| Epsilon-Greedy (ε = 0.10) | 2.543 | Reklam 5 |
| UCB | 2.178 | Reklam 5 |
| Thompson Sampling | 2.607 | Reklam 5 |

Tüm yöntemler doğru şekilde **Reklam 5**'in en yüksek tıklama oranına sahip olduğunu tespit etmiştir. Aralarındaki fark, bu sonuca ne kadar verimli (az kayıpla) ulaşıldığındadır. Bu veri setinde en iyi sonucu **Thompson Sampling**, en düşük sonucu ise **UCB** vermiştir.

## Kurulum ve Çalıştırma

```bash
git clone https://github.com/bybetulbeyza/bandit_algoritms.git
cd bandit_algoritms
pip install numpy pandas matplotlib
jupyter notebook
```

Ardından not defterlerinden istediğinizi açıp hücreleri sırayla çalıştırabilirsiniz. Her not defteri `Ads_CTR_Optimisation.csv` dosyasını aynı klasörden okur.

## Gereksinimler

- Python 3.x
- numpy
- pandas
- matplotlib
- Jupyter Notebook

## Notlar

- Sonuçların tekrarlanabilir olması için A/B Testi ve Epsilon-Greedy not defterlerinde `np.random.seed(42)` kullanılmıştır.
- UCB ve Thompson Sampling deterministik olmayan/rastgele bileşenler içerdiğinden (Thompson Sampling'de `random.betavariate`), çalıştırmalar arasında küçük farklılıklar gözlemlenebilir.
