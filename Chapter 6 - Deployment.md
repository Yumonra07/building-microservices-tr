# Deployment
Monolitik bir aplikasyonu deploy etmek oldukca düz bir iştir. Microservislerin birbirlerine olan bağımlılıkları tamamen farklı bir durumdur. Eğer deployment'a doğru yaklaşmazsanız, kompleksliğin hayatınızı sefalete sürükleyebileceği bir konudur.

İlk önce CI ve CD'ye bakarak başlayalım. Bu birbirleri ile bağlantılı ama farklı işlemleri neyi build edeceğimizi, nasıl build edeceğimizi ve nasıl deploy edeceğimizi şekillendirir.

# A Brief Introduction to Continuous Integration
CI yıllardır olan bir teknoloji. Yinede microservisler hakkında konuşurken microservisler arasındaki mapping, buildler ve versiyon kontrol konusunda birden fazla seçeneğimiz olduğu için, bu seçeneklerin üzerinden gidelim.

CI'in temel amaçlarından biri herkesi sync tutmaktır. CI yapısında bir kodda değişiklik yapıldığı zaman, bu kod ci sunucusuna çekilir, basit verifikasyonlar yapılır ve kodun compile olup, testlerin başarılı olduğu gibi durumlar kontrol edilir.

Bu işlemler yapılırken kodun o versiyonu ile ilgili testler çalıştırılabilir. Bu test artifactlarını 7. chapter(Testing)de daha detaylı inceleyeceğiz.

CI'ın birden fazla yararı vardır. Kodumuzun kalitesi hakkında feedback alma, binary oluşturma için otomatik bir sistem ve versiyon kontrol gibi yararlar bunun başındadır.

## Are you really doing it?
Çoğu şirket CI approachını enable eden bir tool veya benzeri bir yapı kullanır. Ama bu şirketlerin CI kullanıp kullanmadığını anlamamız için sormamız gereken 3 temel soru vardır. Eğer bu şirket bunları yapmıyorsa, düzgün bir yapıya geçmesi ve doğru şekilde CI kullanmaya başlaması gerekir.

### Mainline'a günde en az 1 kere check in yapıyor musunuz?
Eğer kodunuzu kısa aralıklarla birleştirmiyorsanız, projeye yapılan bağımsız değişikliklerin artmasına sebep olup, ileride bu değişikliklerin birleştirilmesini zorlaştırırsınız.

### Yaptığınız değişiklikleri doğruluyan bir test suitiniz var mı?
Testler olmadan, kodumuzun syntatic olarak çalıştığını öğreniriz ama, bu sistemimizin davranışlarının düzgün çalıştığı anlamına gelmez. Sistemimizin doğru davrandığını kontrol etmeyen bir CI, CI değildir.

### Build bozulduğunda takımın en büyük(#1) önceliği tamir etmek midir?
Yeşil bir CI sonucu, değişikliklerin başarılı bir şekilde entegre edildiği anlamına gelir. Kırmızı bir sonuç ise edilemediği. Bu durumda bu sorunu çözmek ile ilgili olmayan tüm check-inler durdurulmalı ve bu sorunlar çözülmeye çalışılmalıdır. Yoksa değişiklikler arttıkça, bu sorunu çözmek zorlaşacaktır.

# Mapping Continous Integration to Microservices
Kullanabileceğimiz en basit sistem, tüm microservislerimiz için tek bir repo kullanmak ve repoya bir değişiklik yapıldığında, değişiklik yapılan microservislere bakmaksızın tüm microservisleri build ve test etmek olabilir.

Fakat bu yöntemde yapılan küçük bir değişiklik uzun bir build & test zamanına sahip olacağı ve servisler üzerinde yapılan değişiklikleri commit history'e bakarak anlamamız zor olacağı için, tercih etmesek daha iyi olabilir.

Bu sistemi kullanan şirketler genelde tüm microservisleri birlikte deploy etme hatasına düşürler.

Dahada önemlisi, bu tarz bir yapıyı kullandığımız zaman, bir microserviste yapılan breaking change düzelene kadar, diğer microservislere change yapılması kötü bir durum oluşturabilir.

Bu yapıya bir varyasyon olarak tüm source kodu bir klasör altında toplayıp, birden fazla build processi bu klasörün altındaki microservisler için çalıştırmak olabilir. Fakat bu tarz bir yapıda coupled servisler için aynı anda checkin yapma gibi bir practice ortaya çıkabilir. Yinede bu tarz bir sistem ilk bahsettiğimiz sistemden çok daha iyidir.

Peki başka bir alternatifimiz var mı? Tabiki. Bu alternatifte her microservisimizin kendi source kontrolü vardır ve bu servislere yapılan her check-in, servisin kendine ait CI buildini trigger eder. Bu yapıda servisler arası değişiklikler yapmak zor olabilir.

# Build Pipelines and Continuous Delivery
CD'de yapılan test ve işlemler genelde farklı stagelar üzerinde olur. Örnek bir senaryo (compile & fast tests -> slow tests -> uat(user acceptance testing) -> performance testing -> production) şeklinde olabilir. Bu tarz bir senaryoda her adımda yaptığımız değişikliklerin doğruluğu ve yararından emin oluruz ve productiona taşırız.

CD konusunda genellikle insanlar CI tollarını garip hacklerle extend ederler fakat first class CD ile yapılan bir sistemde, tüm pipeline stageleri modellenip, gözlemlenebilir.

CI'da olduğu gibi CD'dede tüm microservislerimiz için unique 1 pipeline isteyeceğizdir.

## And the Inevitable Exception
Çok fazla deneyim olmayan bir alanda yeni bir projeye başladığınız zaman, bu projede microservis başına pipeline tarzında bir yöntem izlemek yerine, monolitik bir pipeline'a sahip olabilirsiniz.

Hatta boundrylerinizi tam olarak anlayana kadar microservislerinizi tam olarak ayırmayıp, monolitik bir yapıda CI ve CD yapabilirsiniz.

# Platform Specific Artifacts
Çoğu teknoloji kendine ait artifact sistemlerine sahiptir. Örneğin Java JAR ve WAR, Ruby Gem ve Python EGG adını verdiği artifaktlara sahiptir. Genellikle Java aplikasyonları execute edip, içlerinde bir HTTP çalıştırmak mümkünken, PHP vb. teknolojilerde sistem önüne bir NGINX vs. kurmanız gerekebilir. Bu tarz bir durumda, bu uyglamaların konfigürasyonu ve yüklenmesi ile uğraşmamak için Chef gibi bir sistem kullanabilirsiniz.

Otomasyon sisteminizdeki artifaktların deploymentını saklamak için çok yardımcı olur.

# Operating System Artifacts
Birden fazla teknoloji stacki kullanırken işletim sistemi seviyesinde bir artifact kullanmak(rpm, deb, nuget) deployment processinizi kolaylaştırmak için yardımcı olabilir.

Fakat birden fazla işletim sistemi kullanıyorsanız bu approach sıkıntılı olabilir.

# Custom Images
Deployment ve test aşamasında gerek duyduğunuz artifactları yüklemek, uzun zaman alabilir. Bu tarz yükleme işlemlerini her CI, CD işlemi için yapmak, eğer bir microservisi gün içinde öldürüp, sunucunuzu(virtual) silip, yeniden başlattığınız durumlar oluyorsa, size oldukca fazla zaman kaybettirebilir.

Bu tarz durumlarda çoğu virtual sunucu sağlayıcısı size bir custom image kullanma şansı verir.

Fakat önceden custom image yaratma pahalı bir işlem olduğu ve oluşan imageler büyük boyutlu için, bu approach pek kullanılmıyordu. Son zamanlarda Docker ile birlikte bu işlem dahada kolaylaştı. İleride Docker'a bakacağız.

## Images as artifacts
Dependencylerimiz için bir custom image oluşturmakla yetinmeyip, servislerimiz için custom image oluşturma yolunu seçebiliriz. Böylece servislerimiz bir image haline gelir ve deployment processimizde kullanılabilirler. Bu şekilde bir microservisimizin neyle yazıldığı umurumuzda olmaz, sadece çalışır bir image olması yeterli olur. Böylece immutable server adını verdiğimiz bir deployment concepte geçebiliriz.

## Immutable Servers
Deployment processimizde çalışan bir sunucu elde ettikten sonra, bu sunucuda yapılan bir değişiklik, source kontrolümüzdeki kodun konfigürasyonu ile uyuşmaz. Bu duruma configuration drift denir.

Bu yüzden sunucularımızın konfigürasyonunu değiştirilemez yapıp, sunucularda yapılan her değişikliğin pipeline'dan geçmesi gereken bir sistem oluşturabiliriz.

# Environments
Daha önceden bahsettiğimiz CD pipelineımızdaki her stage için, farklı birer environment isteyebiliriz. Örneğin test aşamasında single host isteyebileceğimiz gibi, production esnasında birden fazla host ve load balancing ile çalışmak isteyebiliriz. 

# Service Configuration
Servislerimizin konfigürasyonları oldukça küçük ve genellikle environmentden environmente değişen şeyler olmalıdır. Örneğin veritabanına bağlanmak için hangi username ve şifreyi kullanmalıyım*

Buna en basit çözümlerden biri environment başına bir artifact oluşturmak olabilir, fakat artifact build etmek oldukca zaman alabilir ve belirli bir environment için build edilen artifactın geçtiği testleri, diğer build edilen bir artifact geçmeyebilir.

Aynı zamanda source kodumuz içinde şifre vb. bilgilerin olmasınıda istemeyiz. Genelde bu tarz şeyleri manage etmek zor olmaktadır. Bu yüzden daha ileride bahsediceğimiz konfigürasyonlar için kullandığımız ayrı bir artifact senaryosu, bizim için daha mantıklı olabilir.

# Service-to-Host Mapping
Microservis deployment aşamasında kendimize soracağımız sorulardan biride, host başına kaç microservis çalıştıracağımız vb. tarzında sorulardır. Bu soruya vereceğiniz cevap, deploymentda limitlenmenize sebep olabilir.

Host: virtual bir makinede olabilir, fizikselde biz bunu servis çalıştırabileceğimiz bir işletim sistemi olarak kabul edeceğiz.

## Multiple Services Per Host
Servis sayımız arttıkca host sayımız değişmeyeceği için, maintenance aşamasında yarar sağlayabilir. Monitöringi zorlaştırabiliriz, örneğin CPU kullanımını takip ederken 1 servisin ne kadar cpu kullandığını mı yoksa host'un ne kadar cpu kullandığına bakacağım? Aynı zamanda side-effectlerden kurtulmasıda zor olur. Eğer bir servisin resource kullanımı artarsa, diğer servisleri yavaşlatabilir.

Bu approachı kullandığınız zaman, basit olmasına rağmen, microservislerin en büyük yararlarından biri olan yazılımımızın farklı independent release'inden vazgeçtiğimiz için oldukca fazla şey kaybederiz.

Bu approach aynı zamanda immutable servers ve image based deployments gibi muhabbetleride ortadan kaldırır.

Aynı zamanda microservislerin hostda değişiklik yapmaları gerekirse buna kim karar vericek?

Tüm servisleri tek bir hostda tutmak, ihtiyaçları farklı olan servislerimize, aynı hostu dayatmak olur.

## Application Containers
Aplikasyon konteynırları birden fazla servisin tek bir host üzerinde bir aplikasyon konteynırı adı verilen process içinde çalışması ile olur. Bu approachın iyi yanlarından birisi birden fazla servisin gereksinim duyduğu JVM gibi dil runtimelarının tek bir instance'ını çalıştırmak olabilir. Aynı zamanda additional logging ve monitoring konusunda da bize yardımcı olabilirler. Fakat teknoloji spesifik oldukları için, bu konteynırlar ile kendimizi kısıtlamak, ileride coupling sorunları yaşamamıza sebep olabilir.

Bu konteynırların çoğu servislerinizin kullanabileceği bir shared memory verdiklerinden bahsederler fakat biz bunu avoid etmek isteriz.
Aynı zamanda bu tarz konteynırlarda ortak kullanımda olan resourcelar olduğu için, resource monitoring gittikce zorlaşır.

## Single Service Per Host
Her microservisi kendi hostunda çalıştırarak, birden fazla microservisi tek hostda çalıştırmanın ortaya çıkardığı yan etkilerden kurtulmuş oluyoruz. Ayrıca bu yapıda daha önceden bahsettiğimiz immutable servers ve image based deployment yapısını kullanmamızda bize oldukca yardımcı olur. Bu yapıda aynı zamanda resource usage'ı kontrol etmek ve ilgimizi duyan host/servise ilgi göstermek daha kolay olur.

Eğer güzel bir PaaS seçeneğiniz yoksa bu yöntem güzel bir yöntemdir. Bu yöntemin downsidelarından biri birden fazla hostu manage etmek zorunda kalmamızdır.

## Platform as a Service
Heroku, IBM Bluemix benzeri sistemler buna örnektir. Microservislerinizi sizin için manage etmeye çalışmakla birlikte size farklı servislerde sunabilirler. Bu sistemler 2015 yılı itibari ile ne kadar akıllı olmaya çalışırlarsa genelde o kadar çok hata yapar durumdalar. O yüzden eğer işinize yarayacağınızdan emin değilseniz Single Service Per Host yapısı ile işe atılmanız daha iyi olacaktır.

# Automation
Microservislerin kompleksitesini kontrol altında tutmak için otomasyon anahtardır. Deploymentlarınızı otomatik yapabiliyorsanız, tek bir satırla virtual machine başlatabiliyor musunuz vs. bunların hepsi işinize yarayacak ve kompleksitiyi sizin için azaltıcak şeylerdir. SSPH ile gittiğiniz zaman, elle birden fazla hostu kontrol etmeniz oldukca zorlaşacaktır.

## Case Studies
Yıllardır süregelen deneyimimde, ilk önce otomasyon ve toolinge önem veren şirketlerin ilk başta yavaşta olsa bir kaç microservis çıkardıktan sonra, toolingi ve otomasyonu yerine oturttuktan sonra hızlıca, yüzlerce microservis çıkartabildiğine ve manager edebildiğine şahit oldum.

## From Physical to Virtual
Önceden yavaş ve cluncky olan yapay makineler, artık yeni teknolojilerin ortaya çıkması ile birlikte rahatça kullanılabilir ve denenmesi şart olan bir teknolojiye dönüştü.

### Traditional Virtualization
Geleneksel yapaysallaştırmada, bir işletim sistemine kurulan hypervisor bizim birden fazla işletim sistemi, ve bu işletim sistemleri içinde servis çalıştırmamıza yardımcı olur. Bu tarz bir sernayoda, birden fazla microservis için tek bir makine kullanıp, microservisleri izole etmiş oluruz. Fakat bu senaryoda ne kadar çok microservis çalıştırırsak, ve bu microservisler hypervisor'ün başlattığı işletim sistemlerine göre ne kadar küçük olursa, o kadar büyük bir overheadimiz olur ve bir süre sonra bunu kullanması pratik olmayabilir.

### Vagrant
Vagrant bir text dosyası ile işletim sistemlerinizi ve bu işletim sistemleri arasındaki bağlantıları tanımlamanıza yarayan, yapay bir cloud oluşturma aracıdır. Bu şekilde production ortamına yakın bir ortama localde sahip olabilirsiniz. Fakat bazen production ortamı çok büyük olduğu için, localde çalışmayabilir. Bu yüzden daha küçük parçalara bölüp, vagrant configleri oluşturmanız ve manage etmeniz gerekebilir.

### Linux Containers
Çoğu yeni linux işletim sistemiyle gelen container yapısı, linuxda her process spawn edilince, spawnee'nin spawn eden processin child'ı olması mantığını extend etmesiyle gelişir. Containerlarınızdaki  çalıştırmak istediğiniz işletim sistemleri, aynı kerneli paylaşabildiği sürece herhangi bir sıkıntı yaşamadan farklı işletim sistemleri çalıştırabilirsiniz. Artık şişman VM'ler yaratıp başlatmak zorunda kalmıyacağınız için, çok daha fazla microservisi tek bir makine üzerinde, daha az resource'u isolation için harcayarak çalıştırabilirsiniz.

### Docker
Docker LXC'lerin üzerine build ederek sizin için networking, revisioning gibi sorunları halledip, size bir registry verir. Aynı zamanda bir abstraction sağladığı için, tek yapmamız gereken bir docker image oluşturup, bu image'ı istediğimiz yerde çalıştırmak olur.

Docker tek başına tüm ihtiyaçlarınıza çözüm olamaz. Örneğin scheduling için Google'ın Docker üzerine kurduğu Kubernetes veya, Heroku tarzı bir scheduling stratejisi inceleyen Deis'i inceleyebilirsiniz.

Docker son zamanlarda çok revaçta olan ve kesinlikle göz atmanız gereken bir teknoloji.

Örneğin dockerı vagrant ile kullanıp vagrant ile her servis için bir vm açmak yerine, bir vm açıp içine docker sistemimizi build edebiliriz.

# A deployment interface
Kullandığınız teknolojiler ve servisleriniz ne olursa olsun, deployment için ortak bir dile sahip olmak çok önemlidir. Bu dil örneğin localde test yaparken local makinenizdeki, test aşamasında latest green buildi, diagnosing/debugging aşamasında exact bir buildi çalıştırabilecek kapasite olmalı, ve nereye build ediceğinizi belirttiğiniz bir parametreniz olmalı. Örnek bir deployment scripti: `$	deploy	artifact=catalog	environment=local	version=local` olabilir.

## Environment Definition
Deployment aşamasında envrionemtnımızı belirlemek için, bir sistemde ne kadar node çalıştıracağımız, bu nodeların/servislerin tanımı gibi bilgileri tuttuğumuz config dosyaları olmalı. Bunun için örnek olarak docker compose config dosyalarına bakabilirsiniz.

Summary
We’ve	covered	a	lot	of	ground	here,	so	a	recap	is	in	order.	First,	focus	on	maintaining	the
ability	to	release	one	service	independently	from	another,	and	make	sure	that	whatever
technology	you	select	supports	this.	I	greatly	prefer	having	a	single	repository	per
microservice,	but	am	firmer	still	that	you	need	one	CI	build	per	microservice	if	you	want
to	deploy	them	separately.
Next,	if	possible,	move	to	a	single-service	per	host/container.	Look	at	alternative
technologies	like	LXC	or	Docker	to	make	managing	the	moving	parts	cheaper	and	easier,
but	understand	that	whatever	technology	you	adopt,	a	culture	of	automation	is	key	to
managing	everything.	Automate	everything,	and	if	the	technology	you	have	doesn’t	allow
this,	get	some	new	technology!	Being	able	to	use	a	platform	like	AWS	will	give	you	huge
benefits	when	it	comes	to	automation.
Make	sure	you	understand	the	impact	your	deployment	choices	have	on	developers,	and
make	sure	they	feel	the	love	too.	Creating	tools	that	let	you	self-service-deploy	any	given
service	into	a	number	of	different	environments	is	really	important,	and	will	help
developers,	testers,	and	operations	people	alike.
Finally,	if	you	want	to	go	deeper	into	this	topic,	I	thoroughly	recommend	you	read	Jez
Humble	and	David	Farley’s	Continuous	Delivery	(Addison-Wesley),	which	goes	into
much	more	detail	on	subjects	like	pipeline	design	and	artifact	management.
In	the	next	chapter,	we’ll	be	going	deeper	into	a	topic	we	touched	on	briefly	here.	Namely,
how	do	we	test	our	microservices	to	make	sure	they	actually	work?


