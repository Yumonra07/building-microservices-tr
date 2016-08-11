# Testing
Testing dünyası son zamanlarda çok gelişti ve her ay yeni bir yöntem ve yeni bir tool çıkıyor. Testing yaptığımız değişiklikleri kendimize güvenerek deploy etmemiz çok önemlidir.

Testing yaparken ürünümüzü ne kadar hızlı çıkaracağımız ile testing için ne kadar zaman harcayacağımızın dengesini iyi tutturmamız gerekir.

## Types of Tests
4 Farklı test tipi olduğunu söyleyebiliriz.

Bunlar:
```
                 Business
        ############################
        # Acceptance | Exploratory # 
Support #--------------------------# Critique
        #     Unit   |  Property   #
        ############################
				Technology
```

şeklinde bir diagram ile gösterilebilir. Technology tarafındaki testler bizim yazılım yaparken rahat olmamızı sağlayan testlerken, business facing olanlar iş tarafındaki kişilere sistemin sağlamlığını daha iyi gösterilebecek tarzda testler olabilir. Technology tarafında genellikle otomatize olmuş response/performance benchmarklar ve basit functonality kontrol eden unit testler vardır.

Manuel testler yararlı olabilen şeylerdir fakat biz bu chapterda otomatik testlere odaklanacağız.

## Test Scope
Testler genelde UI, Service ve Unit diye ayrılı ve Unit teste indikce izolasyon artar ve UI'ye geçtikce testlerden elde edeceğimiz güven artar, fakat bu tarz bir tabloda UI, Service ve Unit'in kim için ne anlama geleceği çok değişir.

Helpdesk ve Webshopumuzun kullandığı bir customer servisimizin olduğunu hayal edelim. Bu customer serviside kişinin puanlarını tutmak için bir loyalty points bank ile çalışıyor...

### Unit Tests
Genelde tek bir fonksiyon veya metodu test eden testlerdir. Bu tarz testler genelde çok limitasyonla çalışır ve aşırı hızlılardır. Bu testlerden çok sayıda isteriz çünkü modern bir bilgisayar bir dakikada bunlardan binlercesini çalıştırabilir.

Bunlar programımızın functionalitysinin iyi olup olmadığını bize hızlıca veren testlerdir.

Yukarıda verdiğimiz örnekte bunlar customer servisimizin içinde çalışan testler olabilir.

### Service Tests
Servis testleri user interface'i aşıp, servisleri direk olarak test eden testlerdir. Monolithic bir applikasyonda, bir ui'a fonksiyonalite veren birden fazla classını test yapıyor veya servis bazlı bir applikasyonda servislerden birini test ediyor olabiliriz. Bu test'de servisi diğer bağımlılıklar olmadan izole olarak test etmek isteriz.

Bu tarz testlerde gerçek bir veritabanı kullanmazsanız testleriniz oldukca hızlı olabilir. Bu yüzden mocking tarzı işlemler yaparak testlerinizi unit testler kadar hızlı hale getirebilirsiniz.

### End-to-end tests
Tüm sisteminizi test eden testlerdir. Örneğin bir web applikasyonu için gui'yi headless browserda çalıştırıp, dosya upload eden bir sistem olabilir.

Bu testler doğru tutturması zor olan testlerdir. Fakat bu testleri doğru yaparsanız, testler başarılı olduğu zaman, kendinizi güvende hissedebilrisiniz.

### Trade-Offs
Testlerin hızları ve verdikleri güven konusunda trade-off iyice düşünülüp ona göre testler yazılmalıdır.

### How Many?
Unit teste doğru indikce katlanarak artan sayıda bir test sayısı istemeniz doğaldır. Bu tarz durumlarda, testlerden elde ettiğiniz yararları, testlerin aldığı zamanı düşünerek, test sayınızı ayarlayabilirsiniz.

Örneğin çok fazla end-to-end testinizin olduğu bir yapı test suiteiniz olursa, çok yavaş feedback alırsınız, eğer çok fazla unit testiniz, az end-to-end testiniz olursa testlerin verdiği güven azalır.

## Implementing Service Tests
Bir servisimizin kullandığı servisleri mocking veya stubbing yaparak, sabit değerler döndürecek şekilde konfigüre edebiliriz. Böylece örneğimizdeki customer servisi downstream servisi olan loyalty bank'dan user için bir loyalty puanı istediği zaman, loyalty bank sabit bir değer döndürebilir.

### Mocking or Stubbing
Stublarda ne kadar çağrıldığı önemsiz olarak, bir servise çağrı yapıldığı zaman, dönen değerlerin aynı olmasıdır. Örnek vermek gerekirse customer servis her call yaptığında, aynı balance döner.

Mock servisler ise biraz daha ileriye gidip, örneğin bir customer oluşturulduğu zaman bir balance'ın set edilip, edilmediğini anlamamımızı sağlayan servislerdir. 

### A Smarter Stub Servis
Servis testlerini geçen bir applikasyon direk deployable denemez. End-to-end testleri unutmayın.



