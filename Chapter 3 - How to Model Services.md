# How to Model Microservices
Kitap süresince fikirlerimizi daha iyi gösterebilmek için, MusicCorp adını verdiğimiz hayali bir firma kullanacağız. Bu firma bir online müzik cdsi dağıtım firması ve başarılı olmak için sistemlerinin kolayca değişiklik yapılabilecek bir şekilde tasarlanmasını istiyorlar.

## İyi bir servis?
Microservislerin en önemli özelliği az bağımlılık(coupling) ve çok sıkılık(cohesion) sahibi olmalarıdır.

### Loose Coupling
Servisler birbirine az bağımlı olduğu zaman, bir seviste yapılan değişiklik, diğerini etkilemez. Microservislerin en önemli özelliği, bir serviste değişiklik yaptıktan sonra, diğer servisleri değiştirmeden bu servisi çalıştırabilmektir.

(Fikir: microservis kodlarken(aynı makinede), dosya değişikliklerine göre sistemi yeniden başlatan(nodemon) bir uygulama kullanırken, servislerde birbiri ardına yapılan değişikliklerin sıklığı hesaplanarak, coupling hakkında bilgi edinebilir. Örneğin bir adam a servisinde değişiklik yaptıktan sonra, hemen b klasöründe bir değişiklik yapıyorsa, coupling hakkında alert verilebilir.)

### High Cohesion
Birbiri ile alakalı olan işlemlerin(kodların), aynı yerde, alakasız kodların farklı yerde saklanmasını istiyoruz. Bir davranışta değişiklik yaptığımız zaman, bu davranışla alakalı olan diğer davranışların, aynı serviste olmasını isteriz. Böylece bir servisi yeniden başlatıp, davranışların sonucu görmek kolay olur.

## The Bounded Context
Birbirine bağımlı contextler fikrinde, birden farklı servisin, birbiriyle yaptıkları iletişimin, belli bir interface üzerinde belli olması ile alakalıdır. Örneğin MusicCorp için düşünürsek, Domainimiz tüm işimiz. Depo, resepsiyon, finans ve siparişe kadar herşeyi kapsıyor. Bunların hepsini yazılımımıza eklemeyebiliriz ama yinede bu bizim çalıştığımız domaindir. Depomuzun siparişlerin kargolanması, yeni gelen stokların alımı, geri iadelerin alınması gibi işleri varken... Finans departmanımızın maaş takibi, banka hesaplarının yönetimi ve rapor yazma gibi işleri vardır.

### Paylaşımlı ve Gizli Modeller
Microservis yapısındaki önemli özelliklerden biride, depomuz ve finans departmanımız birbiriyle konuştuğu halde, ortak bir oop modelleme kullanmak yerine, örneğin; deponun, finans servisine bir stok bilgisi vermesi gerekiyorsa, stok modelininin tamamını ve bağımlılıklarını paylaşmak yerine, sadece servisin ihtiyacı duyduğu kadar onunla bilgi paylaşırız.

Yani depo servisinin kendi içinde tuttuğu gizli bir stock modeli varken, finans ile paylaştığı farklı bir stok modeli vardır.

### Modules and Services
Paylaşılacak olan modellerin açıkca düşünülmesi ve gizli modellerin olması, tight couplingden kurtulmamıza yardımcı olur. 

### Premature Decomposition
Yukarıda bahsedilen sebeplerden dolayı, microservisleri monolitik ve anlaşılır bir yapı olmadan, microservislere bölmek zararlı olabilir.

Real world example:
Birbirine benzer 2 toolun, 2.si kodlanırken, 1. toolun yapısında bir microservis modellemesi yapılması ve kodlanması, daha sonra 2. servise ekleme yaptıkca ve 1. servisten farkı anlaşılmaya başlandıkca, microservislerin çok fazla değiştirilmesine sebep oldu. Hatta takım 2. tool'un 1. tooldan çok farklı olduğunu anlayınca, monolitik bir yapıya geçip, 1 yıl sonra sistemi microservis yapısına ayırdı.

## Business Capabilities
Microservis tasarlarken servisler arasında paylaşılan verinin sadece bir veri olmadığını, aslında bu verinin diğer servislerin ne yapabileceğini kısıtlayan bir model olduğunu düşünün.

## Turtles all the way down
Microservis mimarisinde, sisteminizi parçalara ayırdıktan sonra, bu parçaların ne olduğuna dikkate edin. Örneğin sadece depo ve finans diye bir ayırma yapmak yerine depo servisini sipariş alma, stok alma, stok olarak 3 servisede ayırabilirsiniz. Aynı zamanda bu 3 servisi depo servisi olarak boundlelayıp, diğer servislerin sadece 1 servis ile iletişimde olduğunu düşündürüp, aslında bu gelen istekleri 3 küçük microservise yönlendirme yapabilirsiniz.

Böyle nested bir structure yerine tamamen ayrılmış bir structureda seçebilirsiniz. Bu organizasyonunuzun yapısına göre değişebilir.

## İş konseptleri baabında iletişim
Eğer domainimizi represent eden bir microservis decompositionı yapmışsak, bizden istenilen değişikler genelde sadece 1 izole microservis üzerinden yapılır. 

## Teknik sınırlar
Performans kazancı sağlayacak ayrımlar yapmak yerine örneğin (RPC based backend ve stateless bir frontend), işinizi kolaylaştıracak bir ayrıştırma yapmanız daha mantıklı olur.

Summary
In	this	chapter,	you’ve	learned	a	bit	about	what	makes	a	good	service,	and	how	to	find
seams	in	our	problem	space	that	give	us	the	dual	benefits	of	both	loose	coupling	and	high
cohesion.	Bounded	contexts	are	a	vital	tool	in	helping	us	find	these	seams,	and	by	aligning
our	microservices	to	these	boundaries	we	ensure	that	the	resulting	system	has	every
chance	of	keeping	those	virtues	intact.	We’ve	also	got	a	hint	about	how	we	can	subdivide
our	microservices	further,	something	we’ll	explore	in	more	depth	later.	And	we	also
introduced	MusicCorp,	the	example	domain	that	we	will	use	throughout	this	book.
The	ideas	presented	in	Eric	Evans’s	Domain-Driven	Design	are	very	useful	to	us	in
finding	sensible	boundaries	for	our	services,	and	I’ve	just	scratched	the	surface	here.	I
recommend	Vaughn	Vernon’s	book	Implementing	Domain-Driven	Design	(Addison-
Wesley)	to	help	you	understand	the	practicalities	of	this	approach.
Although	this	chapter	has	been	mostly	high-level,	we	need	to	get	much	more	technical	in
the	next.	There	are	many	pitfalls	associated	with	implementing	interfaces	between
services	that	can	lead	to	all	sorts	of	trouble,	and	we	will	have	to	take	a	deep	dive	into	this
topic	if	we	are	to	keep	our	systems	from	becoming	a	giant,	tangled	mess.
1 	http://bit.ly/bounded-context-explained
