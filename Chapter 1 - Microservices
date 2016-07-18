# Microservices
Küçük, otonom çalışan servislere microservis denir. Bunları başlıklar altında incelemek gerekirse...

## Küçüktürler, ve bir şeyi *iyi* yapmaya odaklıdırlar
Codebaseimiz büyüdükce, kodda değişiklik yapmamız ve sorunları düzeltmemiz/feature eklememiz gittikce zorlaşır. OOP'un conceptlerinden biri olan Single responsibility bunu kod düzeyinde yapmaya güzel bir örnektir. Bu konsepte göre aynı sebepten dolayı değişen/çağırılan kodlar birlikte olacakken, farklı sebeplerden dolayı çalışan kodlar birbirinden ayrı olacaktır.

Ne kadar kod düzeyinde bu şekilde bir gruplandırma yapsakta, projelerimiz bir süre sonra *çok büyük* hissettirmeye başlar. Microservisler bu *çok büyük* olan projelerimizi, artık *çok büyük* hissettirmeyene kadar, birbiriyle alakalı kodları içeren küçük parçalara ayırmakla olur. Bir microservisin ne kadar küçük olacağını belirlerken amacımız microservisin getirdiği yararları maksimize ederken, getirdiği zararları minimize etmek olmalıdır.

## Otonom
Microservisler kendi başlarına çalışabilen ve kendilerini kimin consume ettiğini umursamayan yapılardır. Microservisleri son zamanlarda ortaya çıkan virtualization teknikleri ile istediğimiz sunucuya upload edebilir ve çalıştırabilir olmalıyız.

Microservisler birbirleri ile bir kanal üzerinden(örneği tcp/http) mesajlaşmalılardır.

Microservislerin dışarıya expose ettikleri API'ler, ne kadar spesifik olursa, bu bizim eski monolithic yapıdan bir farkımız kalmamasına sebep olur. O yüzden microservis kodlarken, dışarıya expose ettiğimiz apilere olabildiğince dikkat etmeliyiz. Eğer bir microserviste yaptığımız değişiklikten sonra microservisi deploy edersek ve diğer sistemlerin çalışması olumsuz yönde etkilenirse, bu bizim servis için yaptığımız apinin çokda iyi olmadığı anlamına gelir.

## Microservis yararları
Microservislerin distributed olmasından kaynaklanan bir çok yararı vardır. Bu yararlardan bazılarını listelemek gerekirse...

### Technology Heterogeneity
Servislerimizin expose ettikleri api ve birbirleri ile etkileşimlerinin bu apilere göre network üzerinden olmasından dolayı; farklı microservislerde farklı dil ve teknolojiler kullanabiliriz. Örneğin user ilişkilerini tuttuğumuz bir microserviste, graph database kullanırken key/value değer tutmamız gereken bir yerde başka bir veritabanı kullanabiliriz. Aynı zamanda microservisler için kabul edilebilri küçüklük genelde 2 haftada yeniden yazılabiliteleri olduğu için, yeni teknolojileri denemek bizim için dahada kolaylaşıyor.

### Resilience
Monolitik yapıdaki uygulamalar failure durumunda sistemin çalışma ihtimalini riske attığı halde, birden fazla instancelarını çalıştırarak bu failure'ın işleyişimizi etkilemesini engelleyebiliriz. Fakat microservis yapısına geçtiğimiz zaman, microservislerin çoğu çalışmayı durdursa bile çalışabilen bir sistem yapmamız söz konusu olabilir. Aynı zamanda bu microservislerin izole çalışmasından dolayı yeniden başlatmak veya birden fazla çalıştırma olaylarıda daha kolay olur.

### Scaling
İzole çalışan microservislerden ihtiyacımız olanları ihtiyacımız olduğu kadar, farklı makinelerde çalıştırabiliriz. Bu yüzden microservis yazarken deployment aşamasında sistem agnostik olmamıza yardımcı olacak teknolojiler kullanmak(docker) bize yardımcı olabilir.

### Ease of Deployment
Sisteminizin küçük parçalarını deploy etme şansı, sizin sık sık deployment yapıp, deployment aşamasında daha az sorunla karşılaşmanıza sebep olabilir.

### Composability
Son zamanlarda client tarafında artan farklı platformlar, bizim artık sadace bir platform için değil, birden fazla platformu düşünerek sistem tasarlamamıza sebep oluyor. Artık sadece pc, mobil değil... native application, android, ios, tablet, wearable tüm client taraflarını düşünmemiz gerekiyor. Microservislerin yeniden kullanılabilir olması bu aşamada da çok işimize yarıyor.

### Optimizing of Replaceability
Microservislerimizin küçük boyutu, onları projenin devamında silip/değiştirmemizin kolaylaşmasını sağlıyor. Çoğu şirketde 10 yıl önceden kalmış, eski teknoloji ile yazılmış büyük sistemler olabilir. Microservis yapısı, bununda önüne geçebilir.

## Monolitik programları parçalamanın diğer yolları
Yıllardır süregelen monolitik uygulamaları parçalama çabalarından bazılarıda aşağıda yazıldığı gibidir...

### Shared Libraries
Kod bazında decoupling yapmak için kullanılan yöntemlerden biridir. Bir parça kodu statik veya shared olarak projelerinizde kullanmanıza yardımcı olur. Fakat yazılan kütüphaneler genelde belli bir dilde olduğu için, microservis yapımızı engelleyebilir. Aynı zamanda dynamically linked değil, statically linked kütüphanelerde build aşamasında sıkıntı olabilir. 

### Modules

