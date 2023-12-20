# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın. 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
SELECT ö.ograd, ö.ogrsoyad, i.atarih
FROM ogrenci ö, islem i
WHERE ö.ogrno = i.ogrno;

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    SELECT K.kitapadi ,T.turadi From kitap K
    INNER JOIN tur T ON K.turno=T.turno
    WHERE T.turadi IN("Fıkra",Hikaye)


    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
    SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi
    FROM ogrenci o
    JOIN islem i ON o.ogrno = i.ogrno
    JOIN kitap k ON i.kitapno = k.kitapno
    WHERE o.sinif IN ('10B', '10C');


    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
    SELECT o.ograd, o.ogrsoyad, i.atarih
    FROM ogrenci o
    INNER JOIN islem i ON o.ogrno = i.ogrno;


    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    SELECT K.kitapadi ,T.turadi From kitap K
    INNER JOIN tur T ON K.turno=T.turno
    WHERE T.turadi IN("Fıkra",Hikaye)


    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
    SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi
    FROM ogrenci o
    INNER JOIN islem i ON o.ogrno = i.ogrno
    INNER JOIN kitap k ON i.kitapno = k.kitapno
    WHERE o.sinif IN ('10B', '10C')
    ORDER BY o.ograd, o.ogrsoyad;


    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
    SELECT o.ograd, o.ogrsoyad, i.atarih
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno = i.ogrno;


    8) Kitap almayan öğrencileri listeleyin.
    SELECT o.ograd, o.ogrsoyad
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno = i.ogrno
    WHERE i.ogrno IS NULL;


    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
    Select k.kitapno,kitapadi,COUNT(i.kitapno)AS alim_sayisi
    FROM kitap k
    LEFT JOIN islem i ON k.kitapno=i.kitapno
    GROUP BY k.kitapno,k.kitapadi
    ORDER BY k.kitapno;


    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
    Select k.kitapno,kitapadi,COUNT(i.kitapno)AS alim_sayisi
    FROM kitap k
    LEFT JOIN islem i ON k.kitapno=i.kitapno
    GROUP BY k.kitapno,k.kitapadi
    ORDER BY k.kitapno;


    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
    SELECT o.ograd, o.ogrsoyad, k.kitapadi
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno = i.ogrno
    LEFT JOIN kitap k ON i.kitapno = k.kitapno;


    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
    SELECT o.ograd, o.ogrsoyad, k.kitapadi, y.yazaradi, y.yazarsoyadi, t.turadi, i.atarih
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno = i.ogrno
    LEFT JOIN kitap k ON i.kitapno = k.kitapno
    LEFT JOIN yazar y ON k.yazarno = y.yazarno
    LEFT JOIN tur t ON k.turno = t.turno;



    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
    SELECT o.ograd, o.ogrsoyad, COUNT(i.kitapno) AS kitap_sayisi
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno = i.ogrno
    WHERE o.sinif IN ('10A', '10B')
    GROUP BY o.ograd, o.ogrsoyad;


    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG
    SELECT AVG(sayfasayisi) AS ortalama_sayfa_sayisi
    FROM kitap;


    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
    SELECT * FROM kitap
    WHERE sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap);


    16) Öğrenci tablosundaki öğrenci sayısını gösterin
    SELECT COUNT(ogrno) AS toplam_ogrencı_sayisi
    FROM ogrenci;


    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
    SELECT COUNT(ogrno) AS toplam_ogrencı_sayisi
    FROM ogrenci;


    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
    SELECT COUNT(DISTINCT ograd) AS farkli_isim_sayisi
    FROM ogrenci;


    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    SELECT MAX(sayfasayisi) AS en_fazla_sayfa_sayisi
    FROM kitap;


    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    Select kitapadi,sayfasayisi FROM kitap
    WHERE sayfasayisi=(SELECT MAX(sayfasayisi)FROM kitap)


    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    SELECT MIN(sayfasayisi) AS en_az_sayfa_sayisi
    FROM kitap;


    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    Select kitapadi,sayfasayisi FROM kitap
    WHERE sayfasayisi=(SELECT MIN(sayfasayisi)FROM kitap)


    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
    SELECT MAX(sayfasayisi) AS en_fazla_sayfa_sayisi
    FROM kitap
    WHERE turno = (SELECT turno FROM tur WHERE turadi = 'Dram');


    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
    SELECT o.ograd, o.ogrsoyad, SUM(k.sayfasayisi) AS toplam_sayfa_sayisi
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno = i.ogrno
    LEFT JOIN kitap k ON i.kitapno = k.kitapno
    WHERE o.ogrno = 15;


    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
    SELECT ograd, COUNT(*) AS adet
    FROM ogrenci
    GROUP BY ograd;


    26) Her sınıftaki öğrenci sayısını bulunuz.
    SELECT sinif,COUNT(*) AS ogrenci_sayisi
    FROM ogrenci
    GROUP BY sinif;

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
    SELECT sinif, cinsiyet, COUNT(*) AS ogrenci_sayisi
    FROM ogrenci
    GROUP BY sinif, cinsiyet;


    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
    SELECT o.ograd,o.ogrsoyad,SUM(k.sayfasayisi) AS toplam_sayfa_sayisi
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno=i.ogrno
    LEFT JOIN kitap k ON i.kitapno=k.kitapno
    GROUP BY o.ograd,o.ogrsoyad
    ORDER BY SUM(k.sayfasayisi) DESC

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.
    SELECT o.ograd,o.ogrsoyad,COUNT(k.kitapno) AS okunulan_kitap
    FROM ogrenci o
    LEFT JOIN islem i ON o.ogrno=i.ogrno
    GROUP BY o.ograd,o.ogrsoyad
