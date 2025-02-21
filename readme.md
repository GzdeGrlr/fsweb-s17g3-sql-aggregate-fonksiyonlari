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
#ilk 3 soruyu join kullanmadan yazın.

    1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    	select ograd,ogrsoyad,atarih from ogrenci,islem where ogrenci.ogrno=islem.ogrno order by ograd

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    	select kitapadi,turadi from kitap,tur where kitap.turno = tur.turno and turadi in ("Hikaye","Fıkra")

    	//select kitapadi,turadi from kitap,tur where kitap.turno = tur.turno and turadi="Hikaye" and turadi="Fıkra"

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    	select ograd,ogrsoyad,ogrenci.ogrno, kitapadi from ogrenci,islem,kitap where (sinif ="10B" or "10C") and ogrenci.ogrno=islem.ogrno and kitap.kitapno=islem.kitapno

    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    	select ograd,ogrsoyad,atarih from ogrenci inner join islem on ogrenci.ogrno=islem.ogrno

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    	select kitapadi,turadi from kitap inner join tur on kitap.turno = tur.turno where turadi in ("Hikaye", "Fıkra")


    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    	select ogrenci.ogrno, ograd, ogrsoyad,sinif,kitapadi from ogrenci inner join islem on ogrenci.ogrno=islem.ogrno inner join kitap on islem.kitapno=kitap.kitapno where sinif in ("10B", "10C") order by ogrenci.ograd


    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    	select ograd,ogrsoyad,atarih from ogrenci left join islem on ogrenci.ogrno=islem.ogrno

    8) Kitap almayan öğrencileri listeleyin.

    	select ograd,ogrsoyad, atarih from ogrenci left join islem on ogrenci.ogrno=islem.ogrno where atarih is null

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    	select kitap.kitapno, kitap.kitapadi,count(islem.islemno) from kitap left join islem on kitap.kitapno=islem.kitapno group by kitap.kitapadi,kitap.kitapno order by kitap.kitapno

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    	select kitap.kitapno,kitap.kitapadi,count(islem.islemno) from kitap
    	left join islem on kitap.kitapno=islem.kitapno
    	group by kitap.kitapadi, kitap.kitapno ,islem.kitapno
    	order by count(*)

    	select kitap.kitapno,kitap.kitapadi,count(islemno) as defa from kitap left join islem on kitap.kitapno=islem.kitapno group by kitap.kitapadi, kitap.kitapno ,islem.kitapno order by defa

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    	select ograd,ogrsoyad,kitapadi from ogrenci
    	left join islem on ogrenci.ogrno=islem.ogrno
    	left join kitap on islem.kitapno=kitap.kitapno

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

    	select ograd, ogrsoyad, kitapadi, yazarad, yazarsoyad, tur.turadi, islem.atarih from ogrenci
    	left join islem on ogrenci.ogrno = islem.ogrno
    	left join kitap on islem.kitapno = kitap.kitapno
    	left join yazar on kitap.yazarno = yazar.yazarno
    	left join tur on kitap.turno= tur.turno

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

    	1-select ograd,ogrsoyad,count(islemno) as okunanKitapSayisi from ogrenci left join islem on ogrenci.ogrno=islem.ogrno where sinif in("10A","10B") group by ograd,ogrsoyad,sinif

    	2-select ograd,ogrsoyad,count(islemno) as okunanKitapSayisi from ogrenci left join islem on ogrenci.ogrno=islem.ogrno where sinif in("10A","10B") group by ogrenci.ogrno

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

    	select avg(sayfasayisi) from kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

    	select kitapadi,sayfasayisi from kitap where sayfasayisi > (select avg(sayfasayisi) from kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

    	select count(ogrno) from ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    	select count(ogrno) as toplamSayi from ogrenci

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    	select count(distinct ograd) from ogrenci;

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    	select max(sayfasayisi) from kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    	select kitapadi,sayfasayisi from kitap where sayfasayisi=(select max(sayfasayisi) from kitap)

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    	select min(sayfasayisi) from kitap

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    	select kitapadi,sayfasayisi from kitap where sayfasayisi=(select min(sayfasayisi) from kitap)

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

    	select max(sayfasayisi),turadi from kitap,tur where kitap.turno = tur.turno and turadi="Dram"

    	select max(sayfasayisi) from kitap inner join tur on kitap.turno= tur.turno where tur.turadi= "Dram"

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

    	select sum(sayfasayisi) from ogrenci,islem,kitap
    	where ogrenci.ogrno=islem.ogrno
    	and islem.kitapno=kitap.kitapno
    	and ogrenci.ogrno=15

    	--joinli
    	select sum(sayfasayisi), ogrenci.ogrno from ogrenci
    	left join islem on ogrenci.ogrno=islem.ogrno
    	left join kitap on kitap.kitapno=islem.kitapno
    	where ogrenci.ogrno=15

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

    	select ograd, count(ograd) from ogrenci group by ograd

    26) Her sınıftaki öğrenci sayısını bulunuz.

    	select sinif, count(*) from ogrenci group by sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

    	select sinif, cinsiyet, count(cinsiyet) from ogrenci group by sinif,cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

    	Select ograd, ogrsoyad, SUM(sayfasayisi) AS toplamSayfaSayisi From ogrenci
    	INNER JOIN islem ON islem.ogrno = ogrenci.ogrno
    	INNER JOIN kitap ON kitap.kitapno = islem.kitapno
    	GROUP By ogrenci.ogrno
    	ORDER BY toplamSayfaSayisi;

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

    	1-
    	Select ograd, ogrsoyad, COUNT(kitapadi) AS toplamkitap From ogrenci
    	INNER JOIN islem ON islem.ogrno = ogrenci.ogrno
    	INNER JOIN kitap ON kitap.kitapno = islem.kitapno
    	GROUP By ogrenci.ogrno
    	ORDER BY ograd;

    	2-
    	select ograd,ogrsoyad, count(islemno) from ogrenci INNER JOIN islem ON(islem.ogrno=ogrenci.ogrno) GROUP BY ogrenci.ogrno;
