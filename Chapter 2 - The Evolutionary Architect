# The Evolutionary Architect

Projelerimizde microservis yapısını kullanmak, çok sayıda fayda sağladığı gibi ayrıca beraberinde bir çok soruda getiriyor. Bu chapterda bir yazılım mimarının rolünün ne olduğunu ve bu sorulara nasıl cevap vericeğimizi tartışacağız.

## Yanlış karşılaştırmalar

Yazılım mühendisliği dediğimiz şey diğer mühendislikler gibi binlerce yıllık geçmişi ve disiplini olan bir meslek değildir. Mesleğimiz yarı mühendislik, yarı tamircilik içerdiği için, bizim diğer mühendisler gibi fail olmayacağından emin olduğumuz sistemler yapmak için izleyeceğimiz belli yollar yoktur.

Fakat yaptığımız sistemlerin yapım aşamasında ve sonrasında kolayca değişiklik yapabilecek şekilde proje yönetme felsefesini benimseyebiliriz.

Köprünün yarısını yapmış bir mimar, artık geriye dönüp değişiklik yapmakta zorlanabilir.

## Mimar için evrimsel görüş

Gereksinimlerimiz bina inşaatı ve dizaynı yapan kişilere göre oldukca fazla ve hızlı değişmektedir. Aynı zamanda kullandığımız araç ve tekniklerde. Yarattığımız şeyler zamanda sabit olan noktalar değil, yayınlandıktan ve inşa edildikten sonra müşterilerimizin kullanımına göre evrim geçirecek birer üründür.

Bu yüzden mükemmel ürünü ortaya çıkarmak için değil, evrim geçirmesi kolay olan bir sistem yaratmak amacımız olmalıdır.

Yazılım mühendisleri, yazılım geliştiricilerinin üstüne ekleme yapmaktan mutlu olduğu bir sistem geliştirmekle yükümlülerdir.

Bu yüzden bir sistem tasarlarken çok spesifik bir ürün yapmayarak hem kullanıcılarımızı, hemde geliştiricilerimizi memnun edebiliriz(böylece ileride sistemi spesifikleştirmek veya geliştirmek daha kolay olur.)

## Zoning

Microservis geliştirirken nodelarımızın içinde ne olduğuna değil, nodelarımızın arasındaki iletişime daha fazla önem vermeliyiz

## A Principled Approach

Sistem dizaynlarken verdiğimiz kararların hepsi trade-offdur. Microservisler ise bize yapacak çok fazla trade-off vermektedir!

Az deneyimimiz olduğu bir data-store'u, bize daha iyi scale verdiği için seçmeli miyiz? Bazı kararlar anlık olarak verilebileceği gibi, bazı kararları ise yeterince bilgi sahibi olmadan vermemiz gerekebilir.

Bu yüzden vermemiz gereken kararları gruplandıralım.

### Stratejik hedefler

Bu kararlar şirketinizin high-level amaçları ile ilgilidir. Sistem tasarlayan biri olarak, işin business tarafıylada ilgili olmamız lazım. Bu stratejik hedefler; "Asya marketine açılmak", "Müşterilerimizin self-servisden daha iyi yararlanması" gibi hedefler olabilirler.

### Prensipler

Stratejik hedeflerinizle uyuşan prensipleriniz olabilir. Örneğin yeni bir markete açılırken hızlı davranmak istiyorsanız, microservislerinizin takımları hazır oldukları an deployment yaptıkları bir strateji izleyebilirler. Veya yeni bir markete açılırken kullanıcıların local datasını o ülkede tutmak isterseniz, microservislerinizin o localde çalışacağından emin olmak gerektiğini prensip haline getirebilirsiniz.

Çok fazla prensibinizin olmasını istemezsiniz.

### Alışkanlıklar

Prensiplerimizin nasıl işleneceği ile ilgili pratik rehberlerdir. Teknolojisi spesifik olabilirler ve her geliştiricinin anlayacağı kadar low-level olmalıdır. Tüm log datanın centrally yakalabilmesi, http/rest standart mesajlaşma sistemi olması vs.

Alışkanlıklarımız prensiplerimize bağlı olabilir. Örneğin her dev teamin kendi deploymentını yapmak istiyorsak, bunun için her takıma kendi resourcelarını kendileri kontrol ettikleri, isolated AWS hesabı verebiliriz.

### Prensip ve Alışkanları birleştirmek

Bir kişinin prensipleri, diğerinin alışkanlıklarıdır. HTTP/REST kullanmayı bir alışkanlık yerine, prensip olarak belirleyebilirsin.

Tek bir takım için prensip ve alışkanlıkları birleştirmek tamam olabilir ama, büyük organizasyonlar için fakrlı tkaımlar için farklı prensip ve pratikler yapmanız gerekebilir.

Herkesin uyacağı kural ve kararlar koymak, sistemin işleyişi için güzeldir.

### Gerekli Standartlar
Otonom life-cycleları olan microservisleri tasarlarken, büyük resimden gözümüzü ayırmamak lazım.
#### Monitoring
Poll veya Push ile çalışan bir monitoring sistemi yapabiliriz ama önemli olan bunlardan birisi arasında karar vermek ve bunu standart haline getirmektir.
#### Interfaces
Küçük sayıda arayüz seçmek yeni consumerlar yaratırken yardımcı olmaktadır. Bu sadece HTTP/Rest belirleyip kullanmak olarak düşünlmemeli. Fiil mi yoksa isim mi kullanıcaksın, paginationı nasıl handle ediceksin, versiyonlamayı nasıl yapıcaksın... Bunlar düşünülmeli ve standart haline getirilmeli.
#### Architectural Safety
Bir kötü servisin herkesi yok etmesine izin veremeyiz. Unhealthy, downstream servislerden kendini koruyan servisler yapmalıyız.

Örneğin bunun için her servise bir pool yapıp, bu poolar içinde circut breaker yapabiliriz.

Responselarda kurallara göre oynamak ve standart belirlemekte önemlidir. Örneğin HTTP codelara dayanan bir sisteminiz varsa ve bir servisiniz hatalar için 200 http code döndürüyorsa, bu circut breakerınızın kafasını karıştırabilir.

### Kod ile yönetim
Belirli kurallara göre bir sistem tasarlarken, developerlarınızın bu kurallara uyması için işe yarayan bir iki yöntemden bahsetmek gerekirse;
#### Örnekler
Yazılı dökümantasyonlar güzel ve kullanışlıdır. Ancak yazılımcılar codelarla oynayıp, çalıştırmaktan mutluluk duyarlar.

Yazılımcılarınızın çalıştırıp, okuyabileceği örnek sistemler yazın. Bu sistemlerin prodüksüyondaki servisler olması dahada işe yarar. Yazdığınız kuralarrın bir ideal değil, gerçek olduğunu göstermiş olursunuz.
#### Tailored Service Template
Yazılımcılarınızın yeni sistemler yazarken kullanabileceği, özel tasarlanmış frameworkler kodlayabilirsiniz. Fakat bu tarz frameworkleri kodlamak çok fazla coupling yapmanıza veya çok büyük bir framework yaratmanıza sebep olabilir.

O yüzden template yaratırken templatein tam olarak ne işe yarayacağından emin olun. Aynı zamanda oldukca küçük tutmaya bakın. 
### Techincal Debt
Bir feature'ı veya servisi hızlıca çıkarmak istediğimiz için kestiğimiz kısa yolları takip etmek önemlidir. Bu kestirmelerin bize oluşturduğu teknik borçların takibini yapıp, zamanı geldiği zaman intervene etmemiz gerekir.
### İstisna Handling
Kurallarınızda takımınızın ve kendinizin beklentilerine göre istisnalar yapın. Örneğin MYSQL kullanmanın şart olduğu bir şirkette, volume'de çok artış beklediğiniz, scale etmeniz gereken bir servis kullanmayı düşünüyorsanız Cassandrayı istisna olarak sunun.

Geliştiricilerin işini nasıl yapacağının çok kısıtlandığı bir yerde, microservis geliştirmek mümkün olmayabilir.
### Merkezden Yönetim ve Yönlendirme
Takımın gittiği yönün şirketin geleceği ile uyuştuğundan emin olmamız lazım. Eğer birden fazla takımı yöneten bir mimarsanız, bunu her takımdan belli kişilere seçerek ortak bir grup kurarak, önemli kararları bu grupta vererek yapabilirsiniz. Eğer grup sizin karşı çıktığınız bir şey yapmaya çalışırsa, bir çocuğa bisiklet öğretmeye çalışır gibi düşünün. Çok yardım ederseniz hiç öğrenemezler!
### Takım kurmak
Microservisler takımlardaki üyelerin kariyerlerini geliştirmek içinde yardımcıdır. Normal monolitik bir sistemde çok sorumluluk alamayan kişiler, microservislerde bir servisin sahibi olup, önemli kararlar verebilirler.

## Summary
To summarize this chapter, here are	what I see as the core responsibilities of the evolutionary architect:
- Vision
	Ensure there is a clearly communicated technical vision for the system that will help your system meet the requirements of your customers and organization
- Empathy
Understand	the	impact	of	your	decisions	on	your	customers	and	colleagues
- Collaboration
Engage	with	as	many	of	your	peers	and	colleagues	as	possible	to	help	define,	refine,
and	execute	the	vision
- Adaptability
Make	sure	that	the	technical	vision	changes	as	your	customers	or	organization
requires	it
- Autonomy
Find	the	right	balance	between	standardizing	and	enabling	autonomy	for	your	teams
- Governance
Ensure	that	the	system	being	implemented	fits	the	technical	vision
The	evolutionary	architect	is	one	who	understands	that	pulling	off	this	feat	is	a	constant
balancing	act.	Forces	are	always	pushing	you	one	way	or	another,	and	understanding
where	to	push	back	or	where	to	go	with	the	flow	is	often	something	that	comes	only	with
experience.	But	the	worst	reaction	to	all	these	forces	that	push	us	toward	change	is	to
become	more	rigid	or	fixed	in	our	thinking.
While	much	of	the	advice	in	this	chapter	can	apply	to	any	systems	architect,	microservices
give	us	many	more	decisions	to	make.	Therefore,	being	better	able	to	balance	all	of	these
trade-offs	is	essential.
In	the	next	chapter,	we’ll	take	some	of	our	newfound	awareness	of	the	architect’s	role	with
us	as	we	start	thinking	about	how	to	find	the	right	boundaries	for	our	microservices.
