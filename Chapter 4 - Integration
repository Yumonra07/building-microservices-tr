# Integration
Microservislerde entegrasyonu doğru yapmak, en önemli şeylerden biridir. Doğru yaptığınız zaman microservisler otonomluklarını korur ve istediğin gibi bağımlı yada bağımsız değiştirme ve yayınlama yapabilirsin. Eğer yanlış yaparsanız, felaket sonuçlar ortaya çıkabilir. Bu chapterı okuduktan sonra, SOA attemplerinde başımıza gelen ve microservislerde başınıza gelebilecek pitfalları öğrenmiş olacağınızı düşünüyorum.

## Looking for the ideal integration technology
Microservislerin birbiriyle konuşmaları için birden fazla yöntem vardır. SOAP, XML-RPC, REST, Protocol Buffers? Bunların hepsinle konuşacağız ama ilk önce seçtiğimiz teknolojiden ne beklediğimizi düşünmemiz lazım.

### Avoid Breaking Changes
Örneğin microservislerden biri gönderdiği veriye extra bir field eklerse, diğer servislerin bozulmaması gerekir.

### Keep your apis technology agnostic
Şuan kullandığınız dil ve teknolojileri ileride değiştirmek isteyebilirsiniz. Bu yüzden servisler arası kullandığınız teknolojiyi dil agnostik tanımlayın.

### Make Your Service Simple for Consumers
Consumerların servislerinizi kullanmasını kolay hale getirin. Eğer servisiniz çok güzel faktörlenmişse fakat servisi kullanacak diğer servisin gereksinimi çoksa, sıkıntı olabilir.

### Hide internal implementation detail
Consumerlarımızın internal implementasyonumuza bağlı olmasını istemeyiz. Bu artacak coupling(bağımlılığa)'e sebep olur. Bu implementasyonumuzun içinde bir şeyin değişmesini istediğimiz zaman, consumerımızında bir şeyleri değiştirmesine sebep olur. Bu techinal debtimiz arttırabiliriz.

## Interfacing with Customers
Örneğin bir müşteri yaratma işlemini ele alırsak, başta basit bir işlem gibi görünsede, müşteriyi yarattığımız zaman trigger edilmesi gereken işlemler olabilir.(finansal payment setting, mail sending), aynı şekilde bir müşteride değişiklik veya silme yaptığımızdada trigger olması gereken eventler olabilir.

Bu işlemi gerçekleştirmenin, farklı yollarını ele alalım.

## Paylaşımlı database
Sektörde en çok görülen entegrasyonlardan biridir. Bir şeyler eklemek istediğimizde veritabanına gideriz! Bir şeyler okumak istediğimizde veritabanına gideriz! İlk düşündüğünüzde oldukca basit ve entegrasyona başlaması en kolay yöntem gibi görünüyor.

Bu senaryoda tüm internal değerlerimiz diğer consumerlar ile paylaşıldığı için, internal yapıda(şemalarda) bir değişiklik yaptığımız zaman, consumerlarımızıda değiştirmemiz gerekiyor.

Aynı zamanda consumerlarımızı spesifik bir teknolojiye bağlamak zorunda kalıyoruz.

Ve bu şekilde kodladığımız kodda, müşteri değiştirme ekleme gibi logicler veritabanı ile konuşan serviste olacağı için, 3 farklı serviste bu işlemi yapmak istersek, bu logicte yapmak istediğimiz değişikliği 3 farklı yerde deploy etmemiz gerekicek. Client library olarak kodladığımızı düşünsen bile, bu servisler çalışırken çalışmalarını durdurup, yeniden başlatmamız gerekicek.


## Services as state machines
Servislerinizi tasarlarken onları CRUD'dan fazlası olarak tasarlarsanız, bu business logicin servis dışına taşmamasını sağlar.

## Versioning
Bir süre sonra microservislerimizin birden fazlasında değişiklik yapmamız, yeni bir versiyon yayınlamamız gerekebilir. Bunu oldukca uzun bir süre ertelemek için yapabileceğimiz şeyler bazıları, servisler arası mesajlaşmalarda strict templateler kullanmamak, tolerant readerlar yaratmak olabilir.

## Catch breaking services early
Servislerinizin consumerlarında sıkıntılar oluşturabilecek değişikler yapmanız gerekecekse, bunu olabildiğince önceden bulmaya çalışın. Ya bu sıkıntıyı embrace edip, consumer servislerin sahipleri ile konuşun, yada sıkıntıyı ortadan kaldıracak bir şekilde değişiklik yapmanın yolunu bulun.

## Semantic Versioning
MAJOR.MINOR.PATCH gibi bir versiyon numaralı kullanıp, major değişik olan servisler ile konuşamıyacağınızı varsayabilirsiniz.

Örneğin 1.3.1 consumer ile uyumlu olan servisimiz 1.4.2 ile çalışabilir ama 2.0.0 ile çalışmayabilir.

## Coexist different endpoints
Yenilemek istediğiniz servisin hem yeni endpointini hemde eski endpointini aynı anda çalışacak şekilde değişiklikleri yayınlayıp, consumerların yavaş yavaş yeni versiyona geçmesini sağlayabilirsiniz.
Bu rest bir apide url'ye /v1/ /v2/ eklemek gibi bir şey olabilecekken, rpcde v1.createCustomer gibi namespaced fonksiyon isimleri yaratmaklada yapılabilir. Fakat RPCde aynı typeda birden farklı versiyonda türleri olan bir typeı yayınlamak zor olabilir.

## Use multiple concurrent service versions
Farklı endpointler yerine, benzer işi yapan farklı versiyonlarda 2 microservis yayınlayıp, eski consumerların eski servislere, yeni consumerların yeni consumerlara eşleşmesini sağlayabilirsiniz. Fakat bu aynı anda iki farklı versiyondaki microservisleri manage etmenizi gerektireceği için, bunu yapmak biraz daha zor olabilir.

Fakat bu işlemi kısa sürede yapmak blue/green deployments veya canary releasesde işinize yarayabilir. Eski consumerları yeni microservislere geçirmek ne kadar uzun sürerse, 2 farklı microservis yaratmak yerine, aynı microservisde 2 farklı endpoint tutmaya doğru kaymanız daha mantıklı olacaktır. Bu şekilde dev timeınızı daha iyi utilize edebilirsiniz.

## User Interfaces
Sunucularımızın ve business logicimizin işlediği verileri göstermesi ve veri elde etmesi için kodlamamız gereken kullanıcı arayüzleri olabilir. Önceden bu arayüzler desktop aplikasyonlar, kiosklardı.

### Toward Digital
Dijital çağın gelmesi ve internetin gelişmesi ile, artık kullanıcı arayüzlerimiz, basit web veya masaüstü uygulamaları olmaktan çıktı ve artık client denildiği zaman, dijital bir saat, iot bir cihaz, bir tarayıcı gibi bir çok şey akla gelmekte.

O yüzden kullanıcı arayüzü dediğimiz zaman aklınıza gelen şey, birden fazla datayı alıp birleştirdiğimiz bir compositional layer olarak düşünülmeli.

### Constraints
Sınırlamalar, sistemimizin kullanıcılarının, sistemimiz ile farklı sınırlamalarla yaptığı etkileşimlerdir. Örneğin masaüstüne sağ click yapmak kolayken, tabletde tek parmak ile kullanılabilecek bir arayüz yapmak gerekebilir.

Bandwithin sıkıntığı olduğu yerlerde sms bile kullanabiliriz.!

### API Composition
Kullanıcı arayüzlerinde veri göstermek için izleyebileceğimiz yollardan biri, client tarafından birden fazla microservise query atıp, dönen mesajların gösterimini yappmaktadır. Mesaj almak için GET request yaptığımız, göndermek, silmek veya değiştirmek için POST request yaptığımız bir senaryo düşünebilirsiniz. Bu senaryoda karşımıza çıkan sorun, her clientden birden fazla query yapmanın client tarafında getirebileceği zararları olduğu gibi, microservislerin döndürdüğü verilerin client için gereksiz olabileceği durumların oluşması olabilir.

Bu yüzden birden fazla internal microservisi çağıran, composite bir servis yaparak, client bazlı bir microservis yapabiliriz. Ama tabi bu microservisde kendi eksileriyle gelir.

### UI Fragment Composition
Sunucu tarafında ui fragmentlarını render edip, client tarafına gönderdiğimiz bir sistem yapabiliriz. Fakat bu sistem çok büyürse oluşacak cohesion yüzünden sıkıntılar yaşayabiliriz.

### Backend For Frontend (BFF) API Composition
Bir diğer çözümde daha öncede bahsedildiği gibi, birden fazla microservisi çağırıp, her frontend için bir backendin yaratıldığı bir sistem olabilir.

### Hybrid
İlla yukarıdaki çözümlerden biri kullanmak zorunda değilizi, websitesi için UI Fragment Composition yapan bir şirket, mobilde BFF kullanabilir veya servislere direk api call yapabilir.

## Integrating With Third Party Software
Yazılım yaparken bazen kendimizin yapmamızın cost effective olmadığı bazı SAAS veya COTS(commercial of the shelf software) satın almamız gerekebilir. Bu durumlarda çokda kontrolümüz altında olmayan bir yazılımı, sistemimize entegre etmemiz gerekebilir. Bu durumda verilen öneri, eğer yapmak istediğiniz şey yapıcağınız iş için uniquese kendiniz kodlayın, değilse satın alın olmalı.

### Lack of control
External bir yazılım veya servis satın aldığınız zaman verebileceğiniz kararlar oldukca kısıtlanır. Örneğin tool ile entagrasyon için hangi dili kullanabilirsiniz? Konfigürasyonu version controlda saklayıp, continous integrationda kullanabilir misiniz vs kısıtlamalarınız olur.

Bu external yazılımı entegre etmenin ne kadar kolay veya zor olacağı, muhattab olduğunuz kişilerle alakalıdır.

### Customization
Bazen external bir tool satın alıp customize etmek, çalışma şeklinizi değiştirmekten daha pahalıya patlayabilir. O yüzden kullanacağınız external toolu customize ederken bu tuzağa düşmemeye çalışın.

### Integration Spaghetti
Sizin kullandığınız birleştirme mekanizmasından farklı, hatta birden fazla integration yöntemi kullanmanızı gerektiren bir ürün düşündürün. Soap, XML-RPC, Binary... Bunlarla iletişim kuracak microservisleri yazarken çekeceğiniz zorlukları düşünün.

Aynı zamanda bundan dahada kötü olan external servisler ise underlying data storelarına direk erişim sağlayan yazılımlardır. Bu şekildede daha önce bahsettiğimiz CRUD based microservice bozuntuları tuzağına düşmüş olursunuz.

### On your own terms
COTS ve SAAS ürünlerin microservislerimiz ve businessımız içinde yerleri tabikide vardır. Bu ürünleri kullanırken kolaylık olması ve customize şansımızı arttırmak için, ilk önce bu servisleri kendi şartlarımıza getirebiliriz.

Bunu bir kaç örnekle açıklamak gerekirse;
#### CMS as a service
En çok customize edilmesi gereken COTS ve SAAS ürünler CMSlerdir.

Örneğin bir CMS'i customize edip kendi ihtiyacımız için kullanmak istersek, CMSin kendini customize etmek yerine, CMSin üzerine bir frontend yazıp, gerekli api calları yaparak CMS yerine, kendi frontendimizde CMSin döndürdüğü verilerin gösterimini yapabiliriz.

#### The multirole CRM system
Bir başka yöntemde crmin farklı parçaları için farklı façadeler yapıp, bunları kullanarak bir platform oluşturmaktır. Zamanla bu façadelerin consumerları ve yaptıkları işler kontrol edilip, façadelerin yeniden kodlanıp kodlanılmayacağına karar verilebilir.

### Strangler Pattern
Strangler pattern servislerimize yapılan isteklerin, uyumlu olan(eski veya yeni) servislere yönlendirilmesiyle çalışır. Proxy vb. bir şey kullanarak bu servisi çalışır hale getirebilirsiniz ve COTS veya SAAS sisteme olan bağımlılığınızı yavaşca yok edebilirsiniz.

Summary
We’ve	looked	at	a	number	of	different	options	around	integration,	and	I’ve	shared	my
thoughts	on	what	choices	are	most	likely	to	ensure	our	microservices	remain	as	decoupled
as	possible	from	their	other	collaborators:
Avoid	database	integration	at	all	costs.
Understand	the	trade-offs	between	REST	and	RPC,	but	strongly	consider	REST	as	a
good	starting	point	for	request/response	integration.
Prefer	choreography	over	orchestration.
Avoid	breaking	changes	and	the	need	to	version	by	understanding	Postel’s	Law	and
using	tolerant	readers.
Think	of	user	interfaces	as	compositional	layers.
We	covered	quite	a	lot	here,	and	weren’t	able	to	go	into	depth	on	all	of	these	topics.
Nonetheless,	this	should	be	a	good	foundation	to	get	you	going	and	point	you	in	the	right
direction	if	you	want	to	learn	more.
We	also	spent	some	time	looking	at	how	to	work	with	systems	that	aren’t	completely
under	our	control	in	the	form	of	COTS	products.	It	turns	out	that	this	description	can	just
as	easily	apply	to	software	we	wrote!
Some	of	the	approaches	outlined	here	apply	equally	well	to	legacy	software,	but	what	if
we	want	to	tackle	the	often-monumental	task	of	bringing	these	older	systems	to	heel	and
decomposing	them	into	more	usable	parts?	We’ll	discuss	that	in	detail	in	the	next	chapter.
