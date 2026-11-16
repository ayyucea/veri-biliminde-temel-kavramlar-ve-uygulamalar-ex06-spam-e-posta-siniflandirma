# veri-biliminde-temel-kavramlar-ve-uygulamalar-ex06-spam-e-posta-siniflandirma
Veri Biliminde Temel Kavramlar ve Uygulamalar dersi kapsamında spam e-postaların PCA ve Karar Ağacı kullanılarak sınıflandırılması; model performansının değerlendirilmesi.
# Ex-06: Spam E-posta Sınıflandırma

Bu çalışma, **Veri Biliminde Temel Kavramlar ve Uygulamalar** dersi kapsamında hazırladığım 6. egzersiz ödevidir. Bu ödevde, bir e-postanın spam olup olmadığını Karar Ağacı modeliyle tahmin etmeye çalıştım.

## Ödevin amacı

Spam e-postaları, e-postalardaki kelime ve karakter özelliklerinden yararlanarak sınıflandırmak ve modelin performansını farklı ölçütlerle değerlendirmek.

## Veri seti

Çalışmada 4601 e-postadan oluşan `spam.csv` veri setini kullandım. Veri setinde e-postaları tanımlayan altı özellik (`crl.tot`, `dollar`, `bang`, `money`, `n000`, `make`) ve hedef değişken olarak `yesno` sütunu bulunuyor. `yesno` sütununda `n` spam olmayan, `y` ise spam olan e-postaları gösteriyor.

Veri seti, [Rdatasets içindeki Spam E-mail Data](https://vincentarelbundock.github.io/Rdatasets/doc/DAAG/spam7.html) verisinin bir alt kümesidir.

## Uygulama adımları

Notebook'ta şu işlemleri gerçekleştirdim:

1. Veriyi yükleyip sütunlarını ve boyutunu inceledim.
2. `yesno` hedef sütununu sayısal etiketlere dönüştürdüm: `n = 0`, `y = 1`.
3. Veriyi eğitim ve test kümelerine ayırdım. Test kümesi, verinin yüzde 20'sini oluşturdu.
4. PCA kullanarak altı özelliği iki bileşene indirdim.
5. PCA ile dönüştürülen eğitim verisi üzerinde Karar Ağacı modelini eğittim.
6. Modelin karar sınırını çizerek tahmin bölgelerini görselleştirdim.
7. Modeli accuracy, precision, recall, F1 ve AUC-ROC ölçütleriyle değerlendirdim.
8. PCA kullanmadan eğitilen Karar Ağacı sonuçlarını da karşılaştırma amacıyla inceledim.

## Sonuçlar

| Ölçüt | PCA ile | PCA olmadan |
|---|---:|---:|
| Accuracy | 0.809 | 0.841 |
| Weighted precision | 0.809 | 0.842 |
| Weighted recall | 0.809 | 0.841 |
| Weighted F1 | 0.807 | 0.840 |
| AUC-ROC | 0.769 | 0.810 |

PCA ile elde edilen sınıf sonuçlarına ayrıca baktığımda, model 390 spam e-postanın 283'ünü doğru sınıflandırdı ve 107'sini kaçırdı. Spam sınıfı için recall değeri 0.726, precision değeri 0.804 oldu.

## Kısa değerlendirme

PCA, özellikleri iki bileşene indirerek karar sınırını iki boyutlu bir grafikte incelememi sağladı. Ancak bu çalışmadaki karşılaştırmada PCA kullanılan modelin performansı, PCA kullanılmayan modele göre daha düşük çıktı. PCA öncesinde özellikleri standartlaştırmadığım için ölçekleri farklı olan sütunlar bileşenleri etkilemiş olabilir. Nitekim PCA'da varyansın neredeyse tamamı ilk bileşende toplandı.

Bu çalışmada spam e-postaları kaçırmamak önemli olduğu için spam sınıfının recall değeri özellikle bilgilendiricidir. Model, gerçek spam e-postaların %72,6’sını doğru yakalamış ve 107 spam e-postayı kaçırmıştır. Bununla birlikte spam sınıfının precision değeri 0,804’tür; bu ölçüt, spam olarak işaretlenen e-postaların ne kadarının gerçekten spam olduğunu gösterir. Karar sınırı grafiği iki PCA bileşenindeki tahmin bölgelerini görselleştirir; modelin başarısını değerlendirmek için bu görseli test kümesindeki metriklerle birlikte ele almak gerekir.

## Dosyalar

- `ex-06.ipynb`: Veri hazırlama, PCA, Karar Ağacı, görselleştirme ve değerlendirme adımlarını içeren notebook
- `data/spam.csv`: Kullanılan veri seti
```**
