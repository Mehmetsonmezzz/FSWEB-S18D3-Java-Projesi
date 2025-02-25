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
 
	SELECT o.ograd,o.ogrsoyad,i.atarih FROM ogrenci as o , islem as i
	WHERE o.ogrno=i.ogrno
 	ORDER BY o.ograd,o.ogrsoyad asc;
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
 
	SELECT k.kitapadi,t.turadi FROM kitap as k, tur as t
	WHERE k.turno=t.turno
 	AND t.turadi IN('Fıkra','Hikaye')
  	ORDER BY t.turadi,k.kitapadi
   
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
 
	SELECT o.ogrno, o.ograd, o.ogrsoyad,k.kitapadi 
 	FROM ogrenci as o,kitap as k, islem as i
 	WHERE o.ogrno=i.ogrno
  	AND i.kitapno=k.kitapno
   	AND o.sinif IN('10C','10B')
    	ORDER BY o.ogrno, o.ograd, o.ogrsoyad,k.kitapadi
     
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
 
	SELECT o.ograd,o.ogrsoyad,i.atarih 
 	FROM ogrenci as o
	INNER JOİN islem as i
 	ON o.ogrno=i.ogrno
  	ORDER BY o.ograd
   
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	    SELECT k.kitapadi, t.turadi 
        FROM kitap AS k INNER JOIN tur as t
 	ON k.turno=t.turno
 	AND t.turadi IN ('Fıkra','Hikaye')
  	ORDER BY t.turadi,k.kitapadi
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	 SELECT o.ogrno, o.ograd, o.ogrsoyad,k.kitapadi 
 	FROM ogrenci as o,kitap as k, islem as i
 	WHERE o.ogrno=i.ogrno
  	AND i.kitapno=k.kitapno
   	AND o.sinif IN('10C','10B')
    	ORDER BY o.ogrno, o.ograd, o.ogrsoyad,k.kitapadi
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	SELECT o.ograd,o.ogrsoyad,i.atarih 
 	FROM ogrenci as o
	LEFT JOIN islem as i
 	ON o.ogrno=i.ogrno
  	ORDER BY o.ograd
	
	8) Kitap almayan öğrencileri listeleyin.
	SELECT o.ograd,o.ogrsoyad,i.atarih 
 	FROM ogrenci as o
	LEFT JOIN islem as i
 	ON o.ogrno=i.ogrno
        AND i.atarih IN(null)
  	ORDER BY o.ograd
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	SELECT i.kitapno,k.kitapadi,
COUNT(i.kitapno) as TOTAL 
 	FROM islem as i
	INNER JOIN kitap as k
 	ON i.kitapno=k.kitapno
        GROUP BY k.kitapno,k.kitapadi
  	ORDER BY k.kitapno ASC
      
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

	SELECT i.kitapno,k.kitapadi,
COUNT(i.kitapno) as TOTAL 
 	FROM kitap as k
	LEFT JOIN islem as i 
 	ON k.kitapno=i.kitapno
        GROUP BY k.kitapno,k.kitapadi
  	ORDER BY TOTAL asc
   
	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	 SELECT o.ograd,o.ogrsoyad,k.kitapadi
FROM ogrenci as o
INNER JOIN islem as i 
ON o.ogrno=i.ogrno
INNER JOIN kitap as k
ON i.kitapno=k.kitapno
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
	 SELECT o.ograd,o.ogrsoyad,k.kitapadi,t.turadi,y.yazarad
FROM ogrenci as o
LEFT JOIN islem as i 
ON o.ogrno=i.ogrno
LEFT JOIN kitap as k
ON i.kitapno=k.kitapno
LEFT JOIN tur t
ON t.turno =k.turno
LEFT JOIN yazar y
ON y.yazarno=k.yazarno
ORDER BY o.ograd DESC
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

 	 SELECT o.ograd,o.ogrsoyad,COUNT(i.atarih) as TOPLAM FROM ogrenci as o
  	INNER JOIN  islem as i
   	ON o.ogrno=i.ogrno
    	WHERE o.sinif IN('10A','10C')
        GROUP BY o.ograd,o.ogrsoyad
	
 
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
	



        SELECT AVG(k.sayfasayisi)
 	FROM kitap as k

 
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

        SELECT * FROM kitap as k
	WHERE k.sayfasayisi>(SELECT AVG(k.sayfasayisi)
        FROM kitap as k)




 
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin


 	 SELECT  COUNT(*) FROM ogrenci as o
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
         SELECT  COUNT(*) AS Toplam  FROM ogrenci 

 
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
        SELECT count(distinct ograd) as toplam from ogrenci
 
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

             ÇÖZÜM 1 -SELECT * FROM kitap
	     ORDER BY sayfasayisi DESC LIMIT 1
             ÇÖZÜM 2 SELECT MAX(sayfasayisi) FROM kitap
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.


             -SELECT ad,sayfasayisi FROM kitap
	     ORDER BY sayfasayisi DESC LIMIT 1
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

               SELECT MIN(sayfasayisi) FROM kitap
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
            SELECT kitapadi,sayfasayisi FROM kitap
	     ORDER BY sayfasayisi ASC LIMIT 1
  
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

         SELECT MAX(k.sayfasayisi) FROM kitap k
	    INNER JOIN tur as t
             ON k.turno=t.turno
             AND turadi IN('Dram')
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	SELECT * FROM ogrenci as o
	 INNER JOIN islem as i
  	  ON o.ogrno=i.ogrno
    	INNER JOIN kitap as k
	ON i.kitapno = k.kitapno
	WHERE o.ogrno=15
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

          SELECT o.ograd, COUNT(o.ograd) from ogrenci o
	  GROUP BY o.ograd

	
	26) Her sınıftaki öğrenci sayısını bulunuz.

            SELECT o.cinsiyet, COUNT(o.ograd) from ogrenci o
	  GROUP BY o.cinsiyet
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

             SELECT o.ograd, COUNT(o.ograd) from ogrenci o
	  GROUP BY o.ograd
      
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

         SELECT o.ograd, o.ogrsoyad SUM(k.sayfasayisi) as Toplam
	 FROM ogrenci as o
         INNER JOIN islem i
	ON o.ogrno=i.ogrno
         INNER JOIN kitap k
	 ON i.kitapno=k.kitapno
         GROUP BY o.ograd,o.ogrsoyad
	 ORDER BY Toplam desc
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

        SELECT o.ograd, o.ogrsoyad COUNT(k.kitapno) as Toplam
	 FROM ogrenci as o
         INNER JOIN islem i
	ON o.ogrno=i.ogrno
         INNER JOIN kitap k
	 ON i.kitapno=k.kitapno
         GROUP BY o.ograd,o.ogrsoyad
	 ORDER BY Toplam desc






 
