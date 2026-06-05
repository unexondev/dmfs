# Data Management — Sınav Özeti

Kapsam: Complex Data Types (Ch-8), Indexing & B-Trees Part 1 (Ch-14), Indexing & B-Trees Part 2 (Ch-14), Application Development (Ch-9), Big Data (Ch-10). Anlatım Türkçe, teknik terimler orijinal İngilizce halinde bırakılmıştır.

---

## 1) Complex Data Types (Chapter 8)

### Giriş
Relational model çok yaygın kullanılır ama temel bir kuralı vardır: data values **atomic** olmalıdır. Yani multivalued, composite ve diğer complex data type'lar core relational model tarafından kabul edilmez. Ancak birçok uygulamada bu atomicity kısıtı problem yaratır. Bu bölümde yaygın kullanılan non-atomic data type'lar incelenir: **semi-structured data, textual data, spatial data**.

### Semi-Structured Data
Relational tasarımda tablolar sabit sayıda attribute içerir ve her biri atomic value tutar. Schema değişikliği (örn. yeni attribute eklemek) nadirdir ve genelde application code değişikliği gerektirir. Oysa birçok uygulama, schema'sı sık sık değişen complex data saklamak zorundadır; hızlı evrilen web uygulamaları buna örnektir.

Örnek: farklı uygulamaların erişmesi gereken bir user profile. Profile çeşitli attribute'lar içerir ve sık sık yeni attribute eklenir. Örneğin bir "set of interests" attribute'u kullanıcıya ilgi alanına göre içerik göstermek için kullanılabilir. Böyle bir set ayrı bir relation'da normalized şekilde saklanabilse de, bir **set data type** kullanmak normalized gösterimden çok daha verimli erişim sağlar.

**Data exchange** semi-structured data'dan büyük fayda sağlar. Exchange, uygulamalar arasında ya da bir uygulamanın back-end ile front-end'i arasında olabilir. Web-services bugün çok yaygındır; complex data front-end'e çekilir ve mobil app ya da JavaScript (browser) ile gösterilir. Client makinesinde çalışabilmek, geliştiricilere çok responsive arayüzler kurma imkânı verir. **JSON ve XML** en yaygın semi-structured data model'leridir.

### Features of Semi-Structured Data Models
Relational model modern ihtiyaçlar için birkaç yönden genişletilmiştir:

- **Flexible schema:**
  - *Wide column representation:* her tuple farklı bir attribute kümesine sahip olabilir, istenildiği an yeni attribute eklenebilir.
  - *Sparse column representation* (kısıtlı form): schema sabit ama çok büyük bir attribute kümesine sahiptir, her tuple bunların yalnızca bir alt kümesini saklar.
- **Multivalued data types (non-atomic):**
  - *Sets, multisets* — örn. set of interests `{'basketball', 'La Liga', 'cooking', 'anime', 'jazz'}`.
  - *Key-value map* (kısaca map): key-value pair kümesi saklar. Örn. `{(brand, Apple), (ID, MacBook Air), (size, 13), (color, silver)}`. Map üzerindeki operasyonlar: `put(key, value)`, `get(key)`, `delete(key)`.
  - *Arrays:* bilimsel ve monitoring uygulamalarında yaygın. Örn. düzenli aralıklarla alınan ölçümler `(time, value)` pair'leri yerine değer array'i olarak tutulur: `[5, 8, 9, 11]` yerine `{(1,5),(2,8),(3,9),(4,11)}` — time saklamaya gerek yok, offset kullanılır. Image'lar da temelde iki boyutlu pixel value array'leridir.
- Multi-valued attribute type'lar geçmişte **non first-normal-form (NFNF)** data model ile modellenirdi; bugün çoğu database tarafından desteklenir.
- **Array database:** array'ler için özel destek sağlayan database (sıkıştırılmış depolama, query language uzantıları). Örnekler: Oracle GeoRaster, PostGIS (PostgreSQL eklentisi), SciDB, MonetDB.

### Nested Data Types
Birçok gösterim attribute'ların yapılandırılmasına (structured) izin verir; bu, E-R model'deki **composite attribute**'ları doğrudan modeller. Örn. `name` attribute'unun `firstname` ve `lastname` alt attribute'ları olabilir. Bu data type'lar bir hierarchy oluşturduğu için "nested data types" terimi kullanılır. Hiyerarşik data birçok uygulamada yaygındır. İki yaygın gösterim — sabit schema'ya uymak zorunda olmayan, esnek:

- **JSON (JavaScript Object Notation):** bugün çok yaygın.
- **XML (Extensible Markup Language):** daha eski nesil notasyon, hâlâ yaygın kullanılır.

Her ikisi de set, array, map gibi multivalued data type'ları destekler.

### JSON
Complex data type'ların textual representation'ı; data exchange için çok yaygın. Desteklediği yapılar:

- Types: integer, real, string.
- Objects: key-value map'lerdir, yani (attribute name, value) pair kümeleri.
- Arrays de key-value map'tir (offset'ten value'ya).

Örnek JSON:
```json
{
  "ID": "22222",
  "name": { "firstname": "Albert", "lastname": "Einstein" },
  "deptname": "Physics",
  "children": [
    {"firstname": "Hans", "lastname": "Einstein"},
    {"firstname": "Eduard", "lastname": "Einstein"}
  ]
}
```
Burada `name` bir object, `children` bir array'dir.

JSON bugün data exchange'de her yerdedir (primary), özellikle web services için. Modern uygulamalar genelde web services etrafında tasarlanır. SQL uzantıları:

- JSON data saklamak için JSON data type'ları.
- SQL query'leri relational data'dan JSON data üretebilir. Örn. `json_build_object('ID', 12345, 'name', 'Einstein')` → `{"ID": 12345, "name": "Einstein"}`.
- Syntax ve semantics database'den database'e çok değişir.

JSON ↔ object representation (JavaScript, Java, Python, PHP vb.) dönüşümü için birçok library mevcuttur. JSON **verbose**'dur (aynı data için daha çok yer kaplar) ve text'i parse edip alanları almak CPU-intensive olabilir. Verimli depolama için **BSON (Binary JSON)** gibi compressed gösterimler kullanılır (parsing gerekmeden).

### XML
XML daha eski bir gösterimdir; configuration bilgisi saklamak ve data exchange için birçok sistem kullanır. Bilgiyi açılı parantez `<>` içine alınan **tag**'lerle işaretler. `<tag>` ve `</tag>` çiftler halinde kullanılır. Relational data'yı, relation ve attribute isimlerini tag olarak vererek temsil edebilir:
```xml
<course>
  <course_id> CS-101 </course_id>
  <title> Intro. to Computer Science </title>
  <dept_name> Comp. Sci. </dept_name>
  <credits> 4 </credits>
</course>
```
Tag'ler hierarchical olabilir. Relational schema'nın aksine yeni tag'ler kolayca, uygun isimlerle eklenebilir. Data **self-documenting**'dir: insan, isme bakarak parçanın ne anlama geldiğini anlayabilir/tahmin edebilir.

XML data exchange örneği — purchase order: bir organizasyon tarafından üretilip başka birine gönderilir, çeşitli bilgi içerir; iki taraf hangi tag'lerin geçeceği ve ne anlama geldiği konusunda anlaşmak zorundadır.
```xml
<purchase_order>
  <identifier> P-101 </identifier>
  <purchaser>
    <name> Cray Z. Coyote </name>
    <address> Route 66, Mesa Flats, Arizona 86047, USA </address>
  </purchaser>
  <supplier> <name> Acme Supplies </name> ... </supplier>
  <itemlist>
    <item>
      <identifier> RS1 </identifier>
      <description> Atom powered rocket sled </description>
      <quantity> 2 </quantity>
      <price> 199.95 </price>
    </item>
    ...
  </itemlist>
  <total_cost> 429.85 </total_cost>
</purchase_order>
```
Nested XML yapıları sorgulamak için **XQuery** geliştirilmiştir ama bugün yaygın kullanılmaz. SQL de XML desteği için genişletilmiştir: XML data type olarak saklama, relational data'dan XML üretme, XML data type'ından veri çıkarma.

### Knowledge Representation — RDF
**RDF (Resource Description Framework)**, entity-relationship model temelli ama esnek schema'lı bir data representation standardıdır. Attribute'lara ve diğer object'lerle relationship'lere sahip object'leri modeller. Data'yı iki formdaki **triple** kümesi olarak temsil eder:

- `(ID, attribute-name, value)`
- `(ID1, relationship-name, ID2)`

Doğal bir **graph representation**'a sahiptir. ID, ID1, ID2 entity identifier'larıdır; entity'lere RDF'te **resource** da denir. E-R model'den farklı olarak RDF yalnızca **binary relationship**'leri destekler.

RDF, knowledge base representation olarak yaygındır. Triple'lar 3 attribute'a sahiptir: **(subject, predicate, object)**. Örnekler: `(NBA-2019, winner, Raptors)`, `(Washington-DC, capital-of, USA)`, `(Washington-DC, population, 6,200,000)`. Object'lerin type bilgisi **instance-of** relationship ile verilir; örn. 10101 instructor instance'ı, 00128 student instance'ı. **Knowledge graph**'lar Wikipedia, Wikidata gibi kaynaklardan toplanan fact'leri saklar.

**Graph View:** object'ler oval, attribute value'lar dikdörtgen, relationship'ler etiketli edge olarak gösterilir.

### Querying RDF: SPARQL
SPARQL, RDF data sorgulamak için tasarlanmış query language'dir; **(subject, predicate, object)** triple pattern'leri kullanır. Örnek: `?cid title "Intro. to Computer Science"` triple'ı `(CS-101, title, "Intro. to Computer Science")` ile eşleşir; `?sid sec_course ?cid` triple'ı `(sec1, sec_course, CS-101)` ile eşleşir. Paylaşılan `?cid` değişkeni iki triple pattern arasında bir join condition uygular.

"Intro. to Computer Science" başlıklı bir course'un section'ını alan tüm öğrencilerin isimlerini getiren query:
```sparql
select ?name
where {
  ?cid title "Intro. to Computer Science" .
  ?sid course ?cid .
  ?id takes ?sid .
  ?id name ?name .
}
```

### Textual Data
Textual data unstructured text'ten oluşur. **Information retrieval** terimi, unstructured textual data'nın sorgulanmasını ifade eder. Geleneksel modelde textual information **document**'lar halinde organize edilir. Bir database'de text-valued bir attribute bir document sayılabilir; web'de her web page bir document'tır.

**Information retrieval** = unstructured data sorgulama. Basit keyword query modeli: verilen keyword'ler için, hepsini içeren document'ları getir. Daha gelişmiş modeller document'ların **relevance**'ını sıralar. Bugün keyword query'leri birçok tip bilgi döndürebilir (örn. "cricket" → süren maç bilgileri). **Relevance ranking** zorunludur çünkü genelde çok sayıda eşleşen document vardır.

Web search engine'leri özünde information retrieval sistemleridir: web'i **crawl** ederek web page'leri getirip saklar, kullanıcı keyword query gönderir, sistem keyword içeren saklı sayfaları bulur. Bugün artık sadece sayfa getirmekten öteye geçmişlerdir.

### Ranking using TF-IDF
**Term:** document/query'de geçen keyword. Soru: verilen bir term `t` için, bir document `d` ne kadar relevant?

**Term Frequency** — TF(d, t):
```
TF(d, t) = log(1 + n(d,t)/n(d))
```
n(d,t) = `t` term'inin `d` document'ında geçme sayısı; n(d) = `d`'deki toplam term sayısı.

Bir query Q birden çok keyword içerebilir. Basit yöntem ölçüleri toplamaktır, ancak tüm term'ler eşit değildir. Örneğin "database" sık geçen, "Silberschatz" nadir geçen bir term'dir. "Silberschatz" içeren ama "database" içermeyen bir document, tersine göre daha yüksek sıralanmalıdır. Bunu düzeltmek için **inverse document frequency (IDF)** ile term'lere ağırlık verilir.

**Inverse document frequency** — IDF(t):
```
IDF(t) = 1 / n(t)
```
n(t) = `t` term'ini içeren document sayısı (sistemin index'lediği içinde).

Bir document'ın bir query Q'ya relevance'ı:
```
r(d, Q) = Σ (t ∈ Q) TF(d, t) * IDF(t)      (TF–IDF approach)
```
Diğer tanımlar kelimelerin **proximity**'sini de hesaba katar: term'ler document'ta birbirine yakınsa daha yüksek sıralanır. **Stop words** genelde göz ardı edilir.

### Retrieval Effectiveness
- **Precision:** döndürülen sonuçların yüzde kaçı gerçekten relevant.
- **Recall:** relevant sonuçların yüzde kaçı döndürüldü.

### Spatial Data
Spatial database'ler spatial location'larla ilgili bilgiyi saklar; spatial data'nın verimli depolanması, indexing'i ve sorgulanmasını destekler.

- **Geographic data:** road map, land-usage map, topographic elevation map, political map (sınırlar), land-ownership map vb. **GIS (Geographic Information Systems)**, geographic data saklamak için özel database'lerdir. Round-earth coordinate system kullanılabilir: (Latitude, longitude, elevation).
- **Geometric data:** object'lerin nasıl inşa edildiğine dair design bilgisi (bina, uçak, IC layout tasarımları). 2 veya 3 boyutlu Euclidean space, (X, Y, Z) coordinate'larıyla.

**Representation of Geometric Constructs:**
- *Line segment:* endpoint'lerinin coordinate'larıyla temsil edilir.
- *Polyline (open polygon) / linestring:* bağlı line segment dizisi; sıralı endpoint coordinate'ları listesiyle temsil edilir. Bir curve, segment dizisine bölünerek yaklaşık temsil edilir (örn. yollar için kullanışlı).
- *Polygon:* sıralı vertex listesiyle; liste bir polygonal region'ın sınırını belirtir. Triangle kümesi (triangulation) olarak da temsil edilebilir.
- 3-D'de gösterim 2-D'ye benzer, point'lerde ekstra `z` bileşeni vardır.

Birçok database **geometry ve geography** data type'larını destekler (örn. SQL Server, PostGIS). Subtype'lar: point, linestring, curve, polygon. Collection'lar: multipoint, multilinestring, multicurve, multipolygon. Örnekler: `LINESTRING(1 1, 2 3, 4 4)`, `POLYGON((1 1, 2 3, 4 4, 1 1))`. Type conversion: `ST_GeometryFromText()`, `ST_GeographyFromText()`. Operasyonlar: `ST_Union()`, `ST_Intersection()`.

**Design Databases:** CAD sistemleri geleneksel olarak data'yı editing sırasında memory'de tutar, oturum sonunda file'a yazardı. Dezavantajı: data'yı bir formdan diğerine dönüştürme maliyeti. **Object-oriented database**'ler design bileşenlerini object (genelde geometric object) olarak temsil eder; object'ler arası bağlantılar tasarımın nasıl yapılandırıldığını gösterir. Basit 2-D object'ler: point, line, triangle, rectangle, polygon. Complex 2-D object'ler: basit object'lerden union/intersection/difference ile oluşturulur. Complex 3-D object'ler: sphere, cylinder, cuboid gibi daha basit object'lerden aynı operasyonlarla oluşturulur. Design database'leri object'ler hakkında non-spatial bilgi de saklar (malzeme, renk). **Spatial integrity constraint**'ler önemlidir (örn. pipe'lar kesişmemeli, wire'lar birbirine çok yakın olmamalı).

**Geographic Data — iki tip:**
- *Raster data:* iki veya daha fazla boyutta bit map / pixel map. Örn. bulut örtüsünün uydu görüntüsü (her pixel bir alandaki bulut görünürlüğünü saklar). Ek boyutlar: farklı yüksekliklerdeki sıcaklık, farklı zamanlardaki ölçümler. Design database'leri genelde raster data saklamaz.
- *Vector data:* temel geometric object'lerden oluşturulur (point, line segment, triangle, polygon; 3-D'de cylinder, sphere, cuboid, polyhedron). Map data'yı temsil etmek için sık kullanılır. Yollar 2-D kabul edilip line/curve ile temsil edilir; nehirler genişliklerine göre complex curve ya da complex polygon olabilir; göl ve bölgeler polygon olarak gösterilir.

**Spatial Queries:**
- *Region queries:* spatial region'larla ilgilenir; örn. belirli bir region içinde kısmen/tamamen olan object'ler. PostGIS: `ST_Contains()`, `ST_Overlaps()`.
- *Nearness queries:* belirli bir location'a yakın object'ler. **Nearest neighbor query:** verilen bir point/object için, koşulu sağlayan en yakın object'i bul.
- *Spatial graph queries:* spatial graph'lara dayalı (örn. road network üzerinden iki nokta arası shortest path).
- *Spatial join:* iki spatial relation'ın, location'un join attribute rolü oynadığı join'i.
- Region'ların intersection/union'ını hesaplayan query'ler.

---

## 2) Indexing & B-Trees Part 1 (Chapter 14)

> To see simulation, check: [simulation link](https://unexondev.github.io/dmfs/BPlusTree_Simulation.html)

### Basic Concepts
Birçok query bir file'daki kayıtların yalnızca küçük bir kısmına başvurur (örn. "Physics bölümündeki tüm instructor'ları bul"). Her tuple'ı okumak verimsizdir. **Index**, kitabın sonundaki dizin gibi çalışır: aranan değer sorted order'da tutulur, böylece istenen kaydın hangi disk block'ta olduğu hızlıca bulunur ve sadece o block fetch edilir.

- **Search Key:** bir file'da kayıtları aramak için kullanılan attribute (ya da attribute kümesi).
- **Index file:** `(search-key, pointer)` formundaki kayıtlardan (index entry'lerden) oluşur. Index file'lar genelde orijinal file'dan çok daha küçüktür.
- Çok büyük database'lerde basitçe sorted bir ID listesi tutmak işe yaramaz çünkü: (i) index'in kendisi çok büyük olur, (ii) sorted olsa bile arama hâlâ uzun sürebilir, (iii) ekleme/silmede sorted listeyi güncellemek çok pahalıdır. Bu yüzden daha gelişmiş teknikler kullanılır.

**İki temel index türü:**
- **Ordered indices:** search key'ler sorted order'da saklanır.
- **Hash indices:** search key'ler bir hash function ile bucket'lara düzgün (uniform) dağıtılır.

### Index Evaluation Metrics
Her teknik şu faktörlerle değerlendirilir:
- **Access types:** verimli desteklenen erişim türleri (belirli değere sahip kayıtlar; belirli aralıktaki değere sahip kayıtlar).
- **Access time:** bir veri öğesini bulma süresi.
- **Insertion time:** yeni veri ekleme süresi.
- **Deletion time:** veri silme süresi.
- **Space overhead:** index yapısının kapladığı ek alan.

Bir file için birden fazla index istenebilir (örn. kitabı author, subject veya title ile aramak). Bir file'da kayıt aramak için kullanılan attribute(lar) **search key** olarak adlandırılır — bu, primary key/candidate key/superkey'den farklı bir kavramdır. Birden çok index varsa, birden çok search key vardır.

### Ordered Indices
Ordered index'te entry'ler search key value üzerine sorted saklanır.
- **Clustering index (Primary index):** sequentially ordered bir file'da, search key'i file'ın sequential order'ını belirleyen index. Primary index'in search key'i genelde (zorunlu değil) primary key'dir.
- **Nonclustering index (Secondary index):** search key'i file'ın sequential order'ından farklı bir sıra belirten index.
- **Index-sequential file:** bir search key üzerine ordered, o search key üzerinde clustering index'e sahip sequential file.

**Pointer:** bir kayda işaret eden pointer, disk block identifier'ı ile block içindeki offset'ten oluşur (kaydı block içinde belirler).

İki tür ordered index: **Dense ve Sparse**.

### Dense Index Files
**Dense index:** file'daki her search-key value için bir index entry vardır.
- *Dense clustering index*: index record search-key value ve o değere sahip ilk data record'a pointer içerir. Aynı değere sahip kalan kayıtlar ilk record'dan sonra sequential saklanır.
- *Dense nonclustering index*: index, aynı search-key value'ya sahip tüm record'lara pointer listesi saklamalıdır.

### Sparse Index Files
**Sparse index:** yalnızca bazı search-key value'lar için index record içerir. Sadece kayıtlar search-key üzerine sequentially ordered olduğunda uygulanabilir. Search-key value `K`'yi bulmak için:
1. `K`'den küçük en büyük search-key value'ya sahip index record'u bul.
2. O index record'un işaret ettiği record'tan başlayarak file'ı sequential ara.

**Dense ile karşılaştırma:**
- Daha az alan, ekleme/silmede daha az maintenance overhead.
- Kayıt bulmada genelde dense'ten daha yavaş.
- İyi tradeoff: clustered index için file'daki her block'a bir index entry (block'taki en küçük search-key value'ya karşılık). Unclustered index için: dense index üzerine sparse index (**multilevel index**).

### Multilevel Index
1,000,000 tuple'lık bir relation üzerinde dense index düşünün. Index entry'ler data record'dan küçüktür; 4-KB block'a 100 index entry sığsın → index 10,000 block tutar. Relation 100,000,000 tuple olsaydı index 1,000,000 block = 4 GB tutardı. Böyle büyük index'ler diskte sequential file olarak saklanır.

- Sequential search `b` sequential block read gerektirir.
- Index file'da **binary search** kullanılabilir, ama maliyet hâlâ yüksektir: index `b` block tutuyorsa binary search en fazla `⌈log2(b)⌉` block okur. Her okuma bir **random (non-sequential) I/O** gerektirir. 10,000-block'luk index için binary search worst case 14 random block read; random read ortalama 10 ms ise arama 140 ms ≈ 7 access/sec.

**Çözüm:** index memory'ye sığmıyorsa, diskteki index'i sequential file gibi alıp üzerine bir sparse index inşa et (sorted olmalı):
- **outer index** — basic index'in sparse index'i.
- **inner index** — basic index file.

Kayıt bulmak için: önce outer index'te binary search ile istenen değerden küçük/eşit en büyük search-key value'lu record bulunur; pointer inner index'in bir block'una işaret eder; bu block taranarak hedef bulunur. Outer index de memory'ye sığmıyorsa bir seviye daha eklenir, vb. Multilevel index, binary search'e göre çok daha az I/O gerektirir ve **tree** yapılarıyla (in-memory binary tree) yakından ilişkilidir.

### Index Update
Her kayıt ekleme/silme işleminde her index güncellenmelidir. Bir kayıt güncellenirse, search-key attribute'u etkilenen her index de güncellenmelidir (örn. instructor'ın bölümü değişirse dept_name index'i güncellenir). Bir record update, eski kaydın silinmesi + yeni değerle eklenmesi olarak modellenir (index deletion + index insertion).

**Insertion (single-level):** eklenecek kaydın search-key value'su ile lookup yapılır.
- *Dense:* değer index'te yoksa uygun pozisyona eklenir; varsa (çoklu kayıt) index entry'ye yeni kayda pointer eklenir.

**Deletion (single-level):**
- *Dense:* silinen kayıt o search-key value'ya sahip tek kayıtsa, search-key index'ten de silinir (primary key değilse). Aksi halde: (a) entry tüm kayıtlara pointer tutuyorsa, silinen kayda pointer çıkarılır; (b) entry sadece ilk kayda pointer tutuyorsa ve silinen ilk kayıtsa, entry bir sonraki kayda işaret edecek şekilde güncellenir.
- *Sparse:* silinen kaydın search-key value'su için bir index entry yoksa, index'te bir şey yapmaya gerek yoktur.

### Secondary Indices
Secondary (non-clustering) index'ler **dense olmak zorundadır**: her search-key value için bir entry ve her kayda pointer. Clustering index sparse olabilir (ara değerler sequential erişimle bulunabilir), ama secondary index sadece bazı değerleri saklasaydı ara değerli kayıtlar file'da herhangi bir yerde olabileceğinden bulunamazdı — bu yüzden tüm kayıtlara pointer içermelidir.

**Nonunique search key:** birden fazla kayıt aynı search-key value'ya sahip olabiliyorsa, search key nonunique'tir. Secondary index'lerde nonunique key uygulaması: pointer'lar doğrudan kayda değil, bir **bucket**'a işaret eder; bucket da file'a pointer'lar içerir. (Örn. instructor'ın salary field'i üzerine secondary index.)

**Dezavantajlar:** ekstra indirection seviyesi nedeniyle erişim daha uzun sürer (random I/O gerekebilir); az/duplicate'siz key için bütün bir block bucket'a ayrılırsa çok alan boşa gider. Secondary index'ler clustering index'in search key'i dışındaki key'lerle yapılan query'lerin performansını artırır, ama database modification'a önemli overhead bindirir.

### Indices on Multiple Keys
**Composite search key:** birden fazla attribute içeren search key. Örn. `takes` relation'ı üzerinde `(course_id, semester, year)` composite key'i; belirli bir dönem/yılda belirli bir course'a kayıtlı tüm öğrencileri bulmak için kullanışlı.

### B+-Tree Index Files
**Indexed-sequential file'ın dezavantajı:** file büyüdükçe performans bozulur (çok sayıda overflow block oluşur), tüm file'ın periyodik reorganization'ı gerekir.

**B+-tree'nin avantajı:** insertion ve deletion karşısında küçük, lokal değişikliklerle kendini otomatik yeniden düzenler; performans için tüm file'ın reorganization'ı gerekmez. (Minör) dezavantaj: ekstra insertion/deletion overhead'i ve space overhead'i. Avantajları dezavantajlarına ağır bastığı için B+-tree'ler çok yaygın kullanılır.

**B+-tree özellikleri** (rooted tree, `n` = order/fan-out):
- Root'tan leaf'e tüm path'ler aynı uzunluktadır (**balanced**).
- Root ya da leaf olmayan her node'un `⌈n/2⌉` ile `n` arası child'ı vardır.
- Bir leaf node'un `⌈(n–1)/2⌉` ile `n–1` arası value'su vardır.
- Özel durumlar: root leaf değilse en az 2 child'ı vardır; root leaf ise (tek node) 0 ile `n–1` arası value tutabilir.

**Node Structure:** tipik node `P1, K1, P2, K2, ..., Kn-1, Pn` şeklindedir. `Ki` search-key value'lar; `Pi` non-leaf'te child pointer'ları, leaf'te record/bucket pointer'larıdır. Node içinde key'ler sıralıdır: `K1 < K2 < ... < Kn-1`. (Başta duplicate yok varsayılır.)

**Leaf Nodes:** `i = 1..n-1` için pointer `Pi`, search-key value `Ki` olan file record'una işaret eder. `Li, Lj` leaf'leri ve `i < j` ise `Li`'nin search-key value'ları `Lj`'ninkilerden küçük/eşittir. `Pn`, search-key order'da bir sonraki leaf node'a işaret eder (leaf'ler linked).

**Non-Leaf Nodes:** leaf'ler üzerinde multi-level sparse index oluştururlar. `m` pointer'lı bir node için: `P1`'in subtree'sindeki tüm key'ler `K1`'den küçük; `2 ≤ i ≤ n-1` için `Pi`'nin subtree'sindeki key'ler `Ki-1 ≤ key < Ki`; `Pn`'in subtree'sindeki key'ler `Kn-1`'den büyük/eşittir.

**Gözlemler:** B+-tree'ler balanced'dır (B = "balanced"). Pointer'larla bağlandığı için logically yakın block'lar physically yakın olmak zorunda değildir. Non-leaf seviyeler sparse index hiyerarşisi oluşturur. Ağaç görece az seviye içerir: `K` search-key value varsa ağaç yüksekliği en fazla `⌈log⌈n/2⌉(K)⌉`'dir; böylece arama verimli yapılır. Insertion/deletion logaritmik zamanda halledilir.

### Queries on B+-Trees
`find(v)` fonksiyonu:
```
1. C = root
2. while (C leaf değil)
     i = en küçük sayı öyle ki v ≤ C.Ki
     if böyle i yoksa: C = C'deki son non-null pointer
     else if (v = C.Ki): C = Pi+1
     else: C = C.Pi
3. bir i için Ki = v ise return C.Pi
4. else return null   /* v değerli record yok */
```
**Range queries:** verilen aralıktaki tüm kayıtları bulur (`findRange(lb, ub)`); gerçek implementasyonlar genelde `next()` ile birer birer kayıt getiren iterator interface sunar. *(Bu kısım kitap referansı — sınavda sorumlu değilsiniz.)*

**Yükseklik/maliyet:** `K` search-key value için yükseklik ≤ `⌈log⌈n/2⌉(K)⌉`. Bir node genelde bir disk block boyutundadır (tipik 4 KB); `n` (pointer, value) pair sayısı disk block için tipik ~100 (~40 byte/index entry). 1 milyon search key ve n=100 ile root'tan leaf'e lookup'ta en fazla `log50(1,000,000) ≈ 4` node erişilir. Karşılaştırma: 1 milyon key'li balanced binary tree'de ~20 node erişilir. Her node access bir disk I/O gerektirebileceğinden (≈20 ms) bu fark önemlidir. *(Bu slayt da kitap referansı — sınavda sorumlu değilsiniz.)*

### B+-Tree Insertion
Kayıt zaten file'a eklenmiştir. `pr` = kayda pointer, `v` = search key value.
1. Değerin gireceği leaf node'u bul.
2. Leaf'te yer varsa `(v, pr)` pair'i ekle.
3. Yer yoksa node'u (yeni entry ile birlikte) **split** et ve güncellemeleri parent'lara propagate et.

**Leaf split:** `n` adet (value, pointer) pair'ini (yeni dahil) sorted al; ilk `⌈n/2⌉` orijinal node'da, kalanı yeni node'da. Yeni node `p`, `p`'deki en küçük key `k` olsun; `(k, p)` parent'a eklenir. Parent doluysa o da split edilir, yukarı doğru propagate olur. Worst case'de root split olur ve ağaç yüksekliği 1 artar.

**Non-leaf split** (dolu internal node N'e `(k,p)` eklerken):
1. N'i `n+1` pointer ve `n` key için yerli in-memory alan M'e kopyala.
2. `(k,p)`'yi M'e ekle.
3. `P1,K1,...,K⌈n/2⌉-1,P⌈n/2⌉`'i M'den N'e geri kopyala.
4. `P⌈n/2⌉+1,K⌈n/2⌉+1,...,Kn,Pn+1`'i M'den yeni node N′'ye kopyala.
5. `(K⌈n/2⌉, N′)`'i parent'a ekle.

### B+-Tree Deletion *(kitap referansı — sınavda sorumlu değilsiniz)*
Kayıt file'dan silinmiştir. `(Pr, V)` leaf'ten çıkarılır.
- Node, silme sonucu çok az entry'ye düşer ve node + bir sibling tek node'a sığarsa → **merge**: tüm değerler soldaki node'a alınır, diğer node silinir; parent'tan `(Ki-1, Pi)` pair'i çıkarılır (recursive). (Örn. "Srinivasan" silinince under-full leaf'ler merge olur.)
- Sığmıyorsa → **redistribute**: node ile sibling arasında pointer'lar yeniden dağıtılır, her ikisi de minimum'dan fazla entry'ye sahip olur; parent'taki ilgili search-key value güncellenir. (Örn. "Singh" ve "Wu" silinince leaf under-full olur ve sol sibling'den "Kim" ödünç alınır; parent'taki ayırıcı değer değişir.) Silmeler yukarı doğru cascade olabilir; root'tan sonra tek pointer kalırsa root silinir, tek child yeni root olur. (Örn. "Gold" silinince node merge → parent under-full → o da merge → root tek child'lı kalır ve silinir; merge'de parent'taki ayırıcı değer aşağı çekilir.)

---

## 3) Indexing & B-Trees Part 2 (Chapter 14) — Hashing, Write-Optimized & Bitmap

### Hashing
Hashing, bir alfasayısal string'e önce başka bir numerik değere dönüştürerek bir numerik değer atama ve onu indexed table'da saklama işlemidir; veri erişimini hızlandırmak ve/veya encryption için veriyi maskelemek amacıyla, bir **hash function** ile yapılır.

Hashing, main memory'de index kurmak için yaygın kullanılır (örn. join işlemi için geçici olarak oluşturulan index'ler). File'daki kayıtları organize etmek için de kullanılmıştır, ama **hash file organization** çok yaygın değildir.

- **Bucket:** bir veya daha çok entry içeren depolama birimi (tipik olarak bir disk block). Bir entry'nin bucket id'si, search-key value'sundan bir hash function ile elde edilir.
- **Hash function h:** tüm search-key value kümesi K'den tüm bucket address kümesi B'ye bir fonksiyon. Erişim, ekleme ve silme için kullanılır.

### Static Hashing
Farklı search-key value'lar aynı bucket'a map'lenebilir; bu yüzden bir entry'yi bulmak için tüm bucket sequential aranır.
- *Insert:* `Ki` için `h(Ki)` hesaplanır (bucket address'i), kayıt o bucket'a eklenir.
- *Lookup:* `h(Ki)` hesaplanır, o address'teki bucket aranır. İki key aynı hash value'ya sahipse (`h(K5) = h(K7)`), bucket her ikisinin kayıtlarını içerir; istenen kaydı doğrulamak için bucket'taki her kaydın search-key value'su sequential kontrol edilir.
- *Delete:* `h(Ki)` ile bucket bulunur, kayıt aranıp silinir (linked list gösteriminde silme kolaydır).

İki kullanım:
- **Hash index:** bucket'lar kayıtlara pointer içeren entry'ler saklar.
- **Hash file-organization:** bucket'lar doğrudan kayıtları saklar.

Örnek: instructor file'ında `dept_name` key ile hash file organization. Secondary hash index örneği: search key ID için, hash function ID rakamlarının toplamının modulo 8'i; 8 bucket, her biri boyut 2; bir bucket'a 3 key map'lenince **overflow bucket** kullanılır. ID primary key olduğu için her key'in tek pointer'ı vardır (genelde çoklu olabilir).

### Handling of Bucket Overflows
Bir kayıt eklenirken bucket dolu ise **bucket overflow** oluşur. Çözüm: **overflow bucket**'lar. Kayıt `b` bucket'ına eklenecek ve `b` doluysa, sistem `b` için overflow bucket sağlar ve kaydı oraya ekler. **Overflow chaining:** bir bucket'ın overflow bucket'ları linked list halinde zincirlenir. Lookup'ta `h(k)` bucket'ı ile birlikte ona bağlı overflow bucket'lar da aranır.

**Overflow nedenleri:**
- Yetersiz bucket sayısı.
- Kayıt dağılımında **skew**: (i) birden çok kayıt aynı search-key value'ya sahip; (ii) seçilen hash function non-uniform dağılım üretiyor.

Overflow olasılığı azaltılabilir ama elimine edilemez; overflow bucket'larla halledilir.

### Deficiencies of Static Hashing
Static hashing'te `h`, search-key value'ları sabit B bucket address kümesine map'ler. Database zamanla büyür/küçülür. Seçenekler:
1. Mevcut file boyutuna göre hash function seç → database büyüdükçe performans bozulur.
2. Gelecekteki tahmini boyuta göre seç → performans bozulması önlenir ama başta önemli alan israfı olur.
3. File büyümesine göre hash yapısını periyodik reorganize et → yeni hash function seçilir, her kayıt için yeniden hesaplanır, yeni bucket atamaları üretilir; bu çok masraflı, zaman alıcı bir işlemdir.

**Daha iyi çözüm:** bucket sayısının ve hash function'ın dinamik değiştirilmesine izin vermek (database büyüme/küçülmesine uyum için).

### Index Definition in SQL
```sql
create index <index-name> on <relation-name> (<attribute-list>)
-- örn:
create index b-index on branch(branch_name)
```
`create unique index` ile search key'in candidate key olması dolaylı olarak belirtilip zorlanır (SQL unique constraint destekleniyorsa gerekli değildir). Drop: `drop index <index-name>`.

Örnek: `create index takes_pk on takes (ID, course_ID, year, semester, section)` / `drop index takes_pk`. Çoğu database index tipi ve clustering belirtmeye izin verir. Primary key üzerine index'ler tüm database'lerce **otomatik** oluşturulur; bazıları foreign key attribute'ları üzerine de oluşturur. Index'ler lookup'ı çok hızlandırır ama update'lere maliyet bindirir. **Index tuning assistant/wizard**'lar, query ve update workload'una göre index seçimine yardımcı olur.

### Write-Optimized Indices
B+-tree'nin dezavantajı: **random write**'larda performans düşük olabilir; write-intensive workload'lar için zayıf. Leaf başına bir I/O (tüm internal node'lar memory'de varsayılır). Index memory'ye sığmıyorsa ve write/insert'ler index'in sort order'ına uymayan sırada yapılırsa, her write farklı bir leaf node'a dokunur; leaf sayısı buffer'dan çok büyükse bunların çoğu random read + sonrasında write gerektirir. Yani temel B+-tree, saniyede çok sayıda random write/insert gereken uygulamalar için ideal değildir.

**Write maliyetini azaltan iki yaklaşım:** Log-Structured Merge (LSM) tree ve Buffer tree.

### Log-Structured Merge (LSM) Tree
Bir LSM tree, birkaç B+-tree'den oluşur: in-memory tree **L0** ile başlar, diskte **L1, L2, ..., Lk** tree'leri vardır (`k` = level). Index lookup, her tree (L0..Lk) üzerinde ayrı lookup yapıp sonuçları merge ederek gerçekleştirilir.

**Insert akışı:**
- Kayıtlar önce in-memory L0 tree'ye eklenir; tree, kendisine ayrılan memory dolana dek büyür.
- Dolunca data in-memory yapıdan diske bir B+-tree'ye taşınır. L1 boşsa, tüm L0 diske yazılarak ilk L1 oluşturulur.
- L1 boş değilse, L0'ın leaf seviyesi artan key order'da taranır ve L1'in leaf entry'leriyle merge edilir; **bottom-up build** ile oluşturulan yeni B+-tree eski L1'i değiştirir. Her iki durumda da, taşıma sonrası L0 ve (varsa) eski L1 silinir; insert'ler artık boş L0'a yapılır.
- L1 bir eşiği aşınca L2'ye merge edilir, vb. `Li+1` için boyut eşiği, `Li` eşiğinin `k` katıdır.

**Faydalar:** insert'ler yalnızca sequential I/O ile yapılır; leaf'ler doludur (alan israfı yok); normal B+-tree'ye göre kayıt başına I/O sayısı azalır (belli bir boyuta kadar). **Dezavantajlar:** query'ler birden çok tree'yi aramak zorunda; her seviyenin tüm içeriği birden çok kez kopyalanır.

**Deletion:** özel "delete" entry'leri eklenerek halledilir. Lookup hem orijinal entry'yi hem delete entry'yi bulur ve yalnızca eşleşen delete entry'si olmayanları döndürür. Tree'ler merge edilirken, orijinal ile eşleşen bir delete entry bulunursa ikisi de düşürülür. **Update** = insert + delete.

LSM tree'ler disk-tabanlı index'ler için tanıtılmıştır. **Stepped-merge** varyantı birçok Big Data depolama sisteminde kullanılır: Google BigTable, Apache HBase, Apache Cassandra, Apache AsterixDB, MongoDB; ayrıca SQLite4, LevelDB ve MySQL'in MyRocks storage engine'i.

### Buffer Tree
LSM tree'ye alternatif. **Temel fikir:** B+-tree'nin her internal node'una insert'leri saklayan bir **buffer** eklemek.
- Insert'te, leaf'e kadar inmek yerine kayıt root'un buffer'ına eklenir. Buffer dolunca, içindeki her kayıt bir seviye aşağı, uygun child'a itilir. Child internal node ise kayıt onun buffer'ına eklenir (o da doluysa benzer şekilde aşağı itilir). Child leaf node ise kayıtlar normal şekilde leaf'e eklenir; leaf overfull olursa normal B+-tree split'i yapılır, split parent'lara propagate olabilir.
- **Lookup:** B+-tree normal şekilde gezilir, ama ek olarak gezilen her internal node'da buffer da, lookup key ile eşleşen kayıt olup olmadığına bakılarak incelenir.

**Faydalar:** query'lerde daha az overhead; herhangi bir tree index yapısıyla kullanılabilir; PostgreSQL'in **GiST (Generalized Search Tree)** index'lerinde kullanılır; kayıt başına I/O buna karşılık azalır. **Dezavantaj:** LSM tree'ye göre daha fazla random I/O.

### Bitmap Indices
Bitmap index, birden çok key üzerinde verimli sorgulama için tasarlanmış özel bir index türüdür (gerçi her bitmap index tek bir key üzerine kurulur). Kayıtların 0'dan başlayarak sequential numaralandırıldığı varsayılır; verilen `n` için record `n` kolayca getirilebilmelidir (kayıtlar fixed size ve consecutive block'larda ise çok kolay). **Görece az sayıda distinct value alan attribute'lara** uygulanır (örn. gender, country, state; ya da income-level: 0-9999, 10000-19999, 20000-50000, 50000-∞ gibi az sayıda seviye).

**Bitmap** = bit array'i. En basit formda bir attribute üzerindeki bitmap index, attribute'un her distinct value'su için bir bitmap içerir. Bir bitmap, kayıt sayısı kadar bit'e sahiptir. Value `v` için bitmap'te, kayıt o attribute'ta `v` değerine sahipse ilgili bit 1, değilse 0'dır.

**Ne zaman faydalı?** Tek bir value'ya (örn. tüm 'm' kayıtları) sahip kayıtları getirmek için bitmap pek hızlandırmaz — muhtemelen file'ın her disk block'u yine de okunur. Bitmap index'ler asıl **birden çok key üzerindeki selection**'larda faydalıdır. Örnek: gender üzerine bitmap'e ek olarak income_level üzerine de bitmap olsun. "Geliri 10,000–19,999 aralığındaki kadınlar" query'si için gender = f bitmap'i ile income_level = L2 bitmap'i fetch edilir ve **intersection (logical-AND)** yapılır:
```
gender = f       : 01101
income_level = L2: 01000
AND               -> 01000
```
İlk attribute 2, ikincisi 5 değer alabildiğinden, ortalama ~1/10 kaydın birleşik koşulu sağlaması beklenir.

**Bitmap operasyonları** (aynı boyuttaki iki bitmap üzerinde, karşılık gelen bitlere):
```
100110 AND 110011 = 100010
100110 OR  110011 = 110111
NOT 100110         = 011001
Males with income level L1: 10010 AND 10100 = 10000
```
Eşleşen tuple'lar sonra getirilebilir; eşleşen tuple sayısını **saymak** daha da hızlıdır. Özetle bitmap index'ler multiple-attribute query'lerde faydalı, single-attribute query'lerde pek değildir.

---

## 4) Application Development (Chapter 9)

### Application Programs ve User Interfaces
Kullanıcılar database ile en sık bir **application program** üzerinden etkileşir: front-end'de user interface, back-end'de database. Application program, kullanıcılar ile database arasında aracıdır. Uygulamalar üçe ayrılır:
- **front-end:** user interface (form'lar, GUI; çoğu Web-based).
- **middle layer:** "business logic" içerir.
- **backend:** database ile iletişir.

### Application Architecture Evolution
Üç (dört) dönem: **Mainframe** (1960-70'ler), **Personal Computer** (1980'ler), **Web** (1990'ların ortasından itibaren), **Web ve Smartphone** (2010'dan itibaren). Mainframe döneminde kullanıcı makineleri database'e doğrudan erişiyordu — güvenlik riski. HTML standardı OS ve browser'dan bağımsızdır, kurulum gerektirmez. Browser'dan gelen request'ler web server'a gider, server da request'i işlemek için bir application program çalıştırır.

### Web Interface
Web arayüzü: çok sayıda kullanıcının her yerden database'e erişmesini sağlar; özel kod indirme/kurma ihtiyacını ortadan kaldırırken iyi bir GUI sunar. JavaScript, Flash gibi scripting dilleri browser'da çalışır ama transparan biçimde indirilir. Örnekler: bankalar, havayolu/araç kiralama rezervasyonu, üniversite kayıt ve not sistemleri. Web browser'lar database'lere de-facto standart user interface haline gelmiştir.

### The World Wide Web
Web, **hypertext** tabanlı dağıtık bir information system'dir. Çoğu Web document'ı **HTML (HyperText Markup Language)** ile formatlanmış hypertext document'tır. HTML document'lar şunları içerir: font/format talimatlarıyla text; diğer document'lara hypertext link'ler (text bölgeleriyle ilişkilendirilebilir); kullanıcıların veri girip Web server'a gönderebildiği **form**'lar.

### URL (Uniform Resource Locator)
Web'de pointer işlevini URL'ler sağlar. Örn. `http://www.acm.org/sigmod`:
- İlk kısım document'a nasıl erişileceğini gösterir; "http" = HyperText Transfer Protocol.
- İkinci kısım Internet'teki makinenin benzersiz adı.
- Geri kalanı document'ı makine içinde tanımlar: ya bir file'ın path name'i ya da bir programın identifier'ı + programa geçirilecek argument'lar. Örn. `http://www.google.com/search?q=silberschatz`.

### HTML ve HTTP
HTML format, hypertext link ve image display özellikleri sunar (table, stylesheet vb.). Ayrıca input özellikleri: bir seçenek kümesinden seçim (pop-up menu, radio button, check list), değer girme (text box). Doldurulan input server'a geri gönderilir ve server'daki bir executable tarafından işlenir. Web server ile iletişim **HTTP (HyperText Transfer Protocol)** ile yapılır.

Örnek HTML form kaynağı:
```html
<html><body>
  <table border>
    <tr><th>ID</th><th>Name</th><th>Department</th></tr>
    <tr><td>00128</td><td>Zhang</td><td>Comp. Sci.</td></tr>
  </table>
  <form action="PersonQuery" method=get>
    Search for:
    <select name="persontype">
      <option value="student" selected>Student</option>
      <option value="instructor">Instructor</option>
    </select><br>
    Name: <input type=text size=20 name="name">
    <input type=submit value="submit">
  </form>
</body></html>
```

### Web Servers
**Web server**, server makinesinde çalışan, web browser'dan request kabul edip sonuçları HTML document olarak geri gönderen programdır; iletişim HTTP'dir. URL'deki document adı bir executable program'ı tanımlayabilir; çalıştığında HTML document üretir. HTTP server böyle bir request alınca programı çalıştırır ve üretilen HTML'i geri yollar. Web client, document adıyla ekstra argument geçirebilir.

### Three-Layer ve Two-Layer Web Architecture
**Three-layer:** **CGI (Common Gateway Interface)** standardı, web server'ın application program'larla nasıl iletişeceğini tanımlar. Application program, veri almak/saklamak için ODBC, JDBC veya başka protokollerle database server ile iletişir. Çok seviyeli server kullanmak sistem overhead'ini artırır; CGI her request için yeni bir process başlatır → daha da büyük overhead.

**Two-layer:** indirection'ın overhead'i nedeniyle çoğu web uygulaması bugün two-layer mimari kullanır: web ve application server'lar tek bir server'da birleştirilir. Client ile web server arasında sürekli bağlantı yoktur; request gelince geçici bir connection oluşturulur, response alınır, sonra kapatılabilir; bir sonraki request yeni bir connection'dan gelebilir.

### HTTP and Sessions
HTTP protokolü **connectionless**'tır: server bir request'e yanıt verince connection'ı kapatır ve request'i tamamen unutur. Buna karşılık Unix login'leri ve JDBC/ODBC connection'ları client disconnect edene dek bağlı kalır (user authentication vb. bilgiyi korur). **Motivasyon:** server yükünü azaltmak (OS'lerin açık connection sayısında sıkı limitleri vardır). Ama information service'ler **session** bilgisine ihtiyaç duyar (örn. authentication oturum başına bir kez). **Çözüm: cookie.**

### Sessions and Cookies
**Cookie**, identifying bilgi içeren küçük bir text parçasıdır. Server'dan browser'a gönderilir (ilk etkileşimde, session'ı tanımlamak için); sonraki etkileşimlerde browser cookie'yi onu yaratan server'a geri yollar — HTTP protokolünün parçasıdır. Server, çıkardığı cookie'ler hakkında bilgi saklar ve request'leri sunarken kullanır (örn. authentication bilgisi, kullanıcı tercihleri). Cookie'ler kalıcı veya sınırlı süreli saklanabilir.

### Servlets
**Java Servlet** spesifikasyonu, Web/application server ile server'da çalışan application program arasında iletişim için bir API tanımlar (Web form'larından parametre alma, client'a HTML gönderme metodları). Application program (servlet) server'a yüklenir. "Servlet" genelde servlet interface'ini implemente eden bir Java program/class'ı için kullanılır. Servlet'ler HTTP request'lere dinamik yanıt üretmek için yaygındır: HTML form input'larını alır, business logic uygular ve geri gönderilecek HTML üretir.

Servlet kodu web/application server'da çalışır. Her HTTP request server'da yeni bir **thread** doğurur; request servis edilince thread kapanır. Programcı `HttpServlet`'ten türeyen bir class yazar ve `doGet`, `doPost` gibi metodları override eder. Servlet adı → servlet class eşlemesi `web.xml` dosyasında yapılır (çoğu IDE otomatik yapar).

Örnek servlet:
```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
public class PersonQueryServlet extends HttpServlet {
  public void doGet(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    response.setContentType("text/html");
    PrintWriter out = response.getWriter();
    out.println("<HEAD><TITLE> Query Result</TITLE></HEAD>");
    out.println("<BODY>");
    String persontype = request.getParameter("persontype");
    String number = request.getParameter("name");
    if (persontype.equals("student")) {
      // JDBC ile öğrencileri bul, tabloya yaz
    } else {
      // instructor'lar için
    }
    out.println("</BODY>");
    out.close();
  }
}
```

**Servlet Support:** servlet'ler Apache Tomcat, Glassfish, JBoss, BEA Weblogic, IBM WebSphere, Oracle Application Server gibi application server'larda çalışır. Application server'lar deployment/monitoring sağlar. **Jakarta EE (J2EE)** platformu object'ler ve birden çok application server üzerinde paralel işleme destekler. Geliştirme için en iyisi Tomcat/Glassfish gömülü gelen Eclipse veya NetBeans gibi IDE'lerdir.

### Server-Side Scripting
Java Servlet'lere alternatif olarak scripting dilleri ve web application framework'leri vardır (örn. Python için). Server-side scripting, database'i Web'e bağlamayı kolaylaştırır (Java/C ile bu zaman alıcıdır). Mantık: embedded executable code / SQL query içeren bir HTML document tanımlanır; HTML form'larından gelen input değerleri doğrudan embedded code/query'de kullanılır; document istendiğinde web server embedded kodu çalıştırıp gerçek HTML'i üretir. Diller: **JSP, PHP**; genel amaçlı: VBScript, Perl, Python.

**JSP (Java Server Pages):** embedded Java kodu içeren sayfa; static HTML ile dinamik üretilen HTML karıştırılabilir. JSP, Java + Servlet'e derlenir (JSP script'leri servlet koduna çevrilip derlenir).
```html
<html><head><title> Hello </title></head><body>
<% if (request.getParameter("name") == null)
     { out.println("Hello World"); }
   else { out.println("Hello, " + request.getParameter("name")); }
%>
</body></html>
```

**PHP:** server-side scripting için yaygın; ODBC ile database erişimi dahil geniş library'ler.
```html
<html><head><title> Hello </title></head><body>
<?php if (!isset($_REQUEST['name']))
        { echo "Hello World"; }
      else { echo "Hello, " . $_REQUEST['name']; }
?>
</body></html>
```

**JavaScript:** çok yaygın; zengin user interface sunan yeni nesil web uygulamalarının (**Web 2.0**) temelidir. JavaScript fonksiyonları: input'u geçerlilik için kontrol eder; gösterilen sayfayı, altta yatan **DOM (Document Object Model)** ağacını değiştirerek günceller; sayfayı reload etmeden Web server ile veri alışverişi yapıp sayfayı günceller — **AJAX** teknolojisinin temeli (örn. drop-down'da ülke seçilince ilgili eyaletler otomatik dolar).
```html
<html><head><script type="text/javascript">
function validate() {
  var credits = document.getElementById("credits").value;
  if (isNaN(credits) || credits <= 0 || credits >= 16) {
    alert("Credits must be a number greater than 0 and less than 16");
    return false;
  }
}
</script></head>
<body><form action="createCourse" onsubmit="return validate()">
  Title: <input type="text" id="title" size="20"><br/>
  Credits: <input type="text" id="credits" size="2"><br/>
  <input type="submit" value="Submit">
</form></body></html>
```

### Application Architectures
Büyük uygulamalar karmaşıklığı yönetmek için katmanlara ayrılır:

- **Presentation / user-interface layer:** kullanıcı etkileşimini ele alır. Bir uygulamanın farklı arayüz versiyonları olabilir (web browser, küçük ekranlı mobil). Bu katman genelde **MVC (Model-View-Controller)** mimarisine göre kendi içinde de katmanlara ayrılır.
- **Business-logic layer (MODEL):** data ve data üzerindeki action'ların yüksek seviyeli görünümünü sağlar (genelde object data model); data storage schema detaylarını gizler.
- **Data access layer (VIEW):** business-logic layer ile database arasında arayüz; business layer'ın object model'ini database'in relational model'ine map'ler.
- **Controller:** event'leri (user action) alır, model üzerinde action'ları yürütür ve kullanıcıya bir view döndürür.

**Business Logic Layer:** entity soyutlamaları sağlar (student, instructor, course); action'lar için **business rule**'ları zorlar (örn. öğrenci, prerequisite'leri tamamlayıp harcını ödediyse derse kayıt olabilir); birden çok katılımcılı bir görevin nasıl yürütüleceğini tanımlayan **workflow**'ları destekler (adım dizisi, error handling — örn. tavsiye mektupları zamanında gelmezse ne yapılacağı).

**Request işleme adımları (MVC akışı):** request application server'a ulaşır → controller model'e request gönderir → model business logic ile request'i işler (model object'leri güncelleyip bir result object oluşturabilir) → model, database'i güncellemek/okumak için data-access layer'ı kullanır → oluşturulan result object view modülüne gönderilir → view, sonucun browser'da gösterilecek HTML görünümünü oluşturur → view, sonucu görmek için kullanılan cihazın özelliklerine göre uyarlanabilir.

---

## 5) Big Data (Chapter 10)

### Motivation
1990-2000'lerde Web'in büyümesi, relational database'lerin yönetmek için tasarlandığı enterprise data'yı çok aşan hacimlerde veri saklama/sorgulama ihtiyacı doğurdu. Modern uygulamalar genelde relational olmayan verilerle de uğraşır ve tek bir kuruluşun üreteceğinden çok daha büyük hacimlerle: **BIG DATA**. Çok büyük hacimli veri toplanıyor — web, sosyal medya ve son dönemde **internet-of-things** etkisiyle. Erken kaynak **web log**'lardı (textual): sitelere kimin girdiği, hangi sayfalara ne zaman eriştiği. Web log analytics'i reklam, site yapılandırma, kullanıcıya hangi gönderinin gösterileceği için büyük değer taşır.

**Big Data, geleneksel relational database'lerle 3 V üzerinden karşılaştırılır:**
- **Volume:** geleneksel database'lerden çok daha büyük veri saklanır/işlenir.
- **Velocity:** çok daha yüksek ekleme oranları; bugünün ağlı dünyasında veri varış hızı çok yüksektir, sistemler veriyi çok yüksek hızda ingest edip saklayabilmelidir.
- **Variety:** relational'ın ötesinde birçok veri tipi (semi-structured, textual, graph data).

"Big Data" terimi, veri relational olsun olmasın, yüksek derecede **parallelism** gerektiren herhangi bir veri işleme ihtiyacı için genel anlamda kullanılır. Son on yılda binlerce (bazen on binlerce) makineli çok büyük cluster'larla Big Data depolama/işleme sistemleri geliştirildi. Cluster'daki bir makineye **node** denir.

**Diğer yüksek-hacimli veri kaynakları:** mobil app verileri (kullanıcı etkileşimini anlamak için, tıpkı web click'leri gibi); retail enterprise transaction verileri (örn. Walmart, web öncesinde bile parallel database kullanıyordu); sensör verileri (ekipman sağlığını izlemek, arıza öncesi sorun tahmini; **internet of things** — sensörlerin araç, bina, makine vb. içine gömülüp internete bağlanması; sayıları artık internetteki insan sayısından fazla); iletişim ağlarından metadata (trafik/monitoring, çağrı bilgisi — sorun tespiti ve capacity planning için).

### Querying Big Data
Big Data uygulamaları sıklıkla çok büyük hacimde text, image, video verisi işler. Geleneksel olarak bu veri file system'lerde saklanır ve stand-alone uygulamalarla işlenirdi (DBMS öncesi organizasyonel veri işleme gibi). SQL construct'ları bu görevlere uygun değildir çünkü input/output relational formda değildir. SQL relational database sorgulamada açık ara en yaygın dildir, ancak Big Data için 3 V'yi ele alan daha geniş bir query language seçeneği yelpazesi vardır. Büyük volume/velocity'ye ölçeklenen sistemler **parallel storage ve processing** gerektirir. Veri boyutlarının çok hızlı büyümesiyle stand-alone programların sınırları belirginleşti; failure'lar (büyük ölçekli paralellikte yaygın) ile başa çıkarken paralel işleyen program yazmak kolay değildir.

### Big Data Storage Systems
Popüler uygulamaların yüz milyonlarca kullanıcısı vardır; yük tek yılda kat kat artabilir. Geliştirilen yaklaşımlar:

- **Distributed file systems:** file'ların birçok makineye yayılarak saklanmasına izin verir, geleneksel file-system interface'i ile erişim sağlar. Büyük file'lar (örn. log file'lar) için kullanılır.
- **Sharding across multiple databases:** kayıtların birden çok sistem arasında bölünmesi (partitioning).
- **Key-value storage systems:** kayıtları bir key'e göre saklayıp getirir, sınırlı query imkânı sunabilir. Tam teşekküllü database değildir; SQL desteklemedikleri için sıklıkla **NoSQL** denir.
- **Parallel and distributed databases:** geleneksel database interface'i sunar ama veriyi birden çok makineye yayar ve query processing'i paralel yapar.

Bu tekniklerin başarısının anahtarı, karmaşık veri işleme görevlerinin belirtilmesine izin verirken görevlerin kolay paralelleştirilmesini sağlamalarıdır. Programcıyı parallelization, failure handling, makineler arası load imbalance gibi düşük seviye sorunlarla uğraşmaktan kurtarırlar.

### Distributed File Systems
Distributed file system veriyi çok sayıda makineye yayar ama tek bir file-system görünümü sunar. File name ve directory sistemi vardır; client'lar file'ların nerede saklandığını bilmek zorunda değildir. Çok büyük veri ve çok sayıda eşzamanlı client destekler. Web page, web server log, image gibi büyük file olarak saklanan **unstructured data** için idealdir.

Yüksek ölçeklenebilir (örn. 10K node, 100 milyon file, 10 PB). Veri makinelere yayılır; **file'lar block'lara bölünür**; bir file'ın block'ları birden çok makineye partition edilir; her file block, makine arızası file'ı erişilmez kılmasın diye birden çok (tipik **üç**) makineye **replicate** edilir. Örnekler: **Google File System (GFS)**, **Hadoop File System (HDFS)** (open source).

**HDFS — master/slave mimari:** bir cluster tek bir **NameNode** (master server; file system namespace'i yönetir ve client erişimini düzenler) ve genelde node başına bir **DataNode** (üzerinde çalıştıkları node'a bağlı storage'ı yönetir) içerir.
- **NameNode:** filename'i Block ID listesine map'ler; her Block ID'yi, block'un bir replica'sını içeren DataNode'lara map'ler.
- **DataNode:** Block ID'yi diskteki fiziksel konuma map'ler.

Distributed file system'ler milyonlarca büyük file için iyidir, ama milyarlarca küçük tuple ile çok yüksek overhead ve zayıf performans gösterir. **HDFS mimarisi:** tüm cluster için tek namespace; file'lar block'lara bölünür (tipik **64 MB** block size); her block birden çok DataNode'a replicate edilir; client block konumunu NameNode'dan bulur ve veriye doğrudan DataNode'dan erişir.

### Sharding
Tek bir database genelde bir kuruluşun tüm transaction ihtiyacına yeter. Ama milyonlarca/milyarlarca kullanıcılı uygulamalar (sosyal medya, büyük bankalar) için tek database yetmez. Yaygın çözüm: veriyi birden çok database'e **partition** etmek, her database'e bir kullanıcı alt kümesi atamak. **Sharding** = verinin birden çok database/makineye partition edilmesi.

Partitioning genelde **partitioning attribute**'ları (partitioning/shard key, örn. user ID) üzerinden yapılır. Örn. key 1–100,000 database 1'de, 100,001–200,000 database 2'de. Uygulama hangi kaydın hangi database'de olduğunu takip edip query/update'i o database'e göndermelidir.
- **Artıları:** iyi ölçeklenir, uygulaması kolay.
- **Dezavantajları:** transparent değildir (uygulama query routing'i ve birden çok database'i kapsayan query'leri yönetmek zorunda); bir database overload olunca yükün bir kısmını taşımak kolay değil; daha çok database = daha çok failure şansı; availability için replica tutmak gerekir (uygulamaya ek iş). Key-value store'lar bu sorunların bazılarını çözer.

### Key-Value Storage Systems
Birçok web uygulaması çok sayıda görece küçük kayıt saklamak zorundadır. File system'ler (distributed dahil) bu kadar çok file saklamak için tasarlanmamıştır. İdeal olarak massively parallel relational database kullanılmalı, ama standart database özelliklerini destekleyerek çok sayıda makinede paralel çalışan relational sistem kurmak kolay değildir. **Key-value store**'lar çok sayıda (milyarlarca+) küçük (KB-MB) kayıt saklar.

**Key-value store:** bir kaydı (value) bir key ile saklayıp/güncelleyip, verilen key ile getirme imkânı verir. Kayıtlar birden çok makineye partition edilir, query'ler sistem tarafından uygun makineye route edilir. Kayıtlar makineler arası replicate edilir (availability için). Update'lerin tüm replica'lara uygulanmasıyla değerlerin **consistent** olması sağlanır. İhtiyaç oldukça makine eklenip yükün otomatik dengelenmesini sağlarlar; bu yüzden parallel key-value store'lar bugün sharding'den daha yaygındır.

**Sakladıkları:**
- **Wide-table** (rastgele çok sayıda attribute name) + key: Google BigTable, Apache Cassandra, Apache HBase, Amazon DynamoDB; bazı işlemlerin (örn. filtering) storage node'da çalışmasına izin verir.
- **JSON / document model:** MongoDB, CouchDB. Document store'lar semi-structured (tipik JSON) veri saklar. Bazıları timestamp/version number ile verinin çoklu versiyonunu destekler.

**Operasyonlar:** `put(key, value)`, `get(key)`, `delete(key)`. Bazıları key value'lar üzerinde range query destekler. Key-value store'lar tam database değildir: transactional update desteği yok/sınırlı; query processing'i uygulama kendi yönetmeli. Bu özellikleri desteklememek, ölçeklenebilir depolama kurmayı kolaylaştırır. **NoSQL** sistemleri de denir.

### Parallel and Distributed Databases
Parallel database'ler birden çok makinede (cluster) çalışır. 1980'lerde, Big Data'dan çok önce geliştirildiler; daha küçük ölçek için tasarlandılar (10-100 makine) ve kolay ölçeklenebilirlik sunmadılar. Makine arızasına rağmen veri availability'si için **replication** kullanılır, ama tipik olarak failure durumunda query yeniden başlatılır. Çok büyük ölçekte restart sık olabilir. **Map-reduce** sistemleri ise failure'ları atlayarak query execution'a devam edebilir.

### Replication and Consistency
Parallel/distributed database'ler için **availability** (parçalar arızalansa da sistemin çalışması) esastır; replication ile sağlanır (bir node arızalansa başka kopya hazır). Replicate edilen veride **consistency** önemlidir: tüm canlı replica'lar aynı değere sahip ve her read en son versiyonu görür. Genelde **majority protocol**'leriyle uygulanır (örn. 3 replica ile read/write 2 replica'ya erişmeli).

### The MapReduce Paradigm
Güvenilir, ölçeklenebilir parallel computing için bir platform. Dağıtık/paralel ortamın sorunlarını programcıdan soyutlar. Programcı çekirdek mantığı `map()` ve `reduce()` fonksiyonlarıyla sağlar; sistem parallelization, koordinasyon vb. ile ilgilenir. Paradigma onlarca yıl eski olsa da, 10³–10⁴ makineli cluster'larda çok büyük ölçekli implementasyonları daha yenidir (Google MapReduce, Hadoop). Veri saklama/erişimi tipik olarak distributed file system veya key-value store ile yapılır.

**Word Count örneği:** büyük bir document koleksiyonunda her kelimenin geçme sayısını saymak. Paralel çözüm: document'ları worker'lara böl; her worker document'ı parse eder, `map` fonksiyonu `(word, count)` pair'leri çıkarır; `(word, count)` pair'leri word'e göre worker'lara partition edilir; her word için worker'da `reduce` fonksiyonu count'ları lokal toplar.

Girdi `"One a penny, two a penny, hot cross buns."` için `map()` çıktısı: `("One",1),("a",1),("penny",1),("two",1),("a",1),("penny",1),("hot",1),("cross",1),("buns",1)`. `reduce()` çıktısı: `("One",1),("a",2),("penny",2),("two",1),("hot",1),("cross",1),("buns",1)`.

Pseudo-code:
```
map(String record):
  for each word in record
    emit(word, 1);
  // emit'in ilk attribute'u "reduce key"dir.
  // reduce key üzerinde group by yapılır -> her grup için value listesi oluşur.
  // Bu, makineler arası bir "shuffle" adımı gerektirir.

reduce(String key, List value_list):
  String word = key;
  int count = 0;
  for each value in value_list:
    count = count + value;
  Output(word, count);
```

**MapReduce Programming Model:** functional programming'deki (Lisp) map ve reduce'tan esinlenmiştir. Input: key/value pair kümesi. Kullanıcı iki fonksiyon verir: `map(k,v) → list(k1,v1)` ve `reduce(k1, list(v1)) → v2`. `(k1,v1)` ara (intermediate) key/value pair'idir. Çıktı `(k1,v2)` pair kümesidir. Word count örneğinde sistem file'ları satırlara böler ve `map`'i her satırın value'su ile çağırır; key satır numarasıdır.

**Örnek 2 — Log Processing:** verilen log formatında (`2013/02/21 10:31:22.00EST /slide-dir/11.ppt` ...), `/slide-dir/` dizinindeki her file'a 2013/01/01–2013/01/31 arası kaç kez erişildiğini bul. Seçenekler: sequential program çok yavaş; database'e yüklemek pahalı (log üzerinde doğrudan işlem daha ucuz); özel paralel program mümkün ama çok zahmetli; **map-reduce** paradigması uygun.
```
map(String key, String record) {
  // record'u space'e göre token'lara ayır
  String date = attribute[0];
  String time = attribute[1];
  String filename = attribute[2];
  if (date between 2013/01/01 and 2013/01/31
      and filename starts with "/slide-dir/")
    emit(filename, 1);
}                                  // her record için çağrılır
reduce(String key, List recordlist) {
  String filename = key;
  int count = 0;
  for each record in recordlist
    count = count + 1;
  output(filename, count);
}                                  // her partition için çağrılır
```

### Hadoop MapReduce
Google, binlerce node'da çalışabilen ve makine arızalarını transparan yöneten map-reduce implementasyonlarına öncülük etti. **Hadoop**, Java ile yazılmış, yaygın kullanılan open-source MapReduce implementasyonudur. Map/reduce fonksiyonları farklı dillerde yazılabilir (derste Java). Input/output paralel yapılmalıdır. Google GFS kullanırken Hadoop **HDFS** kullanır. Input file formatları: Text/CSV; sıkıştırılmış gösterimler **Avro, ORC, Parquet**. Hadoop ayrıca HBase, Cassandra, MongoDB gibi key-value store'ları da destekler.

---

## Hızlı Tekrar — Anahtar Karşılaştırmalar

- **Dense vs Sparse index:** Dense her search-key value için entry tutar (hızlı ama büyük); Sparse bazı değerler için entry tutar (küçük, az maintenance, ama daha yavaş; sıralı dosya gerektirir).
- **Clustering (primary) vs Secondary index:** Clustering file'ın fiziksel sırasını belirler ve sparse olabilir; Secondary farklı sıra belirtir ve **dense olmak zorundadır** (nonunique key'de bucket kullanır).
- **B+-tree vs B-tree:** B = balanced; B+-tree'de tüm value'lar leaf'te, leaf'ler linked (range query'ye uygun).
- **Ordered index (B+-tree) vs Hash index:** Ordered range query'de iyi; Hash point lookup'ta iyi ama range'de değil.
- **B+-tree vs LSM/Buffer tree:** B+-tree random write'ta zayıf; LSM (sequential I/O, çoklu tree lookup) ve Buffer tree (internal node buffer'ları) write-optimized.
- **Bitmap index:** az distinct value'lu attribute'larda ve **multiple-key** selection'da (AND/OR/NOT) güçlü; single-key'de değil.
- **3 V (Big Data):** Volume, Velocity, Variety.
- **Sharding vs Key-value store:** Sharding manuel partition (transparent değil); key-value store otomatik partition/replication/load balancing (NoSQL).
- **Three-layer vs Two-layer web:** Three-layer ayrı web+app+db (CGI, fazla overhead); Two-layer web+app birleşik (modern standart, connectionless HTTP + cookie).
- **MVC:** Model (business logic), View (data access/sonuç gösterimi), Controller (event'leri yönetir).

--

## Babadan notlar
- RDF => (ID1, ID2, ID3) (yani triple)
- SPARQL => RDF için özelleşmiş query altyapısı
- Spatial data => lokasyon dataları
- TF => Bir text'in dökümanda bulunma sıklığı
- IDF => TF'nin tersi
- Dense index => [id_0 -> row_0], [id_1 -> row_1] ...
- Sparse index => [id_0 -> block_0], [id_1 -> block_1] while block_0, block_1, ... consists of rows like [row_0, row_1, ...]
- Multilevel index (sparse'in sparse'i gibi) => [id_0 -> block_of_blocks_0], ... while [block_of_blocks_0 -> block_0] and blocks consist of rows like [row_0, row_1, ...]
- Secondary indeksler dense olmak zorunda (*)
