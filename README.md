# Ex-06: Spam E-posta Sınıflandırma

Bu çalışma, **Veri Biliminde Temel Kavramlar ve Uygulamalar** dersi kapsamında hazırladığım 6. egzersiz ödevidir. Çalışmada e-postaları spam veya spam değil olarak sınıflandırmak için Karar Ağacı modeli kullandım. PCA’nın model performansına etkisini görebilmek için PCA uygulanmış ve uygulanmamış iki modeli karşılaştırdım.

## Ödevin amacı

E-postalara ait özelliklerden yararlanarak bir e-postanın spam olup olmadığını tahmin etmek ve Karar Ağacı modelinin başarısını farklı performans ölçütleriyle değerlendirmek.

## Veri seti

Çalışmada 4601 e-posta ve 7 sütundan oluşan `spam.csv` veri setini kullandım. Altı sütun modelin kullandığı özellikleri, `yesno` sütunu ise hedef sınıfı içeriyor.

- `crl.tot`: Kesintisiz büyük harf dizilerinin toplam uzunluğu
- `dollar`: `$` işaretinin kullanım oranı
- `bang`: `!` işaretinin kullanım oranı
- `money`: “money” kelimesinin kullanım oranı
- `n000`: “000” dizisinin kullanım oranı
- `make`: “make” kelimesinin kullanım oranı
- `yesno`: E-postanın spam olup olmadığını belirten hedef sütun (`n`: spam değil, `y`: spam)

Veri seti, [Rdatasets içindeki Spam E-mail Data](https://vincentarelbundock.github.io/Rdatasets/doc/DAAG/spam7.html) verisinin bir alt kümesidir.

## Uygulama adımları

1. Veri setini yükleyip boyutunu, sütunlarını ve veri türlerini inceledim.
2. `yesno` hedef sütununu sayısal değerlere dönüştürdüm: `n = 0`, `y = 1`.
3. Veriyi yüzde 80 eğitim ve yüzde 20 test olacak şekilde böldüm. `stratify=y` kullanarak sınıf oranlarının iki kümede de korunmasını sağladım.
4. **PCA’lı model için** özellikleri `StandardScaler` ile standartlaştırdım. Ölçekleyiciyi yalnızca eğitim verisine uyarlayıp test verisine aynı dönüşümü uyguladım.
5. PCA ile altı özelliği iki bileşene indirdim ve bu verilerle Karar Ağacı modelini eğittim.
6. PCA’lı modelin karar sınırını iki boyutlu grafikte gösterdim.
7. PCA’lı modeli test kümesinde accuracy, weighted precision, weighted recall, weighted F1 ve AUC-ROC ölçütleriyle değerlendirdim. Ayrıca spam sınıfının sonuçlarını ve confusion matrix’i inceledim.
8. Karşılaştırma için aynı eğitim/test bölmesini kullanarak, PCA uygulanmamış altı özgün özellikle ikinci bir Karar Ağacı modeli eğittim.

## Sonuçlar

| Ölçüt | PCA ile | PCA olmadan |
|---|---:|---:|
| Accuracy | 0.798 | 0.837 |
| Weighted precision | 0.797 | 0.837 |
| Weighted recall | 0.798 | 0.837 |
| Weighted F1 | 0.797 | 0.837 |
| AUC-ROC | 0.767 | 0.819 |

PCA’lı modelin sınıf bazlı sonuçlarında, test kümesindeki 363 spam e-postanın 263’ü doğru sınıflandırıldı, 100’ü kaçırıldı. Spam sınıfının recall değeri 0.725, precision değeri 0.754 oldu. Confusion matrix’e göre 558 spam olmayan e-postanın 472’si doğru sınıflandırıldı; 86’sı yanlışlıkla spam olarak tahmin edildi.

Standartlaştırılmış özelliklerle elde edilen iki PCA bileşeni, toplam varyansın yaklaşık **%44,8’ini** açıklıyor.

## Değerlendirme

Bu çalışmada spam e-postaları kaçırmamak önemli olduğu için spam sınıfının recall değeri özellikle bilgilendiricidir. Recall değeri 0.725 olduğundan model test kümesindeki spam e-postaların yaklaşık yüzde 72,5’ini yakaladı. Normal e-postaların yanlışlıkla spam olarak işaretlenmesini değerlendirmek için spam sınıfının precision değerine de bakmak gerekir.

Karar sınırı grafiği, PCA ile oluşturulan iki bileşen üzerindeki tahmin bölgelerini gösteriyor. Bu görsel, modelin iki boyutlu uzayda sınıfları nasıl ayırdığını incelemeye yardımcı oluyor; modelin genel başarısını değerlendirmek için test kümesindeki metriklerle birlikte ele alınmalıdır.

PCA’lı modelin performansı, bu karşılaştırmada PCA uygulanmamış modelden daha düşük çıktı. PCA, özellik sayısını ikiye indirerek görselleştirmeyi kolaylaştırdı; ancak iki bileşen toplam varyansın yaklaşık yüzde 44,8’ini açıkladığından bazı bilgiler kaybolmuş olabilir. PCA’sız modelin daha yüksek accuracy ve AUC-ROC değerleri, bu veri setinde altı özgün özellikle sınıflandırmanın daha başarılı olduğunu gösteriyor.

## Dosyalar

- `spam-e-posta-siniflandirma.ipynb`: Veri hazırlama, PCA, Karar Ağacı, karar sınırı görselleştirmesi ve model değerlendirmesini içeren notebook
- `spam.csv`: Kullanılan veri seti
