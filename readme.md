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

		CEVAP: select ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih from ogrenci ,islem where ogrenci.ogrno = islem.ogrno
	

	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

		CEVAP: select kitap.kitapadi, tur.turadi from kitap, tur where kitap.turno = tur.turno and (tur.turadi = 'fıkra' or tur.turadi = 'hikaye')
	
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

		CEVAP: select ogrenci.ogrno, ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi from islem, ogrenci, kitap where ogrenci.ogrno = islem.ogrno and islem.kitapno = kitap.kitapno and ogrenci.sinif in ('10B', '10C') order by ogrno
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

		CEVAP: select ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih from ogrenci inner join islem on ogrenci.ogrno = islem.ogrno 
	
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

		CEVAP: select kitap.kitapadi, tur.turadi from kitap inner join tur on kitap.turno = tur.turno where tur.turadi in ('Fıkra', 'Hikaye')

	
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.


		CEVAP: select o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi from ogrenci as o inner join islem as i on o.ogrno = i.ogrno inner join kitap as k on i.kitapno = k.kitapno where o.sinif in ('10B', '10C') order by o.ograd
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

		CEVAP: select ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih from ogrenci left join islem on ogrenci.ogrno = islem.ogrno 
	
	
	8) Kitap almayan öğrencileri listeleyin.

		CEVAP: select ogrenci.ograd, ogrenci.ogrsoyad from ogrenci left join islem on ogrenci.ogrno = islem.ogrno where islem.atarih is null;
	
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

		CEVAP: select kitap.kitapno, kitap.kitapadi, count(kitap.kitapno) as 'kitabın kaç defa alindigi' from islem inner join kitap on islem.kitapno = kitap.kitapno group by kitap.kitapno
	
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

		CEVAP: select kitap.kitapno, kitap.kitapadi, count(islem.kitapno) as 'kitabin_alim_sayisi' from kitap left join islem on islem.kitapno = kitap.kitapno group by kitap.kitapno order by count(islem.kitapno)


	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

		select concat(o.ograd, o.ogrsoyad) as 'adi_soyadi', k.kitapadi from ogrenci as o 
		inner join islem as i on o.ogrno = i.ogrno 
		inner join kitap as k on k.kitapno = i.kitapno
		order by adi_soyadi
	
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

		CEVAP: select concat(o.ograd, o.ogrsoyad) as 'adi_soyadi', concat(y.yazarad, y.yazarsoyad) as 'yazar_adi_soyadi', k.kitapadi ,t.turadi ,i.atarih  from ogrenci as o
		left join islem as i on i.ogrno = o.ogrno
		left join kitap as k on k.kitapno = i.kitapno
		left join yazar as y on y.yazarno = k.yazarno
		left join tur as t on t.turno = k.turno
		order by adi_soyadi
	
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

		CEVAP: select  concat(ogrenci.ograd, ogrenci.ogrsoyad) as 'adisoyadi', count(ogrenci.ogrno) as 'okudugu kitap sayisi' from ogrenci inner join islem on ogrenci.ogrno = islem.ogrno where ogrenci.sinif in ('10A', '10B') group by ogrenci.ogrno order by count(ogrenci.ogrno) 
	
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG

		CEVAP: select AVG(sayfasayisi) as 'ort_sayfasayisi' from kitap
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

		CEVAP: select * from kitap where sayfasayisi > (select avg(sayfasayisi) from kitap)
	
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin

		CEVAP: select count(ogrno) as 'ogrencisayisi' from ogrenci
	
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

		CEVAP: select count(ogrno) as 'toplam sayi' from ogrenci
	
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

		CEVAP: select count(distinct ograd) as 'farklı_ogrenciadlari' from ogrenci order by ograd
	
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
		CEVAP: select sayfasayisi as 'max_sayfasayisi' from kitap order by sayfasayisi desc limit 1
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

		CEVAP: select kitapadi, sayfasayisi from kitap order by sayfasayisi desc limit 1
	
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.


		CEVAP: select sayfasayisi as 'min_sayfasayisi' from kitap order by sayfasayisi asc limit 1
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

		CEVAP: select kitapadi, sayfasayisi from kitap order by sayfasayisi asc limit 1
	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

		CEVAP: select kitap.sayfasayisi as 'dramturu_max_sayfa' from kitap inner join tur on kitap.turno = tur.turno  where tur.turadi = 'Dram' order by kitap.sayfasayisi desc limit 1
	
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

		CEVAP: select sum(k.sayfasayisi) as 'okudugu_sayfasayisi' from ogrenci as o 
		inner join islem as i on o.ogrno = i.ogrno
		inner join kitap as k on i.kitapno = k.kitapno
		where o.ogrno = 15

	
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

		CEVAP: select ograd, count(ograd) as 'ogrenci_sayisi' from ogrenci group by ograd order by ograd

	
	26) Her sınıftaki öğrenci sayısını bulunuz.

		CEVAP: select sinif ,count(sinif) as 'ogrenci_sayisi' from ogrenci group by sinif order by sinif 
	
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
		CEVAP: select sinif, cinsiyet, count(cinsiyet) as 'ogrencisayisi' from ogrenci group by sinif, cinsiyet order by sinif  
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

		CEVAP:  select concat(o.ograd, o.ogrsoyad) as adi_soyadi, sum(k.sayfasayisi) as toplam_sayfasayisi from ogrenci as o 
		inner join islem as i on o.ogrno = i.ogrno
		inner join kitap as k on i.kitapno = k.kitapno 
		group by adi_soyadi
		order by toplam_sayfasayisi desc


	
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

		CEVAP: select concat(o.ograd, o.ogrsoyad) as adi_soyadi, count(o.ogrno) as toplam_kitapsayisi from ogrenci as o
		inner join islem as i on o.ogrno = i.ogrno
		group by o.ogrno
		order by toplam_kitapsayisi desc