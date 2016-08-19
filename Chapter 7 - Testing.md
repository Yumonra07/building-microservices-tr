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


## Those Tricky End-to-End Tests
End-to-end testleri microservis yapımızda, birden fazla servisimizi deploy edip, onları ortak bir aracı ile birleştirmemiz gerekir. Ancak bu şekilde bir end-to-end test yapmış ve güven kazanmış olabiliriz.

Burada en büyük problemlerden biri test ederken seçeceğimiz servis versiyonları olabilir. Ya sürekli güncellenen servislerimiz varsa, bunların birbiri ile çalıştığından nasıl emin olacağız?

Test edilmek istenen birden fazla servis varsa ne yapacağız?

Bunun bir çözümü çoğu CI'ın default olarak desteklediği fan-in modeli olabilir. Bu modelde bir servisimizde değişiklik olduğu zaman local testler çalışır, ondan sonra diğer servislerle integrasyonu test edilmek için bir end-to-end test başlar.

## Downsides of End-to-End Testing
End-to-End testingin birden fazla kötü yanı vardır.

### Flaky and Brittle Tests
Bir testin scope'u arttıkca, hareket eden parçalarda artar. Microservislerle düşündüğümüz zaman, bir cd satın alabileceğimizi onaylayan bir test düşünelim, böyle bir testi çalıştırmak için birden fazla servise ihtiyacımız olabilir. Böyle bir durumda testin fail olmasına sebep olan şey, yaptığımız kod değişillikleri değil bir network glitchi, bir servisin avaliable olmaması gibi durumlar olabilir. 

Ne kadar fazla hareket eden parçamız olursa, o kadar kırılgan testlerimiz olur. Ne kadar kırılgan olurlarsada o kadar az deterministic olurlar. Eğer testlerinizi acaba bu sefer geçer mi diye, kod değiştirmedne yeniden çalıştırıyorsanız, bu kırılgan testlere sahip olduğunuz anlamına gelir. Eğer testlerimizi bu kırılganlıktan arındırmazsak, "normalization of deviance" tuzağına düşmüş oluruz. Bu yanlış bir şeye alışınca artık normal gelmesi durumudur. Bu bizim testler sırasına "aah o her zaman öyle fail olur" gibi şeyler dememiz sebep olabilir.

"Eradication Non-Determinism in Tests" makalesinde `Martin Fowler` testlerinizin flaky olan kısımlarını bulduktan sonra, kodunuzda bu testleri daha kolay çalışacak hale getirecek değişiklikler yapacak kadar ileriye gitmenizi bile öneriyor.

#### Who Writes These Tests?
Microservis yapımızda takımlar tarafından sahiplik konusunu daha önce konuşmuştuk. Bu tarz bir durumda testleri kim yazacak? Bu konuda bir çok anti pattern oluştuğunu görmüş bulunmaktayım. Örnek vermek gerekirse, testlerin free-for-all olduğu bir şirkette, testler bozulduğu zaman herkes diğer takımı testlerin sorumluluğu ile suçlanabilir.

Bu tarz bir durumda testler bozuk olsa bile, kimse düzeltmekle uğraşmayabilir.

Başka bir anti-patternde testleri yazan kişilerin servisi yazan kişilerden tamamen farklı olduğu durumlardır, böyle durumlarda servisleri yazan kişiler yeni bir şeyler yapmadan önce servisler için test yazılmasını beklemek zorunda kalabilir. Testleri yazan kişiler aynı zamanda servis ile alakalı kişiler olmadığı için, bu kişilerin test yazmak için servis hakkında yeterince bilgiye sahip olmak için çalışması gerekebilir. Aynı durumda servisi yazan kişilerde testleri yazmadığı için, testleri nasıl çalıştırmaları gerektiğini bilmeyebilirler.

Bu işi doğru yapmak oldukca zordur. Benim en işe yarar senaryo olarak gördüğüm senaryo, servislerin joint bir responsibilityde olduğu senaryolardır. Tüm takımlar kodu editleyebilir, fakat test içindeki servislerin sahipleri bu testlerden sorumlulardır.

#### How Long?
End-to-end testler biraz zaman alabilir. Bir gün süren testler gördüm. Hatta bir projemde full regression suite 6 hafta sürüyordu. Takımların testlerini daha hızlı yapmak için çalıştığını çok az görüyorum.

Bu testlerin flaky olabileceklerinide unutmayalım. Eğer 1 gün süren bir testinizin, yaptığınız değişikliklerle alakalı olmayan bir sebepten dolayı bozulma ihtimali varsa, bu test çokta işe yarar bir test değil anlamına gelir. Siz yaptığınız değişiklikteki hatayı bulana kadar, çoktan başka bir şeyle uğraşmaya başlamış olursunuz, ve zaman kaybedersiniz.

Bunu biraz olsun hızlandırmak için Selenium Grid gibi paralel test çalıştırabilen bir sistem kullanabiliriz. Fakat esas öncelik işimize yaramayan testleri silmek(overlap of test coverage), ve gerçekten neyin test edilmesi gerektiğini anlamaktır.

Test silmek tehlikeli bir iştir. Kimse size test sildiğiniz için teşekkür etmez, fakat eğer sildiğiniz bir test bir bugın yayına gitmesine izin verirse, o zaman azarlanabilirsiniz. O yüzden bu aşamada testleri ve risklerini iyi anlamak lazım.

#### The Great Pile-up
End-to-end testlerimiz fail olurken, prodüksiyona çıkamayan servisler, gittikce artabilir. Bu tarz bir durumda testlerimizin hızlı olması ve küçük değişiklikleri check-in yapmamız gerektiğini hatırlamamız lazım. Aksi takdirde end-to-end testlerimizi geçemeyen çok sayıda servisimiz olabilir.

#### The Metaversion
End-to-end testleri geçen servislerimiz için, ahh şu servislerin şu versiyonları birbirleriyle çalışıyor diyip, bu test suitelerine ortak bir versiyon vermek normal gelebilir.

Fakat böyle bir durumda servislerin birbirleri ile birlikte check-in yapılması ve deploy edilmesini norm haline getirmiş oluruz. Böyle bir durumda özerk ve kendi başlarına deploy edilebilen servislerimiz, hızlıca birbirlerine bağlı, bir hale gelebilir.

### Test Journeys, Not Stories
3-4 tane servisimizin olduğu bir sistemde end-to-end testler yazmak oldukca kolaydır. Ama servis sayısı arttıkca ve yeni özellik ekledikce, her özelliğimiz için bir end-to-end test yazmak istersek, bloated bir test suiteimiz olabilir.

Bu yüzden her özellik için değil, komplex sistemlerde bile düşük çift rakamlı sayıları aşmıcak sayıda end-to-end testimiz olamlıdır. Örneğin MusiCorp için cd satın alma, product return etme, yeni bir kullanıcı oluşturma gibi durumlar olabilir.

Bu journeylerde test edilmeyen durumlar için servislerin kendi izole testlerinde test yazmalıyız.

## Consumer Driven Tests to the Rescue
Integration test yaparken yapmaya çalıştığımız şey, servislerimizi deploy ettiğimiz zaman consumerları bozmayacağımızdır. Bunu izole olarak yapabileceğimiz yöntemlerden biri consumer-driven contract(CDC) dediğimiz, consumerlarımızın servislerimiz üzerinde olan beklentilerini code formuna döktüğümüz yöntemdir.

Eğer servisimiz belli consumerların beklentilerini yerine getiremiyorsa, bunun prodüksiyonda sorun oluşturmasını bekleyebiliriz. CI aşamasında bu CDCleri kontrol ederek end-to-end testlere gerek kalmadan, testleri sonlandırıp, düzeltmeler yaparak veya daha önce konuştuğumuz şekilde bir breaking change introduce ederek, servisi tekrar teste sokabilir veya deploymenta koyabiliriz.

### Pact

