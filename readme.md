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
	
SELECT ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih from ogrenci,islem 
WHERE ogrenci.ogrno=islem.ogrno
ORDER By ogrenci.ograd;

joinli;

select ograd,ogrsoyad,atarih from ogrenci
join islem on(islem.ogrno=ogrenci.ogrno)
order by ograd;

	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	1-)select kitapadi,turadi from kitap,tur 
	where kitap.turno=tur.turno and turadi in ("Hikaye","Fıkra")

	2-)select kitapadi,turadi from kitap,tur 
	where kitap.turno = tur.turno and turadi="Hikaye" and turadi="Fıkra"

	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

select ograd,ogrsoyad,ogrenci.ogrno,sinif, kitapadi from ogrenci,islem,kitap 
where sinif in("10B","10C") and ogrenci.ogrno=islem.ogrno and kitap.kitapno=islem.kitapno;

   joinli;

select ogrno,ograd,ogrsoyad,sinif,kitapadi from ogrenci
join islem on(islem.ogrno=ogrenci.ogrno)
join kitap on(kitap.kitapno=islem.kitapno)
where sinif in ("10B", "10C");

	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

	select ograd,ogrsoyad,atarih from ogrenci 
	INNER JOIN islem ON islem.ogrno=ogrenci.ogrno ;

	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	select kitap.kitapadi,tur.turadi from kitap 
	join tur on kitap.turno=tur.turno 
	where turadi="Hikaye" or turadi="Fıkra"
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
select ogrenci.ogrno, ograd, ogrsoyad,sinif,kitapadi from ogrenci
 inner join islem on ogrenci.ogrno=islem.ogrno 
 inner join kitap on islem.kitapno=kitap.kitapno 
 where sinif in ("10B", "10C") 
 order by ogrenci.ograd
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
	select ogrenci.ograd,ogrenci.ogrsoyad,islem.atarih from ogrenci 
  left join islem on islem.ogrno=ogrenci.ogrno;

	
	8) Kitap almayan öğrencileri listeleyin.

	select ograd, ogrsoyad, atarih from ogrenci 
	left join islem on ogrenci.ogrno=islem.ogrno
	 where atarih is null

	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

!!!group by ı silince 1 adet getiriyor.!!!

	1-)SELECT islem.kitapno,kitapadi,COUNT(*) from islem 
	JOIN kitap ON islem.kitapno = kitap.kitapno 
	group by islem.kitapno 
	order by islem.kitapno

	2-)select kitapno,kitapadi,count(islemno) from islem
join kitap on(islem.kitapno=kitap.kitapno)
group by kitap.kitapno
order by islem.kitapno;

derste yapılan ==> select kitapno,kitapadi,COUNT(islem.kitapno) from kitap
JOIN islem ON islem.kitapno=kitap.kitapno 
GROUP BY kitap.kitapno;

10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

	Select kitapno, COUNT(islemno), kitapadi FROM kitap 
	LEFT JOIN islem ON kitap.kitapno = islem.kitapno // listede alınmayanlar da görünecei için left join//
	GROUP BY kitapadi 
	ORDER BY kitap.kitapno;
	
11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

	select ograd,ogrsoyad,kitapadi from ogrenci
	join islem on ogrenci.ogrno=islem.ogrno
	join kitap on islem.kitapno=kitap.kitapno

12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

	select ograd, ogrsoyad, kitapadi, yazarad, yazarsoyad, tur.turadi, islem.atarih from ogrenci
	left join islem on ogrenci.ogrno = islem.ogrno
	left join kitap on islem.kitapno = kitap.kitapno
	left join yazar on kitap.yazarno = yazar.yazarno
	left join tur on kitap.turno= tur.turno

13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

select ograd,ogrsoyad,sinif,count(islemno) from ogrenci
join islem on(islem.ogrno=ogrenci.ogrno)
where sinif in("10A","10B")
group by ogrenci.ograd;

14) Tüm kitapların ortalama sayfa sayısını bulunuz.
#AVG

	select avg(sayfasayisi) from kitap

15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

	select kitapadi,sayfasayisi from kitap 
	where sayfasayisi > (select avg(sayfasayisi) from kitap)

16) Öğrenci tablosundaki öğrenci sayısını gösterin

	select count(ogrno) from ogrenci

17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

	select count(ogrno) as toplamSayi from ogrenci

18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

	select count(distinct ograd) from ogrenci;

19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

	select max(sayfasayisi) from kitap

20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

	select kitapadi,sayfasayisi from kitap 
	where sayfasayisi=(select max(sayfasayisi) from kitap)

21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

	select min(sayfasayisi) from kitap

22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

	select kitapadi,sayfasayisi from kitap 
	where sayfasayisi=(select min(sayfasayisi) from kitap)

23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

	select max(sayfasayisi),turadi from kitap,tur 
	where kitap.turno = tur.turno and turadi="Dram"

	select max(sayfasayisi) from kitap 
	inner join tur on kitap.turno= tur.turno 
	where tur.turadi= "Dram"
 24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

	select sum(sayfasayisi) from ogrenci,islem,kitap
	where ogrenci.ogrno=islem.ogrno
	and islem.kitapno=kitap.kitapno
	and ogrenci.ogrno=15

	--joinli
	select sum(sayfasayisi), ogrenci.ogrno from ogrenci
	join islem on ogrenci.ogrno=islem.ogrno
	join kitap on kitap.kitapno=islem.kitapno
	where ogrenci.ogrno=15

25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	select ograd, count(ograd) from ogrenci
	 group by ograd

26) Her sınıftaki öğrenci sayısını bulunuz.

	select sinif, count(*) from ogrenci 
	group by sinif

27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

	select sinif, cinsiyet, count(cinsiyet) from ogrenci 
	group by sinif,cinsiyet

28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

  !!! left join dersek null olanlar da geldi.!!!
  !!! group by order by dan önce kullanılır.en son order by kullanılır.!!!

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
	select ograd,ogrsoyad, count(islemno) from ogrenci 
	INNER JOIN islem ON(islem.ogrno=ogrenci.ogrno) 
	GROUP BY ogrenci.ogrno;
