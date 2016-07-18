# Splitting the Monolith
İyi servislerin nasıl göründüğünü, küçük sunucuların bizim için neden daha iyi olabileceğini konuştuk. Ayrıca sistemimizin designını evrimleştirme kabiliyetinin önemindende bahsetmiştik. Ama etrafta yatan, bu kurallara uymayan sistemlerimiz hakkında ne yapacağız? Bu monolitik uygulamalarımızı, büyük bir re-write gerektirmeden nasıl decompose edeceğiz?

Monolitik zaman geçtikce büyür. Hızla fonksiyonalite ve line sayısı artar. Bir süre sonra büyük, dev bir yapı haline gelir. Ama ümitsizleşmeyin! Doğru aletler ile bu yaratığı parçalayabiliriz!

# Its all about seams
Chapter 3'de microservislerimizin birbiriyle çok alakalı kodlar içermesi ve alakasız kodlardan ayrı olması gerektiğinden konuşmuştuk. Monolitik bunun tam tersi bir yapı içindedir.

Bu sorunu çözmek için ilk önce bir değişiklik yaptığımız zaman sistemin diğerini değilde, sadece o değişikliği yaptığımız yerdeki işleyişi değiştireceğimiz kodları bulmamız lazım. Bu parçalara düğüm diyelim!

# Musicorp parçalamak
Bir monolitik yapıyı parçalarken, ilk önce business kominikasyonumuzu düşünüp, ondan sonra kodumuzu kafamızda oluşturduğumuz parçalara ayırmaya başlamalıyız. Bir süre sonra etrafta artık kodlar kalıcak. Bu hiçbiryere sığmayan artık kodlar, gözden kaçırdığımız bir ayırım olduğu anlamına gelebilir.

Monolitik bir yapıyı parçalarken, testlere ihtiyacınız olucak. Özellikle dinamik bir dil kullanıyorsanız, kodları değiştirip, etrafta oynatırken, testleri çalıştırmak çok işinize yarayacak.

Bir projeyi parçalamak bir öğlende alabilir, bir kaç hafta veya ayda. Eforunuzu bir parça düğüme odaklamak, tüm projeyi yeniden yaratmaktan daha mantıklı olabilir. O yüzden take your time with it ve gün geçtikce yavaş yavaş parçala.

# Monoliti parçalamak için sebepler
Monolitik bir yapıyı parçalarken, tüm sistemi yeniden tasarlamak yerine, yavaş yavaş parçalama işlemi yapabilirsiniz.

Ama ondan önce codebaseinizi sadece parçalamış olmak için değil, gerçekten kodunuzu ayrımsallaştırmanın getireceği faydalardan dolayı ayırın. Bu faydalardan bazıları, şunlar olabilir.

## Pace of change
Örneğin inventoryimizi nasıl manage ettiğimizi bir yenilik gelicekse, bu kod parçasını bir servis haline getirip, daha rahat bir şekilde değişiklikleri uygulayabiliriz.

## Takım yapısı
İki farklı yerde çalışan takımlar için, sorumluluğu bölmek amacıyla yapabiliriz.

## Güvenlik
Finans ağırlıklı bir uygulamada, veri güvenliği ihtiyacımız varsa, veri saklama ve taşıma işini bir microservise bırakıp, ekstra güvenlik oluşturacak işlemler yapabiliriz.

## Teknoloji
Sisteminizin bir parçasını farklı bir dil veya teknoloji ile re-implemente etmek işinize yarayacak olabilir.

# Tangled Dependencies
Sisteme en karışmış ve bağımlı olan modüller yerine, daha bağımsız olan modüllerden başlamak isteriz. Eğer farklı kod veya modüllerin, sisteme olan bağımlılığını görebileceğimiz bir tool varsa, bunu kullanın.

Sistemin en bağımlı(kendi içinde ve dışında), karışık kod parçacığı, veritabanıdır!

# Veritabanı
Veritabanlarımızdada kodumuzda olduğu gibi düğümleri bulup, sökmeliyiz.

# Problemi ele almak
Veritabanlarında genelede sınırlı bağlamlar için ayrı ayrı kullanılan değil, genel bir domain olarak kodlanmış, birbirine bağımlı bir veritabanı olur. Örneğin hibernate kullanılan bir sistem düşünün. Projenizde veritabanı ile ilgili modelleri ayırıp, package olarak tutuyorsunuz. Bu tarz işlemlerde veritabanınızın tabloları arasındaki ilişikileri görselleştiren bir tool işinize yarayacaktır.

Bu bağlantıları nasıl keseceğiz? Oldukca zor bir işlem ama yapılabilir. Bu işlemi yapmak için izleyebileceğiniz bir kaç yöntem ve örnek vereceğim.

## Breaking foreign key relationships
Örneğin katalogdaki itemler için yapılan satışları tutan bir finance tablomuz olsun. Bu finans tablosunda, satılan her katalog itemi için bir kayıt tutulsun ve satılan item'a referans bir id ile yapılsın. Böyle bir durumda, hangi itemın ne kadar satıldığını, ve itemin adını çekmek istersek, bir join durumu söz konusu olur.

Bu join durumunu bozmak için yapmamız gereken şey, katalog servisinde idlere, itemin adı ile cevap veren bir api call yapmaktır. Bu durumda artık hangi itemin ne kadar satıldığını almak için 2 tane sorgu yapmamız gerekecek fakat bu performans kaybı, bize bir şeyler kazandırabilir.

## Shared Static Data
Birden fazla servisimiz(ve database tablosu) arasında paylaşılan (örneğin country code) gibi bir tablo olması durumunda, bu tabloyu parçalamak için 3 farklı yöntem uygulayabiliriz.

1-) Tablonun servis sayımız kadar kopyasını oluşturup, servislerimiz için ayrı ayrı tutabiliriz. Bu durumda tabloların consistency issueları olabilir.
2-) Static data'yı veritabanında bir tablo değilde, kod paketi olarak paylaşılan bir veri haline getirmek.
3-) Static data'yı servis eden bir microservis oluşturmak.(country code örneği için overkill)

## Shared Data
Örneğin warehouse ve finans tarafından paylaşılan, mutable bir customer tablosu olduğunu düşünün. Bu genellikle bir domain modellemeyi gözden kaçırdığımız anlamına gelir. Bundan kurtulmanın en güzel yollarından biri, customer tablosunu bir servis haline getirmektir. Tabi basit bir crud değil, akıllı bir servis!

## Shared Tables
Örneğin bir tablo tarafından catalog id - name translationı, bir tablo tarafından stok takipi için kullanılan tek bir catalog tablosu olduğunu düşünün. Bu tabloyu ilk tasarlarken gözümüzden kaçmış olabilir ama bunlar aslında çok farklı iki tablo olabilir.

Bu yüzden bu tabloları bölüp, ayrı tutmak en mantıklı seçeneklerimizden biridir.

# Refactoring Databases
Burada sadece database şemalarınızı refactor etmek için bir kaç örnek verilmiştir. Daha detaylı bir kaynak için `Refactoring Databases by Scott J. Ambler` kitabına bakabilirsiniz.

# Staging the Break
Şemalarımızı ve bounded contextlerimizi belirledikten sonra, kocaman bir yeni release yapmak yerine, veritabanını parçalayıp, eski kodu kullanarak, bir sql query yerine birden fazla sql query yapıp, in memory join yaparak çalışabiliriz.

Veritabanı parçalamamızdan memnun olduktan sonra, yeni microservis yapısına geçebiliriz. Bir sorgu yerine 2 sorgu yapmak transactional safety gibi konularda sıkıntı olabilir, bir sonraki bölümlerde bunu tartışacağız.

# Transactional Boundaries
Standart veritabanlarında bir veya birden fazla tablolarda yaptığımız create/update işlemlerini transactionlar olarak yapabiliriz. Transactionlar sayesinde eğer işlemin bir kısmında bir hata ile karşılaşırsak, transaction işlemi içindeki tüm işlemler yapılmadan önceki sistem durumuna `rollback` yapabiliriz.

Microservis yapısına geçtiğimizde, bu tabloları birden fazla servise ayırdığımızı düşünürsek, bu veritabanı queryleri ile gelen basit transaction yararlarını kaybetmiş oluruz.

Örnek vermek gerekirse, 2 tabloya insert yaparken 1. tabloya insert okay, 2. tabloya insert fail olursa nasıl bir senaryo izleyeceğiz?

## Try again later
Sistemin eninde sonunda consistent bir hale ulaşacağı bir senaryo oluşturmak için, fail olan queryimizi bir log dosyasında veya queue'da saklayıp, bu işlemi daha sonra tekrar deneyebiliriz.

Bu tarz bir senaryo, eventually consistent olarak adlandırılır ve scaleable sistemlerde güzelce kullanılabilir.

## Abort the entire operation
n + 1'inci işlemimiz fail olduktan sonra, daha önce yapılan işlemleri compensate etmek için, örnek vermek gerekirse DELETE işlemi yapılacak bir query vs. atılabilir. Fakat bu compensate işlemininde fail olması mümkündür. Bu tarz bir durumun oluşması durumunda bu inconsistencyi not alıp, bir admin panelden düzeltilebilecek veya bir backend process tarafından temizlenecek hale getirebiliriz.

## Distributed Transactions
Distributed transactionların en büyük örneklerinden biri 2pc'dir bu algoritmada coordinater cohortlardan ilk önce 1 voting phasede commit işlemini yapıp yapamayacaklarını alır, daha sonra eğer tüm cohortlar evet diye cevap verirse, işlemin yapılmasını söyler.

Bu algoritma foolproof değildir. Eğer bir coordinater, bir cohort'a işlemi yapmasını söylerse, bu işlemin kesinlikle yapılacağını varsaymalıdır.

## What do we do?
Tüm bu çözümler bir complexity ekler. O yüzden şuanda transaction ile yaptığınız işlemleri, mümkünse hiç ayırmamaya çalışın. Eğer illa ayırmanız gerekiyorsa, kod tarafında değilde kafanızda işlemsel olarak bir transaction haline getirmeye çalışın. Örneğin in-process order gibi.

Eventual consistency gerektiren veya transaction gerektirmeyen sistemler, scale etmesi en kolay sistemlerdir.

# Reporting***
Microservices yapısına geçmek gibi radikal bir karar verirken, eski processlerimizle nasıl çalışacağımızıda düşünmemiz gerekebilir.

Microservis yapısından etkilenebilecek en büyük processlerimizinden biri reportingdir. Verilerimizi dağınık halde tutmaya başlayacağımız için, reporting sıkıntılı olabilir. O yüzden ilk önce eski sistem ile nasıl çalışabilirim diye düşünün.

# The Reporting Database
Monolitik sistemlerde reporting işlemi genelde tek bir veri kaynağımız, monolitik bir veritabanımız olduğu için, sql ile join yaparak anlamlı bir output çıkarmak mümkün olur. Genelde monolitik servislerde logging işlemi, ana sistemde load yaşamamak için, read replica bir database üzerinde bir logging processi ile yapılır. Fakat bu durumda 2 sorun ortaya çıkar.

a-) Logging process replica database kullandığı için, business logic ile aynı veritabanı şemasını kullanmak zorunda kalabilir. Buda şema üzerinde yaptığımız değişiklikleri iyi manage etmemiz anlamına gelir.
b-) Veritabanında yapacağımız değişiklikleri reporting mi yoksa live systemi mi performanslı hale getirecek şekilde yapacağımız konusunda kararsızlığa düşeriz.

Ayrıca son zamanlarda database teknolojilerinin patlaması sonucu, birden fazla seçeneğimiz oldu. Ya logging işlemi için performanslı bir sistem yapmanız için Neo4J veya key-value bir storage kullanmanız daha mantıklı olursa? O zaman ne yapacaksınız?

## Data Retrieval via Service Calls
Son 15 dakikada giriş yapan kullanıcılar gibi basit reporting için, service callar yaparak reporting verisi toplamak mümkün olabilir ama, service call yaparak veri toplamanın birden fazla sıkıntısı vardır.

Örnek vermek gerekirse ya large volume bir veri çekmemiz gerekirse veya tasarladığımız servislerin logging için işimize yarayacak methodları yoksa?
Toplu veri çekmek için endpointlerimiz yoksa bu işlemler için endpoint/microservis yaratık shared bir locationa batch veri çekme işlemi yapabiliriz ama bence bunlar gereksizdir.

## Data Pumps
Bu işleme bir çözüm, reporting system'in data pullaması yerine, diğer microservislerin verilerine erişimi olan sistemlerin, reporting toollarımızın kullandığı bir databasee veri pushlaması olabilir.
Daha önceden shared bir veritabanı kullanmanın zararlarından bahsetmiştik fakat, doğru kullanıldığı zaman bu anlamda bir reporting shared veritabanı, bu olaya güzel bir exception olabilir.

Örneğin periyodik olarak log veritabanına data pumplayan bir microservis data pumperı, microservisi manage eden takım tarafından yazılabilir ve essentially sadece servis verisini log verisine map etme işlemi yapar. Bu periyodik işlem cron ile bile çalıştırılabilir. Ve microservis ile aynı version control içinde bulunup, deploy edilir. Böylece reporting veritabanının o microservis için spesifik veri şemasını, sadece o servise expose etmiş oluruz.

## Event Data Pump
Daha önceden bahsettiğimiz kareografi tarzı sistemlerde, yapılan işlemler sonra event emit etmekten ve bunları dinlemekten bahsetmiştik. Bu tarz bir yapıdaysak, belli eventleri dinleyip, data pumplayan pumperlar yapabiliriz. Bu data pumperlar yine microservisi kodlayan kişiler tarafından manage edilebilir.

Aynı zamanda data pumperlar change eventlerinide listen edip, sadece delta push ederek, reporting veritabanını güncel tutabilir.

Bu yöntem büyük veritabanları için kullanışsız olabilir ve aynı zamanda güzel çalışması için gereksiz yere event emit etmeniz gerekebilir. Ama aklınızda bulunsun!

## Backup Data Pump
Netflixin cassandra üzerine kurduğu Aegisthus projesini örnek verebiliriz. Bu proje sayesinde cassandra üzerinde yapılan backupları, storage databaseinize mapleyebilirsiniz.

## Toward Real Time
Daha önce bahsedilen yöntemlerin hepsi birden fazla yerdeki datayı, tek bir yöne yönlendirmek ve reporting işlemini burada yapmakla ilgiliydi.

Ama buna gerçekten ihtiyacımız var mı? Birden fazla değişen reporting ihtiyacımız ve bu reportingleri yapacağımız yerler olabilir. O yüzden son zamanlarda bu tarz işler gittikce event based ve real time oluyor.

# Cost of Change
Microservisleri tasarlarken mutlaka hata yapacağız. Bu yüzden yapacağınız hataların low impact olmasına özen gösterin. Örneğin bir veritabanını yeniden tasarlamak veya birden fazla consumer tarafından kullanılan bir microservisin patternlerinde değişiklik yapmak, zor şeyler olabilecği gibi, codebase arasında kod değiştirme kolay bir hata olabilir. O düşünürken zamanınızı cost of change'in impactinin büyük olacağı yerlerde harcayın.

Bunu yaparken örneğin OOP'da kullanılan CRC gibi, servislerinizin sorumluluklarını, kimlerle iletişim halinde olduklarını belirleyip, tasarımınızı bir beyaz tahtaya çizip, daha sonra belli use-caselere göre bunların simülasyonunu, birbirleri ile iletişimlerini test edebilirsiniz.

# Understanding Root Causes
Microservis sistemler tasarlayıp, monolitik yapılarımızı küçük servislere ayırmadan önce, ilk önce monolitik yapılarımızın neden şuanki büyük hallerini aldıklarını anlamamız lazım.

Bunu anlamak için yapabileceğimiz şeyler nereden başlayacağımızı öğrenmek ve servis testlerini dizaynlarını low cost test etmektir.

Summary
We	decompose	our	system	by	finding	seams	along	which	service	boundaries	can	emerge,
and	this	can	be	an	incremental	approach.	By	getting	good	at	finding	these	seams	and
working	to	reduce	the	cost	of	splitting	out	services	in	the	first	place,	we	can	continue	to
grow	and	evolve	our	systems	to	meet	whatever	requirements	come	down	the	road.	As	you
can	see,	some	of	this	work	can	be	painstaking.	But	the	very	fact	that	it	can	be	done
incrementally	means	there	is	no	need	to	fear	this	work.
So	now	we	can	split	our	services	out,	but	we’ve	introduced	some	new	problems	too.	We
have	many	more	moving	parts	to	get	into	production	now!	So	next	up	we’ll	dive	into	the
world	of	deployment.
