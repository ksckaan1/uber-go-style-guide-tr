<!--

Editing this document:

- Discuss all changes in GitHub issues first.
- Update the table of contents as new sections are added or removed.
- Use tables for side-by-side code samples. See below.

Code Samples:

Use 2 spaces to indent. Horizontal real estate is important in side-by-side
samples.

For side-by-side code samples, use the following snippet.

~~~
<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
BAD CODE GOES HERE
```

</td><td>

```go
GOOD CODE GOES HERE
```

</td></tr>
</tbody></table>
~~~

(You need the empty lines between the <td> and code samples for it to be
treated as Markdown.)

If you need to add labels or descriptions below the code samples, add another
row before the </tbody></table> line.

~~~
<tr>
<td>DESCRIBE BAD CODE</td>
<td>DESCRIBE GOOD CODE</td>
</tr>
~~~

-->

# Uber Go Style Guide Türkçe

## İçerikler Tablosu

- [Giriş](#giriş)
- [Yönergeler](#yönergeler)
    - [Arabirimlere Yönelik İşaretçiler (Pointer to Interaces)](#arabirimlere-yönelik-i̇şaretçiler-pointer-to-interaces)
    - [Arabirim Uyumluluğunu Doğrulayın](#arabirim-uyumluluğunu-doğrulayın)
    - [Alıcılar ve Arabirimler (Receivers and Interfaces)](#alıcılar-ve-arabirimler-receivers-and-interfaces)
    - [Sıfır-değerli (zero-value) Mutex'ler Geçerlidir](#sıfır-değerli-zero-value-mutexler-geçerlidir)
    - [Slice'ları ve Map'leri Sınırlarda (Boundaries) Kopyalayın](#sliceları-ve-mapleri-sınırlarda-boundaries-kopyalayın)
    - [Temizlemek için Defer Kullan](#temizlemek-için-defer-kullan)
    - [Channel Boyutu 1 veya Yok](#channel-boyutu-1-veya-yok)
    - [Enum'ları 1'den Başlat](#enumları-1den-başlat)
    - [Zamanla uğraşmak için `time` kullanın](#zamanla-uğraşmak-için-time-kullanın)
    - [Errors (Hatalar)](#errors-hatalar)
        - [Error (Hata) Tipleri](#error-hata-tipleri)
        - [Error Sarmalama/Sarma](#error-sarmalamasarma)
        - [Error'ları İsimlendirme](#errorları-i̇simlendirme)
    - [Tür Onaylama Hatalarını Yakalama](#tür-onaylama-hatalarını-yakalama)
    - [Panik (panic) Yapma](#panik-panic-yapma)
    - [go.uber.org/atomic kullan](#gouberorgatomic-kullan)
    - [Değişebilir Globallerden Kaçının](#değişebilir-globallerden-kaçının)
    - [Herkese Açık Struct'larda Tür Gömmekten Kaçının](#herkese-açık-structlarda-tür-gömmekten-kaçının)
    - [Hali Hazırda Bulunan İsimleri Kullanmaktan Kaçının](#hali-hazırda-bulunan-i̇simleri-kullanmaktan-kaçının)
    - [`init()` Fonksiyonundan Kaçının](#init-fonksiyonundan-kaçının)
    - [Main'de Çıkış](#mainde-çıkış)
        - [Bir kez çık](#bir-kez-çık)
    - [Sıralanmış (marshaled) struct'larda alan etiketlerini kullanın](#sıralanmış-marshaled-structlarda-alan-etiketlerini-kullanın)
- [Performans](#performans)
    - [`fmt` yerine `strconv`'u tercih edin](#fmt-yerine-strconvu-tercih-edin)
    - [String'den Byte'a Dönüşümden Kaçının](#stringden-bytea-dönüşümden-kaçının)
    - [Konteyner Kapasitesini Belirtmeyi Tercih Edin](#konteyner-kapasitesini-belirtmeyi-tercih-edin)
        - [Map Kapasitesi İpuçlarını Belirtme](#map-kapasitesi-i̇puçlarını-belirtme)
        - [Slice Kapasitesinin Belirtilmesi](#slice-kapasitesinin-belirtilmesi)
- [Style](#style)
    - [Aşırı uzun satırlardan kaçın](#aşırı-uzun-satırlardan-kaçın)
    - [Tutarlı ol](#tutarlı-ol)
    - [Benzer Bildirimleri Grupla](#benzer-bildirimleri-grupla)
    - [Import Grubu Sıralaması](#import-grubu-sıralaması)
    - [Paket İsimleri](#paket-i̇simleri)
    - [Fonksiyon İsimleri](#fonksiyon-i̇simleri)
    - [Import Lakaplandırma (Aliasing)](#import-lakaplandırma-aliasing)
    - [Fonksiyon Gruplandırma ve Sıralama](#fonksiyon-gruplandırma-ve-sıralama)
    - [İç içeliği Azalt](#i̇ç-içeliği-azalt)
    - [Gereksiz Else](#gereksiz-else)
    - [Üst-Seviye Değişken Tanımlamaları](#üst-seviye-değişken-tanımlamaları)
    - [Dışa aktarılmayan globallerin başına _ ekleyin](#dışa-aktarılmayan-globallerin-başına-_-ekleyin)
    - [Struct'larda İçe Gömmek](#structlarda-i̇çe-gömmek)
    - [Yerel Değişken Tanımlamaları](#yerel-değişken-tanımlamaları)
    - [nil geçerli bir slice'dır](#nil-geçerli-bir-slicedır)
    - [Değişkenlerin Kapsamını Azalt](#değişkenlerin-kapsamını-azalt)
    - [Çıplak Parametrelerden Kaçının](#çıplak-parametrelerden-kaçının)
    - [Escape'ten Kaçınmak için Raw String Literals Kullanın](#escapeten-kaçınmak-için-raw-string-literals-kullanın)
    - [Struct'ları Başlatma](#structları-başlatma)
        - [Struct'ları başlatmak için alan isimlerini kullanın](#structları-başlatmak-için-alan-isimlerini-kullanın)
        - [Struct'larda Sıfır Değer Alanlarını Atla](#structlarda-sıfır-değer-alanlarını-atla)
        - [Sıfır Değerli Struct'lar için `var` Terimini Kullanın](#sıfır-değerli-structlar-için-var-terimini-kullanın)
        - [Struct Referanslarını Başlatma](#struct-referanslarını-başlatma)
    - [Map Başlatma](#map-başlatma)
    - [Printf Dışındaki Format String'leri](#printf-dışındaki-format-stringleri)
    - [Printf-Stili Fonksiyonları İsimlendirme](#printf-stili-fonksiyonları-i̇simlendirme)
- [Desenler](#desenler)
    - [Test Tabloları](#test-tabloları)
    - [Fonksiyonel Seçenekler (Options)](#fonksiyonel-seçenekler-options)
- [Linting](#linting)

## Giriş

Stiller, kodumuzu yöneten kurallardır. Stil terimi biraz yanlış bir adlandırmadır, çünkü bu kurallar yalnızca kaynak dosya biçimlendirmesinden çok daha fazlasını kapsar - `gofmt` bunu bizim için halleder.

Bu kılavuzun amacı, Uber'de Go kodu yazmanın Yapılması ve Yapılmaması Gerekenleri ayrıntılı olarak açıklayarak bu karmaşıklığı yönetmektir. Bu kurallar, mühendislerin Go dili özelliklerini verimli bir şekilde kullanmalarına izin verirken kod tabanını yönetilebilir tutmak için mevcuttur.

Bu kılavuz ilk olarak [Prashant Varanasi] ve [Simon Newton] tarafından bazı meslektaşları Go'yu kullanma konusunda hızlandırmanın bir yolu olarak oluşturulmuştur. Yıllar içinde, diğerlerinden gelen geri bildirimlere dayanarak değiştirilmiştir.

[Prashant Varanasi]: https://github.com/prashantv
[Simon Newton]: https://github.com/nomis52

Bu belge, Uber'de takip ettiğimiz Go kodundaki deyimsel kuralları belgeliyor. Bunların çoğu, Go için genel yönergelerdir, diğer dış kaynaklar ise şunlardır:

1. [Effective Go](https://golang.org/doc/effective_go.html)
2. [Go Common Mistakes](https://github.com/golang/go/wiki/CommonMistakes)
3. [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

Go [sürümleri]nin en son iki küçük sürümü için kod örneklerinin doğru olmasını amaçlıyoruz.

[sürümleri]: https://go.dev/doc/devel/release

`golint` ve `go vet` ile çalıştırıldığında tüm kodlar hatasız olmalıdır. Editörünüzü şu şekilde ayarlamanızı öneririz:

- Kaydedildiğinde `goimports`'u çalıştır
- Hata kontrolleri için `golint` ve `go vet`'i çalıştır

Go araçları _(tools)_ için editör destekleri hakkında bilgilendirmeyi burada bulabilirsiniz:
<https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins>

## Yönergeler

### Arabirimlere Yönelik İşaretçiler _(Pointer to Interfaces)_

Bir arabirim _(interface)_ için neredeyse hiçbir zaman bir işaretçiye _(pointer)_ ihtiyacınız olmaz. Arabirimleri değerler olarak geçirmelisiniz; temel alınan veriler hala bir işaretçi olabilir.

Bir arabirim iki alanlıdır:

1. Bazı türe özgü bilgilere yönelik bir işaretçi. Bunu "type." olarak düşünebilirsiniz.
2. Veri işaretçisi. Depolanan veriler bir işaretçi ise, doğrudan depolanır. Depolanan veriler bir değerse, değere yönelik bir işaretçi saklanır.

Arayüz metodlarının temel alınan verileri değiştirmesini istiyorsanız, bir işaretçi kullanmanız gerekir.

### Arabirim Uyumluluğunu Doğrulayın

Mümkün olduğunda derleme zamanında arayüz uyumluluğunu doğrulayın. Bu da şunları içerir:

- API sözleşmelerinin bir parçası olarak belirli arabirimleri uygulamak için gereken dışa aktarılan türler
- Aynı arabirimi uygulayan türler koleksiyonunun parçası olan dışa aktarılan veya aktarılmayan türler
- Bir arayüzü ihlal etmenin kullanıcıları bozacağı diğer durumlar

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type Handler struct {
  // ...
}



func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  ...
}
```

</td><td>

```go
type Handler struct {
  // ...
}

var _ http.Handler = (*Handler)(nil)

func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

</td></tr>
</tbody></table>

`var _ http.Handler = (*Handler)(nil)` ifadesi, eğer `*Handler`, `http.Handler` ile eşleşmeyi bırakırsa derlenirken hata verir.

Atamanın sağ tarafı, iddia edilen türün sıfır değeri olmalıdır. Bu, işaretçi türleri (`*Handler` gibi), slice'lar ve map'ler için `nil` ve struct türleri için boş bir struct'ır.

```go
type LogHandler struct {
  h   http.Handler
  log *zap.Logger
}

var _ http.Handler = LogHandler{}

func (h LogHandler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

### Alıcılar ve Arabirimler (Receivers and Interfaces)

Değer alıcıları olan metodlar, değerlerin yanı sıra işaretçiler _(pointers)_ üzerinde de çağrılabilir. İşaretçi alıcıları olan metodlar yalnızca işaretçiler veya [adreslenebilir değerler] üzerinde çağrılabilir.

[adreslenebilir değerler]: https://golang.org/ref/spec#Method_values

ÖRnek olarak,

```go
type S struct {
  data string
}

func (s S) Read() string {
  return s.data
}

func (s *S) Write(str string) {
  s.data = str
}

sVals := map[int]S{1: {"A"}}

// Bir değeri kullanarak sadece Read'i çağırabilirsin
sVals[1].Read()

// Bu derlenmeyecektir:
//  sVals[1].Write("test")

sPtrs := map[int]*S{1: {"A"}}

// İşaretçi kullanarak hem Read'i, hem de Write'ı çağırabilirsin
sPtrs[1].Read()
sPtrs[1].Write("test")
```

Benzer şekilde, metod bir değer alıcısına sahip olsa bile, bir arabirim _(interface)_ bir işaretçi _(pointer)_ tarafından karşılanabilir.

```go
type F interface {
  f()
}

type S1 struct{}

func (s S1) f() {}

type S2 struct{}

func (s *S2) f() {}

s1Val := S1{}
s1Ptr := &S1{}
s2Val := S2{}
s2Ptr := &S2{}

var i F
i = s1Val
i = s1Ptr
i = s2Ptr

// Aşağıdakiler derlenmez, çünkü s2Val bir değerdir ve f için değer alıcısı yoktur.
//   i = s2Val
```

Effective Go'da bunun için çok güzel bir bölüm var: [Pointers vs. Values].

[Pointers vs. Values]: https://golang.org/doc/effective_go.html#pointers_vs_values

### Sıfır-değerli _(zero-value)_ Mutex'ler Geçerlidir

`sync.Mutex` ve `sync.RWMutex`'in sıfır değeri geçerlidir, bu nedenle bir mutex için neredeyse hiçbir zaman bir işaretçiye ihtiyacınız olmaz.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
mu := new(sync.Mutex)
mu.Lock()
```

</td><td>

```go
var mu sync.Mutex
mu.Lock()
```

</td></tr>
</tbody></table>

İşaretçiye göre bir struct kullanıyorsanız, mutex, üzerinde işaretçi olmayan bir alan olmalıdır. Struct dışa aktarılmamış olsa bile mutex'i struct'a gömmeyin.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type SMap struct {
  sync.Mutex

  data map[string]string
}

func NewSMap() *SMap {
  return &SMap{
    data: make(map[string]string),
  }
}

func (m *SMap) Get(k string) string {
  m.Lock()
  defer m.Unlock()

  return m.data[k]
}
```

</td><td>

```go
type SMap struct {
  mu sync.Mutex

  data map[string]string
}

func NewSMap() *SMap {
  return &SMap{
    data: make(map[string]string),
  }
}

func (m *SMap) Get(k string) string {
  m.mu.Lock()
  defer m.mu.Unlock()

  return m.data[k]
}
```

</td></tr>

<tr><td>

Mutex alanı, yani artık struct'ın bir parçası olan `Lock` ve `Unlock` metodları `SMap`'in dışa aktarılan API'sinin istenmeyen kısmıdır.

</td><td>

Mutex ve metodları, çağrıcılardan _(caller)_ gizlenen `SMap`'in uygulama detaylarıdır.

</td></tr>
</tbody></table>

### Slice'ları ve Map'leri Sınırlarda _(Boundaries)_ Kopyalayın

Slice'lar ve map'ler, temel alınan verilere işaretçiler içerir, bu nedenle kopyalanmaları gerektiği senaryolara karşı dikkatli olun.

#### Slice'ları ve Map'leri Alma

Argüman olarak bir map'i veya slice'ı alırsanız, bunlar içlerindeki elemanları referans olarak tuttuğu için değişikliğe uğrayabileceğini unutmayın.

<table>
<thead><tr><th>Kötü</th> <th>İyi</th></tr></thead>
<tbody>
<tr>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = trips
}

trips := ...
d1.SetTrips(trips)

// d1.trips'i değiştirmek mi istediniz?
trips[0] = ...
```

</td>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = make([]Trip, len(trips))
  copy(d.trips, trips)
}

trips := ...
d1.SetTrips(trips)

// d1.trips'i etkilemeden trips[0]'ı değiştirebiliriz.
trips[0] = ...
```

</td>
</tr>

</tbody>
</table>

#### Slice'ları ve Map'leri Döndürme

Benzer şekilde, dahili durumu açığa çıkaran map'lerde veya slice'larda yapılan kullanıcı değişikliklerine karşı dikkatli olun.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

// Snapshot, mevcut stats'ı döner.
func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  return s.counters
}

// snapshot artık mutex tarafından korunmuyor
// bu nedenle snapshot'a herhangi bir erişim veri yarışına
// (data race) neden olabilir.
snapshot := stats.Snapshot()
```

</td><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  result := make(map[string]int, len(s.counters))
  for k, v := range s.counters {
    result[k] = v
  }
  return result
}

// Snapshot artık bir kopya.
snapshot := stats.Snapshot()
```

</td></tr>
</tbody></table>

### Temizlemek için Defer Kullan

Dosyalar ve Lock'lar gibi kaynakları temizlemek için `defer` kullan.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
p.Lock()
if p.count < 10 {
  p.Unlock()
  return p.count
}

p.count++
newCount := p.count
p.Unlock()

return newCount

// Çoklu return yapıldığında Unlock'u çağırmak unutulabilir.
```

</td><td>

```go
p.Lock()
defer p.Unlock()

if p.count < 10 {
  return p.count
}

p.count++
return p.count

// daha okunur
```

</td></tr>
</tbody></table>

Defer son derece küçük bir ek yüke sahiptir ve yalnızca fonksiyon çalışma sürenizin nanosaniye düzeyinde olduğunu kanıtlayabilirseniz kaçınılmalıdır. Defer'leri kullanmanın koda kattığı okunabilirlik kazancı, kullanmanın oluşturduğu küçük maliyete değerdir. Bu, özellikle basit bellekten daha fazlasına sahip, diğer hesaplamaların `defer`'den daha önemli olduğu erişimlerde daha büyük metodlar için geçerlidir

### Channel Boyutu 1 veya Yok

Channel'ların genellikle 1 boyutunda olması veya arabelleğe alınmamış _(unbuffered)_ olması gerekir. Varsayılan olarak, channel'lar arabelleğe alınmaz _(unbuffered)_ ve sıfır boyutundadır. Başka herhangi bir boyut, yüksek düzeyde incelemeye tabi olmalıdır. Boyutun nasıl belirlendiğini, channel'ın yük altında dolmasını ve writer'ları bloklamasını neyin engellediğini ve bu gerçekleştiğinde ne olacağını düşünün.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// Herkes için yeterli olmalı!
c := make(chan int, 64)
```

</td><td>

```go
// 1 boyutlu
c := make(chan int, 1) // or
// arabelleğe alınmamış, 0 boyutlu
c := make(chan int)
```

</td></tr>
</tbody></table>

### Enum'ları 1'den Başlat

Go'da numaralandırmaları _(enums)_ tanıtmanın standart yolu, özel bir tür ve `iota` ile bir `const` grubu bildirmektir. Değişkenlerin varsayılan değeri 0 olduğundan, numaralandırmalarınızı genellikle sıfır olmayan bir değerde başlatmalısınız.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota
  Subtract
  Multiply
)

// Add=0, Subtract=1, Multiply=2
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

// Add=1, Subtract=2, Multiply=3
```

</td></tr>
</tbody></table>

Sıfır değeri kullanmanın mantıklı olduğu durumlar vardır, örneğin sıfır değer durumu istenen varsayılan davranış olduğunda kullanmak mantıklıdır.

```go
type LogOutput int

const (
  LogToStdout LogOutput = iota
  LogToFile
  LogToRemote
)

// LogToStdout=0, LogToFile=1, LogToRemote=2
```

### Zamanla uğraşmak için `time` kullanın

Time karmaşıktır. Time hakkında sıklıkla yapılan yanlış varsayımlar aşağıdakileri içerir.

1. 1 gün 24 saattir
2. 1 saat 60 dakikadır
3. 1 hafta 7 gündür
4. 1 yıl 365 gündür
5. [ve daha fazlası](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)

Örneğin _1. (birinci)_, bir zaman anına 24 saat eklemenin her zaman aynı olmayacağı anlamına gelir. yeni bir takvim günü verir.

Bu nedenle, zamanla uğraşırken her zaman [`"time"`] paketini kullanın, çünkü bu yanlış varsayımların daha güvenli ve daha doğru bir şekilde ele alınmasına yardımcı olur.

[`"time"`]: https://golang.org/pkg/time/

#### Bir zaman anı için `time.Time` kullan

Zaman anları ile uğraşırken ve karşılatırma, ekleme, çıkarma işlemleri için [`time.Time`] metodlarını kullanın.

[`time.Time`]: https://golang.org/pkg/time/#Time

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func isActive(now, start, stop int) bool {
  return start <= now && now < stop
}
```

</td><td>

```go
func isActive(now, start, stop time.Time) bool {
  return (start.Before(now) || start.Equal(now)) && now.Before(stop)
}
```

</td></tr>
</tbody></table>

#### Süreler için `time.Duration` kullan

Süreler ile uğraşırken [`time.Duration`] kullanın.

[`time.Duration`]: https://golang.org/pkg/time/#Duration

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func poll(delay int) {
  for {
    // ...
    time.Sleep(time.Duration(delay) * time.Millisecond)
  }
}

poll(10) // hadi bakalım! buraya saniye mi, 
// yoksa milisaniye mi yazıyorduk?
```

</td><td>

```go
func poll(delay time.Duration) {
  for {
    // ...
    time.Sleep(delay)
  }
}

poll(10*time.Second)
```

</td></tr>
</tbody></table>

Bir zaman anına 24 saat ekleme örneğine geri dönersek, zaman eklemek için kullandığımız yöntem niyete bağlıdır. Günün aynı saatini istiyorsak, ancak bir sonraki takvim gününde [`Time.AddDate`] kullanmalıyız. Ancak, bir anın bir önceki zamandan 24 saat sonra olmasını garanti etmek istiyorsak, [`Time.Add`] kullanmalıyız.

[`Time.AddDate`]: https://golang.org/pkg/time/#Time.AddDate
[`Time.Add`]: https://golang.org/pkg/time/#Time.Add

```go
newDay := t.AddDate(0 /* yıl */, 0 /* ay */, 1 /* gün */)
maybeNewDay := t.Add(24 * time.Hour)
```

#### Dış sistemler için `time.Time` ve `time.Duration` kullan

Mümkün olduğunda, dış sistemler ile etkileşim için `time.Duration` ve `time.Time` kullanın. Örnek olarak:

- Komut-satırı flag'leri: [`flag`], [`time.ParseDuration`] aracılığıyla `time.Duration`'ı destekler.
- JSON: [`encoding/json`], bir [RFC 3339] string'i olarak [`UnmarshalJSON` metodu] ile `time.Time`'ı dönüştürmeyi destekler.
- SQL: [`database/sql`], `DATETIME` veya `TIMESTAMP` sütunlarını `time.Time`'a dönüştürmeyi ve temel sürücü destekliyorsa geri döndürmeyi destekler
- YAML: [`gopkg.in/yaml.v2`], [RFC 3339] string'i olarak `time.Time`'ı ve [`time.ParseDuration`] aracılığıyla `time.Duration`'ı destekler.

  [`flag`]: https://golang.org/pkg/flag/
  [`time.ParseDuration`]: https://golang.org/pkg/time/#ParseDuration
  [`encoding/json`]: https://golang.org/pkg/encoding/json/
  [RFC 3339]: https://tools.ietf.org/html/rfc3339
  [`UnmarshalJSON` metodu]: https://golang.org/pkg/time/#Time.UnmarshalJSON
  [`database/sql`]: https://golang.org/pkg/database/sql/
  [`gopkg.in/yaml.v2`]: https://godoc.org/gopkg.in/yaml.v2

Bu etkileşimlerde `time.Duration`'ı kullanmak mümkün olmadığında, `int` veya `float64` kullanın ve alan adına birimi ekleyin.

Örneğin, `encoding/json`, `time.Duration` öğesini desteklemediğinden, birim alan adına dahil edilir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// {"interval": 2}
type Config struct {
  Interval int `json:"interval"`
}
```

</td><td>

```go
// {"intervalMillis": 2000}
type Config struct {
  IntervalMillis int `json:"intervalMillis"`
}
```

</td></tr>
</tbody></table>

Bu etkileşimlerde `time.Time` kullanılması mümkün olmadığında, bir alternatif üzerinde anlaşmaya varılmadığı sürece, `string` kullanın ve zaman damgalarını [RFC 3339]'da tanımlandığı gibi biçimlendirin. Bu biçim varsayılan olarak [`Time.UnmarshalText`] tarafından kullanılır ve [`time.RFC3339`] aracılığıyla `Time.Format` ve `time.Parse` içinde kullanılabilir.

[`Time.UnmarshalText`]: https://golang.org/pkg/time/#Time.UnmarshalText
[`time.RFC3339`]: https://golang.org/pkg/time/#RFC3339

Bu, pratikte bir sorun teşkil etmese de, `time` paketinin artık saniyelerle zaman damgalarının ayrıştırılmasını desteklemediğini _([8728])_ ve hesaplamalarda artık saniyeleri hesaba katmadığını _([15190])_ unutmayın. İki zaman anını karşılaştırırsanız, fark, bu iki an arasında meydana gelmiş olabilecek "artık" _(fazla)_ saniyeleri içermez.

[8728]: https://github.com/golang/go/issues/8728
[15190]: https://github.com/golang/go/issues/15190

<!-- TODO: section on String methods for enums -->

### Errors _(Hatalar)_

#### Error _(Hata)_ Tipleri

Error'ları tanımlamak için birkaç seçenek vardır.
Kullanım durumunuza en uygun seçeneği seçmeden önce aşağıdakileri göz önünde bulundurun.
- Arayanın, işleyebilmesi için hatayı eşleştirmesi gerekiyor mu? Evetse, bir üst düzey hata değişkeni veya özel bir tür bildirerek [`errors.Is`] veya [`errors.As`] fonksiyonlarını desteklememiz gerekir.
- Hata mesajı statik bir dize mi yoksa bağlamsal bilgi gerektiren dinamik bir string mi?
  İlki için [`errors.New`] kullanabiliriz, ancak ikincisi için [`fmt.Errorf`] veya özel bir error tipi kullanmalıyız.
- Aşağı akış fonksiyonunda döndürülen bir hatayı farklı bölümlerde kullanacakmıyız?
  Eğer öyleyse, [Error Sarmalama](#error-sarmalamasarma) konusuna bakın.

[`errors.Is`]: https://golang.org/pkg/errors/#Is
[`errors.As`]: https://golang.org/pkg/errors/#As

| Hata Eşleşmesi Yapılıyor mu? | Hata Mesajı | Neyi kullanalım?                    |
|------------------------------|-------------|-------------------------------------|
| Hayır                        | static      | [`errors.New`]                      |
| Hayır                        | dinamik     | [`fmt.Errorf`]                      |
| Evet                         | static      | [`errors.New`] ile üst-seviye `var` |
| Evet                         | dinamik     | özel `error` türü                   |

[`errors.New`]: https://golang.org/pkg/errors/#New
[`fmt.Errorf`]: https://golang.org/pkg/fmt/#Errorf

Örnek olarak,
Değişmeyen _(static)_ string'li bir error _(hata)_ için [`errors.New`] kullanın.
Arayanın bu hatayı eşleştirmesi ve işlemesi gerekiyorsa, bu hatayı `errors.Is` ile eşleştirmeyi desteklemek için bir değişken olarak dışa aktarın.

<table>
<thead><tr><th>Hata eşlemesi yok</th><th>Hata eşlemesi var</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open() error {
  return errors.New("could not open")
}

// package bar

if err := foo.Open(); err != nil {
  // Hatayı işleyemiyoruz.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

var ErrCouldNotOpen = errors.New("could not open")

func Open() error {
  return ErrCouldNotOpen
}

// package bar

if err := foo.Open(); err != nil {
  if errors.Is(err, foo.ErrCouldNotOpen) {
    // Hatayı işle
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Dinamik string'li bir error için,
arayıcının bunu eşleştirmesi gerekmiyorsa [`fmt.Errorf`], eğer gerekiyorsa özel bir `error` türü kullanın.

<table>
<thead><tr><th>Error eşleştirmesi yok</th><th>Error eşleştirmesi var</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open(file string) error {
  return fmt.Errorf("file %q not found", file)
}

// package bar

if err := foo.Open("testfile.txt"); err != nil {
  // Hatayı işleyemiyoruz.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

func Open(file string) error {
  return &NotFoundError{File: file}
}


// package bar

if err := foo.Open("testfile.txt"); err != nil {
  var notFound *NotFoundError
  if errors.As(err, &notFound) {
    // Hatayı işle
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Bir paketten hata değişkenlerini veya türlerini dışa aktarırsanız,
paketin genel API'sinin bir parçası olacaklardır.

#### Error Sarmalama/Sarma

Bir çağrı başarısız olursa hataları _(errors)_ aktarmak için üç ana seçenek vardır.:

- orijinal hatayı olduğu gibi döndürmek
- `fmt.Errorf` ve `%w` fiili ile bir bağlam _(context)_ eklemek
- `fmt.Errorf` ve `%v` fiili ile bir bağlam _(context)_ eklemek

Yoksa orijinal hatayı olduğu gibi döndürün. Bu, orijinal hata türünü ve mesajını korur.
Temel alınan hata mesajının nereden geldiğini bulmak için yeterli bilgiye sahip olduğu durumlar için çok uygundur.

Aksi takdirde, "connection refused" gibi belirsiz bir hata yerine "call service foo: connection refused" gibi daha yararlı hatalar elde etmek için mümkünse hata mesajına bağlam _(context)_ ekleyin.

Hatalarınıza bağlam eklemek için `fmt.Errorf`'i kullanın, arayanın temel nedeni eşleştirip çıkaramayacağına _(unwrap)_ bağlı olarak `%w` veya `%v` fiilleri arasında seçim yapın.

- Arayanın temeldeki hataya erişimi olması gerekiyorsa `%w` kullanın.
  Bu, çoğu sarılmış hata için iyi bir varsayılandır, ancak arayanların bu davranışa güvenmeye başlayabileceğini unutmayın. Bu nedenle, sarılmış hatanın bilinen bir `var` veya tür olduğu durumlar için, belgeleyin ve fonksiyonunuzun sözleşmesinin bir parçası olarak test edin.
- Temeldeki hatayı gizlemek için `%v` kullanın. Arayanlar bunu eşleştiremez, ancak gerekirse gelecekte `%w`ye geçebilirsiniz.

Döndürülen hatalara bağlam eklerken, bariz olanı belirten ve hata yığın boyunca süzülürken biriken "failed to" gibi ifadelerden kaçınarak bağlamı kısa ve öz tutun:

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "failed to create new store: %w", err)
}
```

</td><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "new store: %w", err)
}
```

</td></tr><tr><td>

```
failed to x: failed to y: failed to create new store: the error
```

</td><td>

```
x: y: new store: the error
```

</td></tr>
</tbody></table>

Ancak hata başka bir sisteme gönderildiğinde, mesajın bir hata olduğu açık olmalıdır _(örneğin, günlüklerde (logs) `err` etiketi veya "Failed" öneki)_.

Ayrıca bkz, [Hataları sadece kontrol etme, onları nazikçe işle].

[`"pkg/errors".Cause`]: https://godoc.org/github.com/pkg/errors#Cause
[Hataları sadece kontrol etme, onları nazikçe işle]: https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully

#### Error'ları İsimlendirme

Global olarak saklanan Error değerleri için,
dışa aktarılıp aktarılmadığına bağlı olarak `Err` veya `err` önekini kullanın.
İsimlendirme için ayrıca [Dışa aktarılmayan globallere _ ön eki ekle](#dışa-aktarılmayan-globallerin-başına-_-ekleyin) bölümüne bakınız.

```go
var (
  // Aşağıdaki iki hata dışa aktarılır, 
  // böylece bu paketin kullanıcıları, bunları 
  // error.Is ile eşleştirebilir.

  ErrBrokenLink = errors.New("link is broken")
  ErrCouldNotOpen = errors.New("could not open")

  // Bu hata, onu genel API'mizin bir parçası yapmak istemediğimiz için dışa aktarılmaz. Yine de error.Is ile paket içinde kullanabiliriz.

  errNotFound = errors.New("not found")
)
```

Özel Error türleri için, `Error`son ekini kullanın.

```go
// Benzer olarak, Bu Error dışa aktarılmış
// Yani kullanıcılar bunu error.As ile 
// eşleştirebilir.

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

// Ve bu Error dışa aktarılmamış çünkü herkese açık
// bir API'ın parçası olmasını istemiyoruz.
// Fakat paket içerisinde hala bu error türünü
// error.As ile eşleştirebiliriz.

type resolveError struct {
  Path string
}

func (e *resolveError) Error() string {
  return fmt.Sprintf("resolve %q", e.Path)
}
```

### Tür Onaylama Hatalarını Yakalama

Bir [type assertion] tek dönüş değeri formu, yanlış bir türde panik _(panic)_ yapacaktır. Bu nedenle, her zaman "comma ok" deyimini kullanın.

[type assertion]: https://golang.org/ref/spec#Type_assertions

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
t := i.(string)
```

</td><td>

```go
t, ok := i.(string)
if !ok {
  // hatayı nazikçe işle
}
```

</td></tr>
</tbody></table>

<!-- TODO: There are a few situations where the single assignment form is
fine. -->

### Panik _(panic)_ Yapma

Üretimde çalışan kodlar panikten kaçınmalıdır. Panikler, [basamaklı başarısızlıkların _(cascading failures)_] önemli bir kaynağıdır. Bir hata oluşursa, işlev bir hata döndürmeli ve arayanın bunu nasıl ele alacağına karar vermesine izin vermelidir.

[basamaklı başarısızlıkların _(cascading failures)_]: https://en.wikipedia.org/wiki/Cascading_failure

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func run(args []string) {
  if len(args) == 0 {
    panic("an argument is required")
  }
  // ...
}

func main() {
  run(os.Args[1:])
}
```

</td><td>

```go
func run(args []string) error {
  if len(args) == 0 {
    return errors.New("an argument is required")
  }
  // ...
  return nil
}

func main() {
  if err := run(os.Args[1:]); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

</td></tr>
</tbody></table>

Panik/kurtarma _(panic/recover)_ bir hata işleme stratejisi değildir. Bir program, yalnızca sıfır referansı gibi geri dönüşü olmayan bir şey olduğunda panik yapmalıdır. Bunun bir istisnası program başlatmadır: program başlangıcında programı iptal etmesi gereken kötü şeyler paniğe neden olabilir.

```go
var _statusTemplate = template.Must(template.New("name").Parse("_statusHTML"))
```

Testlerde bile, testin başarısız olarak işaretlendiğinden emin olmak için panikler yerine `t.Fatal` veya `t.FailNow`'ı tercih edin.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  panic("failed to set up test")
}
```

</td><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  t.Fatal("failed to set up test")
}
```

</td></tr>
</tbody></table>

<!-- TODO: Explain how to use _test packages. -->

### go.uber.org/atomic kullan

[sync/atomic] paketiyle yapılan atomik işlemler ham türler (`int32`, `int64`, vb.) üzerinde çalışır, bu nedenle değişkenleri okumak veya değiştirmek için atomik işlemi kullanmayı unutmak kolaydır.

[go.uber.org/atomic], temel alınan türü gizleyerek bu işlemlere tür güvenliği ekler. Ek olarak, uygun bir `atomic.Bool` türü içerir.

[go.uber.org/atomic]: https://godoc.org/go.uber.org/atomic
[sync/atomic]: https://golang.org/pkg/sync/atomic/

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type foo struct {
  running int32  // atomic
}

func (f* foo) start() {
  if atomic.SwapInt32(&f.running, 1) == 1 {
     // zaten çalışıyor…
     return
  }
  // Foo'yu başlat
}

func (f *foo) isRunning() bool {
  return f.running == 1  // burada bir veri yarışı var!
}
```

</td><td>

```go
type foo struct {
  running atomic.Bool
}

func (f *foo) start() {
  if f.running.Swap(true) {
     // zaten çalışıyor…
     return
  }
  // Foo'yu başlat
}

func (f *foo) isRunning() bool {
  return f.running.Load()
}
```

</td></tr>
</tbody></table>

### Değişebilir Globallerden Kaçının

Bağımlılık enjeksiyonunu _(dependency injection)_ tercih etmek yerine global değişkenleri değiştirmekten kaçının.
Bu, fonksiyon işaretçilerinin yanı sıra diğer değer türleri için de geçerlidir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// sign.go

var _timeNow = time.Now

func sign(msg string) string {
  now := _timeNow()
  return signWithTime(msg, now)
}
```

</td><td>

```go
// sign.go

type signer struct {
  now func() time.Time
}

func newSigner() *signer {
  return &signer{
    now: time.Now,
  }
}

func (s *signer) Sign(msg string) string {
  now := s.now()
  return signWithTime(msg, now)
}
```
</td></tr>
<tr><td>

```go
// sign_test.go

func TestSign(t *testing.T) {
  oldTimeNow := _timeNow
  _timeNow = func() time.Time {
    return someFixedTime
  }
  defer func() { _timeNow = oldTimeNow }()

  assert.Equal(t, want, sign(give))
}
```

</td><td>

```go
// sign_test.go

func TestSigner(t *testing.T) {
  s := newSigner()
  s.now = func() time.Time {
    return someFixedTime
  }

  assert.Equal(t, want, s.Sign(give))
}
```

</td></tr>
</tbody></table>

### Herkese Açık Struct'larda Tür Gömmekten Kaçının

Bu gömülü türler, uygulama ayrıntılarını sızdırır, tür gelişimini engeller ve belgeleri belirsizleştirir.

Paylaşılan _(herkese açık)_ bir `AbstractList` kullanarak çeşitli liste türleri uyguladığınızı varsayarsak, `AbstractList`'i somut _(concrete)_ liste uygulamalarınıza yerleştirmekten kaçının.
Bunun yerine, yalnızca soyut _(abtract)_ listeye devredilecek metodları somut listenize elle yazın.

```go
type AbstractList struct {}

// Add, listeye bir varlık (nesne) ekler.
func (l *AbstractList) Add(e Entity) {
  // ...
}

// Remove, listeden bir varlığı (nesneyi) kaldırır.
func (l *AbstractList) Remove(e Entity) {
  // ...
}
```

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// ConcreteList, bir nesne (varlık) listesidir.
type ConcreteList struct {
  *AbstractList
}
```

</td><td>

```go
// ConcreteList, bir nesne (varlık) listesidir.
type ConcreteList struct {
  list *AbstractList
}

// Add, listeye bir varlık (nesne) ekler.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove, listeden bir varlığı (nesneyi) kaldırır.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

Go, kalıtım ve kompozisyon arasında bir uzlaşma olarak [tür gömmeye _(type embedding)_] izin verir.
Dış tür, gömülü türün metodlarının örtük kopyalarını alır.
Bu metodlar, varsayılan olarak, gömülü örneğin aynı metoduna temsilci olarak atanır.

[tür gömmeye _(type embedding)_]: https://golang.org/doc/effective_go.html#embedding

Yapı ayrıca türle aynı ada sahip bir alan kazanır.
Bu nedenle, gömülü tür herkese açık ise, alan herkese açıktır.
Geriye dönük uyumluluğu korumak için, dış türün gelecekteki her sürümü, gömülü türü korumalıdır.

Gömülü bir tür nadiren gereklidir.
Sıkıcı temsilci metodları yazmaktan kaçınmanıza yardımcı olan bir kolaylıktır.

Struct yerine uyumlu bir AbstractList *arabirimini (interface)* gömmek bile geliştiriciye gelecekte değişiklik yapma konusunda daha fazla esneklik sağlar, ancak yine de somut listelerin soyut bir uygulama kullandığı ayrıntısını sızdırır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// AbstractList, çeşitli varlık listeleri 
// için genelleştirilmiş bir uygulamadır.
type AbstractList interface {
  Add(Entity)
  Remove(Entity)
}

// ConcreteList, bir nesne (varlık) listesidir.
type ConcreteList struct {
  AbstractList
}
```

</td><td>

```go
// ConcreteList, bir nesne (varlık) listesidir.
type ConcreteList struct {
  list AbstractList
}

// Add, listeye bir varlık (nesne) ekler.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove, listeden bir varlığı kaldırır.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

Ya gömülü bir struct ya da gömülü bir arabirim _(interface)_ ile, gömülü tür, türün evrimine sınırlar koyar.

- Gömülü bir arabirime metodlar eklemek, son derece önemli bir değişikliktir.
- Metodları gömülü bir struct'tan kaldırma son derece önemli bir değişikliktir.
- Gömülü türü kaldırma son derece önemli bir değişikliktir.
- Aynı arabirimi _(interface)_ karşılayan bir alternatifle bile gömülü türün değiştirilmesi son derece önemli bir değişikliktir.

Bu temsilci metodlarını _(arabirimde belirtilen metodlar)_ yazmak sıkıcı olsa da, ek çaba bir uygulama _(implementasyon anlamında)_ ayrıntısını gizler, değişiklik için daha fazla fırsat bırakır ve ayrıca belgelerde tam Liste arabirimini keşfetmeye yönelik dolaylılığı ortadan kaldırır.

### Hali Hazırda Bulunan İsimleri Kullanmaktan Kaçının

Go [dil belirtimi], Go programlarında ad olarak kullanılmaması gereken birkaç yerleşik, [önceden bildirilen tanımlayıcı] özetler.

Bağlama _(context)_ bağlı olarak, bu tanımlayıcıları ad olarak yeniden kullanmak, orijinali, geçerli sözcüksel kapsamda _(ve tüm iç içe geçmiş kapsamlarda)_ gölgeleyecek veya etkilenen kodun kafasını karıştıracaktır. En iyi durumda, derleyici şikayet edecektir; en kötü durumda, bu tür kodlar gizli, tespit edilmesi zor hatalara neden olabilir.

[dil belirtimi]: https://golang.org/ref/spec
[önceden bildirilen tanımlayıcı]: https://golang.org/ref/spec#Predeclared_identifiers

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
var error string
// `error` halihazırda olan ismi gölgeler

// or

func handleErrorMessage(error string) {
    // `error` halihazırda olan ismi gölgeler
}
```

</td><td>

```go
var errorMessage string
// `error` hali hazırlı olana referans eder

// or

func handleErrorMessage(msg string) {
    // `error` hali hazırlı olana referans eder
}
```

</td></tr>
<tr><td>

```go
type Foo struct {
    // Bu alanlar teknik olarak gölgeleme oluşturmasa da,
	// `error` veya `string` string'leri için tespit yapmak
	// artık belirsizdir.
    error  error
    string string
}

func (f Foo) Error() error {
    // `error` ve `f.error`
    // görsel olarak benzerdir
    return f.error
}

func (f Foo) String() string {
    // `string` ve `f.string`
    // görsel olarak benzerdir
    return f.string
}
```

</td><td>

```go
type Foo struct {
    // `error` ve `string` string'leri
    // şuanda daha belirgin.
    err error
    str string
}

func (f Foo) Error() error {
    return f.err
}

func (f Foo) String() string {
    return f.str
}
```

</td></tr>
</tbody></table>


Derleyicinin önceden bildirilmiş tanımlayıcıları _(predeclared identifiers)_ kullanırken hata oluşturmayacağını, ancak `go vet` gibi araçların bu ve diğer gölgeleme durumlarını doğru bir şekilde göstermesi gerektiğini unutmayın.

### `init()` Fonksiyonundan Kaçının

Mümkünse `init()` kullanmaktan kaçının. `init()` kaçınılmaz veya arzu edilir olduğunda, kod şunları yapmaya çalışmalıdır:

1. Program ortamından veya çağrıdan bağımsız olarak tamamen belirleyici olun.
2. Diğer `init()` fonksiyonlarının sıralamasına veya yan etkilerine bağlı kalmaktan kaçının. `init()` sıralaması iyi bilinse de kod değişebilir ve bu nedenle `init()` fonksiyonları arasındaki ilişkiler kodu kırılgan ve hataya açık hale getirebilir.
3. Makine bilgileri, ortam değişkenleri, çalışma dizini, program argümanları/girişleri vb. gibi genel veya ortam durumuna erişmekten veya bunları değiştirmekten kaçının.
4. Dosya sistemi, ağ ve sistem çağrıları dahil olmak üzere G/Ç'den _(input/output)_ kaçının.

Bu gereksinimleri karşılayamayan kod, muhtemelen `main()`'in _(veya bir programın yaşam döngüsünün başka bir yerinde)_ bir parçası olarak çağrılacak bir yardımcı olarak aittir veya `main()`'in kendisinin bir parçası olarak yazılabilir. Özellikle diğer programlar tarafından kullanılması amaçlanan kütüphaneler, tamamen deterministik _(belirleyici)_ olmaya ve "init magic" yapmamaya özen göstermelidir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type Foo struct {
    // ...
}

var _defaultFoo Foo

func init() {
    _defaultFoo = Foo{
        // ...
    }
}
```

</td><td>

```go
var _defaultFoo = Foo{
    // ...
}

// veya, daha iyi, test edilebilirlik için:

var _defaultFoo = defaultFoo()

func defaultFoo() Foo {
    return Foo{
        // ...
    }
}
```

</td></tr>
<tr><td>

```go
type Config struct {
    // ...
}

var _config Config

func init() {
    // Kötü: mevcut dizin bazlı
    cwd, _ := os.Getwd()

    // Kötü: G/Ç
    raw, _ := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )

    yaml.Unmarshal(raw, &_config)
}
```

</td><td>

```go
type Config struct {
    // ...
}

func loadConfig() Config {
    cwd, err := os.Getwd()
    // error'u yakala

    raw, err := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )
    // error'u yakala

    var config Config
    yaml.Unmarshal(raw, &config)

    return config
}
```

</td></tr>
</tbody></table>

Yukarıdakileri göz önünde bulundurarak, `init()`'in tercih edilebileceği veya gerekli olabileceği bazı durumlar şunları içerebilir:

- Tek atamalar olarak temsil edilemeyen karmaşık ifadeler.
- `database/sql` deyimleri, kodlama türü kayıtları _(encoding type registries)_ vb. gibi eklenebilir hook'lar.
- [Google Cloud Functions] ve diğer deterministik _(belirleyici)_ ön hesaplama formlarına yönelik optimizasyonlar.

  [Google Cloud Functions]: https://cloud.google.com/functions/docs/bestpractices/tips#use_global_variables_to_reuse_objects_in_future_invocations

### Main'de Çıkış

Go programları, hemen çıkmak için [`os.Exit`] veya [`log.Fatal*`] kullanır. (Paniklemek programlardan çıkmak için iyi bir yol değildir, lütfen [panik yapmayın](#panik-panic-yapma).)

[`os.Exit`]: https://golang.org/pkg/os/#Exit
[`log.Fatal*`]: https://golang.org/pkg/log/#Fatal

`main()` içinde **yalnızca** `os.Exit` veya `log.Fatal*` kullanın. Diğer tüm fonksiyonlar, hataları sinyal arızasına döndürmelidir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func main() {
  body := readFile(path)
  fmt.Println(body)
}

func readFile(path string) string {
  f, err := os.Open(path)
  if err != nil {
    log.Fatal(err)
  }

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  return string(b)
}
```

</td><td>

```go
func main() {
  body, err := readFile(path)
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println(body)
}

func readFile(path string) (string, error) {
  f, err := os.Open(path)
  if err != nil {
    return "", err
  }

  b, err := io.ReadAll(f)
  if err != nil {
    return "", err
  }

  return string(b), nil
}
```

</td></tr>
</tbody></table>

Gerekçe: Birden çok çok çıkış sunan programlar bir kaç tür sorun oluşturabilir:

- Açık olmayan kontrol akışı: Herhangi bir fonksiyon programdan çıkabilir, bu nedenle kontrol akışı hakkında akıl yürütmek zorlaşır.
- Test edilmesi zor: Programdan çıkan bir fonksiyon, onu çağıran testten de çıkacaktır. Bu, fonksiyonun test edilmesini zorlaştırır ve `go test` tarafından henüz çalıştırılmamış diğer testleri atlama riskini beraberinde getirir.
- Atlanan temizleme: Bir fonksiyon programdan çıktığında, `defer` ifadeleriyle sıralanan fonksiyon çağrılarını atlar. Bu, önemli temizleme görevlerini atlama riskini artırır.

#### Bir kez çık

Mümkünse, `main()` içinde **en fazla bir kere** `os.Exit` veya `log.Fatal` çağırmayı tercih edin. Programın yürütülmesini durduran birden fazla hata senaryosu varsa, bu mantığı ayrı bir fonksiyon altına koyun ve oradan hataları döndürün.

Bu, `main()` fonksiyonunu kısaltma ve tüm temel iş mantığını ayrı, test edilebilir bir fonksiyona koyma etkisine sahiptir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
package main

func main() {
  args := os.Args[1:]
  if len(args) != 1 {
    log.Fatal("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    log.Fatal(err)
  }
  defer f.Close()

  // Eğer log.Fatal'ı bu satırdan sonra çağırırsak,
  // f.Close çağrılmayacaktır.

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  // ...
}
```

</td><td>

```go
package main

func main() {
  if err := run(); err != nil {
    log.Fatal(err)
  }
}

func run() error {
  args := os.Args[1:]
  if len(args) != 1 {
    return errors.New("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    return err
  }
  defer f.Close()

  b, err := io.ReadAll(f)
  if err != nil {
    return err
  }

  // ...
}
```

</td></tr>
</tbody></table>

### Sıralanmış _(marshaled)_ struct'larda alan etiketlerini kullanın

JSON, YAML veya etiket tabanlı alan adlandırmayı destekleyen diğer biçimlerde sıralanan _(marshaled)_ herhangi bir struct alanı, ilgili etiketle açıklanmalıdır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type Stock struct {
  Price int
  Name  string
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td><td>

```go
type Stock struct {
  Price int    `json:"price"`
  Name  string `json:"name"`
  // Adı Sembol olarak yeniden adlandırmak güvenli.
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td></tr>
</tbody></table>

Gerekçe:

Struct'ın serileştirilmiş _(serialized)_ formu, farklı sistemler arasındaki bir sözleşmedir. Serileştirilmiş formun struct'ındaki _(alan adları dahil)_ yapılan değişiklikler bu sözleşmeyi bozar. Etiketlerin içinde alan adlarının belirtilmesi, sözleşmeyi açık hale getirir ve alanları yeniden düzenleme veya yeniden adlandırma yoluyla yanlışlıkla sözleşmeyi bozmaya karşı koruma sağlar.

## Performans

Performansa özel yönergeler yalnızca etkin yol için geçerlidir.

### `fmt` yerine `strconv`'u tercih edin

İlkel (primitive) türleri string'lere veya string'lerden çevirirken, `strconv`, `fmt`'den daha hızlıdır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  s := fmt.Sprint(rand.Int())
}
```

</td><td>

```go
for i := 0; i < b.N; i++ {
  s := strconv.Itoa(rand.Int())
}
```

</td></tr>
<tr><td>

```
BenchmarkFmtSprint-4    143 ns/op    2 allocs/op
```

</td><td>

```
BenchmarkStrconv-4    64.2 ns/op    1 allocs/op
```

</td></tr>
</tbody></table>

### String'den Byte'a Dönüşümden Kaçının

Sabit bir string'den art arda byte slice'ları oluşturmayın. Bunun yerine, dönüştürmeyi bir kez gerçekleştirin ve sonucu yakalayın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  w.Write([]byte("Hello world"))
}
```

</td><td>

```go
data := []byte("Hello world")
for i := 0; i < b.N; i++ {
  w.Write(data)
}
```

</td></tr>
<tr><td>

```
BenchmarkBad-4   50000000   22.2 ns/op
```

</td><td>

```
BenchmarkGood-4  500000000   3.25 ns/op
```

</td></tr>
</tbody></table>

### Konteyner Kapasitesini Belirtmeyi Tercih Edin

Ön kapsayıcı için bellek ayırmak için mümkünse kapsayıcı kapasitesini belirtin. Bu, öğeler eklendikçe sonraki tahsisleri _(kapsayıcıyı kopyalayarak ve yeniden boyutlandırarak)_ en aza indirir.

#### Map Kapasitesi İpuçlarını Belirtme

Mümkünse, map'leri `make()` ile başlatırken kapasite ipuçları sağlayın.

```go
make(map[T1]T2, hint)
```

`make()` fonksiyonuna bir kapasite ipucu sağlamak, başlatma zamanında map'i doğru boyutlandırmaya çalışır, bu da map'i büyütme ihtiyacını ve map'e öğeler eklendikçe yeni alan tahsis etme ihtiyacını ortadan kaldırır.

Slice'ların aksine, map kapasitesi ipuçlarının tam, önleyici tahsisi garanti etmediğini, ancak gereken hashmap bucket'larının sayısını tahmin etmek için kullanıldığını unutmayın. Sonuç olarak, map'e öğeler eklenirken, belirtilen kapasiteye kadar olsa bile, tahsisler yine de gerçekleşebilir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[string]os.FileInfo)

files, _ := os.ReadDir("./files")
for _, f := range files {
    m[f.Name()] = f
}
```

</td><td>

```go

files, _ := os.ReadDir("./files")

m := make(map[string]os.DirEntry, len(files))
for _, f := range files {
    m[f.Name()] = f
}
```

</td></tr>
<tr><td>

`m`, boyut ipucu olmadan oluşturulur; atama zamanında daha fazla tahsis olabilir.

</td><td>

`m`, bir boyut ipucuyla oluşturulur; atama zamanında daha az tahsis olabilir.

</td></tr>
</tbody></table>

#### Slice Kapasitesinin Belirtilmesi

Mümkün olduğunda, slice'ları `make()` ile başlatırken, özellikle eklerken kapasite ipuçları sağlayın.

```go
make([]T, length, capacity)
```

Map'lerden farklı olarak, slice kapasitesi bir ipucu değildir: derleyici, slice'ın kapasitesi için `make()`'te belirtilen belleği ayıracaktır. Bu `append()` ile yapılan ileri zamanlardaki ekleme işlemlerinde yeniden tahsis işlemine gerek olmayacağı anlamına gelir. Eğer belirtilen ipucunun dışına çıkılacak olursa, o zamanda yeniden tahsis işlemi gerçekleşir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0, size)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td></tr>
<tr><td>

```
BenchmarkBad-4    100000000    2.48s
```

</td><td>

```
BenchmarkGood-4   100000000    0.21s
```

</td></tr>
</tbody></table>

## Style

### Aşırı uzun satırlardan kaçın

Okuyucuların yatay olarak kaydırmasını gerektiren ya da kafalarını çok çevirmelerini gerektiren kod satırlarından kaçının.

Satır içi limiti **99 karakter** olarak öneriyoruz.
Yazarlar bu limite ulaşmadan alt satıra geçmelidir, ancak bu bir zorunluluk değildir.
Kodun bu sınırı aşmasına izin verilir.

### Tutarlı ol

Bu belgede ana hatları verilen bazı yönergeler nesnel olarak değerlendirilebilir;
diğerleri durumsal, bağlamsal veya özneldir.

Herşeyden önce, **tutarlı ol**.

Tutarlı kodun bakımı daha kolaydır, rasyonelleştirilmesi daha kolaydır, daha az bilişsel ek yük gerektirir ve yeni kurallar ortaya çıktıkça veya hata sınıfları düzeltildikçe taşınması veya güncellenmesi daha kolaydır.

Tersine, tek bir kod tabanında birden çok farklı veya çelişen stile sahip olmak, bakım yüküne, belirsizliğe ve bilişsel uyumsuzluğa neden olur; bunların tümü, daha düşük hıza, acı verici kod incelemelerine ve hatalara doğrudan katkıda bulunabilir.

Bu yönergeleri bir kod tabanına uygularken, değişiklikler
paket (veya daha büyük) düzeyinde yapılır: alt paket düzeyinde uygulama
aynı koda birden çok stil ekleyerek yukarıdaki endişeyi ihlal eder.

### Benzer Bildirimleri Grupla

Go, benzer bildirimleri gruplamayı destekler.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
import "a"
import "b"
```

</td><td>

```go
import (
  "a"
  "b"
)
```

</td></tr>
</tbody></table>

Bu aynı zamanda sabitler, değişkenler ve tür bildirimleri için de geçerlidir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go

const a = 1
const b = 2



var a = 1
var b = 2



type Area float64
type Volume float64
```

</td><td>

```go
const (
  a = 1
  b = 2
)

var (
  a = 1
  b = 2
)

type (
  Area float64
  Volume float64
)
```

</td></tr>
</tbody></table>

Yalnızca grupla ilgili bildirimler. İlişkili olmayan bildirimleri gruplamayın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
  EnvVar = "MY_ENV"
)
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

const EnvVar = "MY_ENV"
```

</td></tr>
</tbody></table>

Gruplamanın kullanılabileceği yerler sınırlı değildir. Örneğin, bu yöntemi fonksiyonların içinde kullanabilirsiniz.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func f() string {
  red := color.New(0xff0000)
  green := color.New(0x00ff00)
  blue := color.New(0x0000ff)

  // ...
}
```

</td><td>

```go
func f() string {
  var (
    red   = color.New(0xff0000)
    green = color.New(0x00ff00)
    blue  = color.New(0x0000ff)
  )

  // ...
}
```

</td></tr>
</tbody></table>

İstisna: Değişken bildirimleri, özellikle fonksiyonların içinde olanlar, diğer değişkenlere bitişik olarak bildirilirse birlikte gruplandırılır. Hatta ilişkisiz olsa bile bunu değişkenler için yapabilirsiniz.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func (c *client) request() {
  caller := c.name
  format := "json"
  timeout := 5*time.Second
  var err error

  // ...
}
```

</td><td>

```go
func (c *client) request() {
  var (
    caller  = c.name
    format  = "json"
    timeout = 5*time.Second
    err error
  )

  // ...
}
```

</td></tr>
</tbody></table>

### Import Grubu Sıralaması

İki Import grubu olmalı:

- Standard Kütüphane
- Diğer her şey

Aşağıdaki `goimports` tarafından uygulanan varsayılan gruplamadır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"
  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td><td>

```go
import (
  "fmt"
  "os"

  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td></tr>
</tbody></table>

### Paket İsimleri

Paketler isimlendirirken, şu şekilde bir yöntem seçin:

- Tümü küçük harf, Büyük harf ve alt tire olmadan.
- Çağrıldığı yerde yeniden isimlendirmeye gerek olmadan.
- Kısa ve öz. Adın her çağrıldığı yerde tam olarak tanımlandığını unutmayın.
- Çoğul değil. Örnek olarak, `net/url`, `net/urls` değil.
- "common", "util", "shared", ve "lib" gibi değil. Bunlar kötü ve bilgilendirmeyen isimlerdir.

[Package Names] ve [Style guideline for Go packages] konuları inceleyebilirsiniz.

[Package Names]: https://blog.golang.org/package-names
[Style guideline for Go packages]: https://rakyll.org/style-packages/

### Fonksiyon İsimleri

Biz Go Topluluğu'nun ortak düşüncesi olan [Fonksiyonlar için MixedCaps] yolunu izliyoruz. Sadece bir istisna olarak test fonksiyonlarının isimleri, ilişkili test durumlarını gruplayabilmek için alt tire içerebilir. Örnek olarak, `TestMyFunction_WhatIsBeingTested`.

[Fonksiyonlar için MixedCaps]: https://golang.org/doc/effective_go.html#mixed-caps

### Import Lakaplandırma (Aliasing)

Import lakaplandırma, paket ismi import yolunun son elemanı ile eşleşmiyorsa kullanılmalıdır.

```go
import (
  "net/http"

  client "example.com/client-go"
  trace "example.com/trace/v2"
)
```

Diğer tüm senaryolarda, içe aktarılan paketlerin isimleri çakışmadığı sürece, import lakaplandırmaktan kaçınılmalıdır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"


  nettrace "golang.net/x/trace"
)
```

</td><td>

```go
import (
  "fmt"
  "os"
  "runtime/trace"

  nettrace "golang.net/x/trace"
)
```

</td></tr>
</tbody></table>

### Fonksiyon Gruplandırma ve Sıralama

- Fonksiyon sıralaması kabası çağrılma sırasına göre yapılmalıdır.
- Bir dosyadaki fonksiyonlar alıcıya göre gruplandırılmalıdır.

Bu nedenle, dışa aktarılan fonksiyonlar bir dosyada `struct`, `const`, `var` tanımlarından sonra ilk sırada görünmelidir.

Bir `newXYZ()`/`NewXYZ()`, tür tanımlandıktan sonra, ancak alıcıdaki diğer metodlardan önce görünebilir.

Fonksiyonlar alıcıya göre gruplandırıldığından, düz utility _(fayda)_ fonksiyonları dosyanın sonuna doğru sıralanmalıdır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func (s *something) Cost() {
  return calcCost(s.weights)
}

type something struct{ ... }

func calcCost(n []int) int {...}

func (s *something) Stop() {...}

func newSomething() *something {
    return &something{}
}
```

</td><td>

```go
type something struct{ ... }

func newSomething() *something {
    return &something{}
}

func (s *something) Cost() {
  return calcCost(s.weights)
}

func (s *something) Stop() {...}

func calcCost(n []int) int {...}
```

</td></tr>
</tbody></table>

### İç içeliği Azalt

Kod üzerinde, hata yakalama veya özel durumları işleme gibi yerlerde, erken return ve döngülerde iç içe yazımı mümkün olduğunca azaltın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
for _, v := range data {
  if v.F1 == 1 {
    v = process(v)
    if err := v.Call(); err == nil {
      v.Send()
    } else {
      return err
    }
  } else {
    log.Printf("Invalid v: %v", v)
  }
}
```

</td><td>

```go
for _, v := range data {
  if v.F1 != 1 {
    log.Printf("Invalid v: %v", v)
    continue
  }

  v = process(v)
  if err := v.Call(); err != nil {
    return err
  }
  v.Send()
}
```

</td></tr>
</tbody></table>

### Gereksiz Else

Eğer bir değişken, `if`'in her iki branşında da değer değişimine uğruyorsa, tek `if` branşı kullanımına düşürülebilir.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
var a int
if b {
  a = 100
} else {
  a = 10
}
```

</td><td>

```go
a := 10
if b {
  a = 100
}
```

</td></tr>
</tbody></table>

### Üst-Seviye Değişken Tanımlamaları

Üst seviyede standart `var` terimini kullanın. İfade ile aynı tür olduğu sürece, türünü belirtme.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
var _s string = F()

func F() string { return "A" }
```

</td><td>

```go
var _s = F()
// F zaten bize string döndürdüğünü belirttiğinden,
// türü tekrar belirtmemize gerek yoktur.

func F() string { return "A" }
```

</td></tr>
</tbody></table>

İfadenin türü istenen türle eşleşmiyorsa türü kesinlikle belirtin.

```go
type myError struct{}

func (myError) Error() string { return "error" }

func F() myError { return myError{} }

var _e error = F()
// F, myError türünde bir nesne döndürür, ancak biz error istiyoruz.
```

### Dışa aktarılmayan globallerin başına _ ekleyin

Eğer dışa aktarılmıyorsa, üst seviye `var` ve `const` tanımlamalarında isimlerinin başlarına `_` (alt tire) getirin.

Gerekçe: Üst düzey değişkenler ve sabitler bir paket kapsamına sahiptir. Normal şekilde kullanımı (alt tire olmadan), yanlışlıkla farklı bir dosyada yanlış değeri kullanmayı kolaylaştırır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// foo.go

const (
  defaultPort = 8080
  defaultUser = "user"
)

// bar.go

func Bar() {
  defaultPort := 9090
  ...
  fmt.Println("Default port", defaultPort)

  // Eğer Bar() fonksiyonunun ilk satırı silinirse,
  // bir derleme hatası görmeyiz.
}
```

</td><td>

```go
// foo.go

const (
  _defaultPort = 8080
  _defaultUser = "user"
)
```

</td></tr>
</tbody></table>

**İstisna**: Dışa aktarılmayan error isimlendirmesi `_` olmadan `err` şeklinde başlamalıdır.
[Error'ları İsimlendirme](#errorları-i̇simlendirme)'ye göz at.

### Struct'larda İçe Gömmek

Gömülü tipler sıra olarak struct'ın en üstünde yer almalıdır ve gömülü olan satırla diğer satırlardan boşlukla ayrılmalıdır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type Client struct {
  version int
  http.Client
}
```

</td><td>

```go
type Client struct {
  http.Client

  version int
}
```

</td></tr>
</tbody></table>

Gömme, işlevsellik eklemek veya anlamsal olarak uygun bir şekilde artırmak gibi somut faydalar sağlamalıdır. Bu, kullanıcıya yönelik sıfır olumsuz etkilerle yapmalıdır. (bkz: [Herkese Açık Structlarda Gömülü Tiplerden Kaçın]).

İstisna: Mutex'ler gömülmemelidir, hatta dışa aktarılmamış türlerde bile. Ayrıca bkz: [Sıfır-Değerli Mutex'ler Geçerlidir].

[Herkese Açık Structlarda Gömülü Tiplerden Kaçın]: #avoid-embedding-types-in-public-structs
[Sıfır-Değerli Mutex'ler Geçerlidir]: #zero-value-mutexes-are-valid

Gömmede **yapılmaması gerekenler**:

- Tamamen kozmetik _(görünüş)_ veya kolaylık odaklı olmamalıdır.
- Dış türleri inşa etmeyi _(construct)_ veya kullanmayı daha zor hale getirmemelidir.
- Dış türlerin sıfır değerlerini etkilememelidir. Dış türün yararlı bir sıfır değeri varsa, iç türü gömdükten sonra yine de yararlı bir sıfır değerine sahip olmalıdır.
- İç türü gömmenin bir yan etkisi olarak dış türden alakasız fonksiyonları veya alanları göstermemlidir.
- Dışa aktarılmamış türleri göstermemelidir.
- Dış türlerin kopyalama mantığını etkilememelidir.
- dış türün API'sini veya tür semantiğini değiştirmemelidir.
- İç türün standart olmayan bir formu gömülmemeldir.
- Dış türün uygulama (implementaston) ayrıntılarını göstermemelidir.
- Kullanıcıların tür içlerini gözlemlemesine veya kontrol etmesine izin vermemelidir.
- Kullanıcıları makul ölçüde şaşırtacak şekilde sarmalama (wrapping) yoluyla iç fonksiyonların genel davranışını değiştirmemlidir.

Basitçe söylemek gerekirse, bilinçli ve kasıtlı olarak gömün. İyi bir turnusol testi şudur: "Dışa aktarılan tüm bu iç metodlar/alanlar doğrudan dış türe eklenebilir mi"; cevap "bazı" veya "hayır" ise, iç türü gömmeyin - bunun yerine bir alan olarak isimlendirmeyi kullanın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
type A struct {
	// Kötü: A.Lock() ve A.Unlock() 
	// şuanda erişilebilir, fonksiyonel
	// bir yararı yok ve kullanıcılar
	// A dahilinde detayları kontrol edebilir.
    sync.Mutex
}
```

</td><td>

```go
type countingWriteCloser struct {
	// İyi: Write(), dış katmanda bir amaç
	// için sunulmuş ve iç türün Write()
	// fonksiyonu ile çalışmasını temsil
	// eder.
    io.WriteCloser

    count int
}

func (w *countingWriteCloser) Write(bs []byte) (int, error) {
    w.count += len(bs)
    return w.WriteCloser.Write(bs)
}
```

</td></tr>
<tr><td>

```go
type Book struct {
    // Kötü: işaretçi sıfır değer kullanışlılığını değiştirir
    io.ReadWriter

    // diğer alanlar
}

// daha sonra

var b Book
b.Read(...)  // panic: nil pointer
b.String()   // panic: nil pointer
b.Write(...) // panic: nil pointer
```

</td><td>

```go
type Book struct {
    // Good: kullanışlı sıfır-değerine sahip
    bytes.Buffer

    // diğer alanlar
}

// daha sonra

var b Book
b.Read(...)  // ok
b.String()   // ok
b.Write(...) // ok
```

</td></tr>
<tr><td>

```go
type Client struct {
    sync.Mutex
    sync.WaitGroup
    bytes.Buffer
    url.URL
}
```

</td><td>

```go
type Client struct {
    mtx sync.Mutex
    wg  sync.WaitGroup
    buf bytes.Buffer
    url url.URL
}
```

</td></tr>
</tbody></table>

### Yerel Değişken Tanımlamaları

Bir değişken açıkça bir değere tanımlanıyorsa, kısa değişken bildirimleri (`:=`) kullanılmalıdır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
var s = "foo"
```

</td><td>

```go
s := "foo"
```

</td></tr>
</tbody></table>

Ancak, `var` terimi kullanıldığında varsayılan değerin daha net olduğu durumlar vardır.
 Örneğin, [Boş Slices Tanımlama].

[Boş Slices Tanımlama]: https://github.com/golang/go/wiki/CodeReviewComments#declaring-empty-slices

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
func f(list []int) {
  filtered := []int{}
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td><td>

```go
func f(list []int) {
  var filtered []int
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td></tr>
</tbody></table>

### nil geçerli bir slice'dır

`nil`, 0 uzunlukta geçerli bir slice'dır. Bunun anlamı,

- Açıkça sıfır uzunluğunda bir slice döndürmemeisiniz. Bunu yerine `nil` döndürün.

  <table>
  <thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  if x == "" {
    return []int{}
  }
  ```

  </td><td>

  ```go
  if x == "" {
    return nil
  }
  ```

  </td></tr>
  </tbody></table>

- Bir slice'ın boş olup olmadığını `nil` ile kontrol etmek yerine `len(s) == 0` kullanın.

  <table>
  <thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  func isEmpty(s []string) bool {
    return s == nil
  }
  ```

  </td><td>

  ```go
  func isEmpty(s []string) bool {
    return len(s) == 0
  }
  ```

  </td></tr>
  </tbody></table>

- Sıfır değeri (`var` ile bildirilen bir slice), `make` ile üretilmezse hemen kullanılabilir.

  <table>
  <thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  nums := []int{}
  // ya da, nums := make([]int)

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td><td>

  ```go
  var nums []int

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td></tr>
  </tbody></table>
Şunu unutmayın, geçerli bir sllice olmasına rağmen, bir nil slice tahsis edilmiş 0 uzunlukta bir slice'a eş değildir _(biri nil diğeri değil)_ ve ikisi farklı durumlarda farklı şekilde ele alınabilir. _(serialization gibi)_

### Değişkenlerin Kapsamını Azalt

Mümkün olduğu yerde, değişkenlerin kapsamını azaltın. Fakat [iç içeliği azaltma](#i̇ç-içeliği-azalt) ile çelişiyorsa kapsamı azaltmayın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
err := os.WriteFile(name, data, 0644)
if err != nil {
 return err
}
```

</td><td>

```go
if err := os.WriteFile(name, data, 0644); err != nil {
 return err
}
```

</td></tr>
</tbody></table>

Eğer `if` dışında bir fonksiyon çağrısının sonucuna ihtiyacınız varsa, kapsamı daraltmaya çalışmamalısınız.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
if data, err := os.ReadFile(name); err == nil {
  err = cfg.Decode(data)
  if err != nil {
    return err
  }

  fmt.Println(cfg)
  return nil
} else {
  return err
}
```

</td><td>

```go
data, err := os.ReadFile(name)
if err != nil {
   return err
}

if err := cfg.Decode(data); err != nil {
  return err
}

fmt.Println(cfg)
return nil
```

</td></tr>
</tbody></table>

### Çıplak Parametrelerden Kaçının

Fonksiyon çağrılarındaki çıplak parametreler okunabilirliğe zarar verebilir.
Naked parameters in function calls can hurt readability. Anlamları açık olmadığında parametre adları için C stili yorumlar (`/* ... */`) ekleyin.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true, true)
```

</td><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true /* isLocal */, true /* done */)
```

</td></tr>
</tbody></table>

Daha da iyisi, daha okunabilir ve tür açısından güvenli kod için çıplak "bool" türlerini özel türlerle değiştirin. Bu, gelecekte o parametre için ikiden fazla duruma (true/false) izin verir.

```go
type Region int

const (
  UnknownRegion Region = iota
  Local
)

type Status int

const (
  StatusReady Status = iota + 1
  StatusDone
  // Belki gelecekte bir StatusInProgress'imiz olacak.
)

func printInfo(name string, region Region, status Status)
```

### Escape'ten Kaçınmak için Raw String Literals Kullanın

Go, birden çok satıra yayılabilen ve tırnak işaretleri içeren [raw string literals](https://golang.org/ref/spec#raw_string_lit)'ı destekler. Bunları, okunması çok daha zor olan elle Escape string'lerden kaçınmak için kullanın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
wantError := "unknown name:\"test\""
```

</td><td>

```go
wantError := `unknown error:"test"`
```

</td></tr>
</tbody></table>

### Struct'ları Başlatma

#### Struct'ları başlatmak için alan isimlerini kullanın

Struct'ları başlatırken hemen hemen her zaman alan adlarını belirtmelisiniz. Bu artık [`go vet`] tarafından uygulanmaktadır.

[`go vet`]: https://golang.org/cmd/vet/

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
k := User{"John", "Doe", true}
```

</td><td>

```go
k := User{
    FirstName: "John",
    LastName: "Doe",
    Admin: true,
}
```

</td></tr>
</tbody></table>

İstisna: 3 veya daha az alan olduğunda, test tablolarında alan adları *atlanabilir*.

```go
tests := []struct{
  op Operation
  want string
}{
  {Add, "add"},
  {Subtract, "subtract"},
}
```

#### Struct'larda Sıfır Değer Alanlarını Atla

Alan adlarıyla struct'ları başlatırken, anlamı olmadığı sürece sıfır değeri olan alanları atlayın. Böyle yaparak, Go'nun bunları otomatik olarak sıfır değerlerine ayarlamasına izin verin.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
  MiddleName: "",
  Admin: false,
}
```

</td><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
}
```

</td></tr>
</tbody></table>

Bu sayede varsayılan değerleri atlayarak okuyucular için görüntü kalabalığı azaltılmasına yardımcı olunabilir. Yalnızca sıfır değer olmayacak alanları değiştirin.

Alanın sıfır değerinde olduğunun özellikle dikkat çekmek için belirtilmesi gerektiği yerlerde sıfır değeri belirtilebilir. Örneğin, test tablolarındaki test senaryoları, sıfır değerli olsa bile alan adlarında belirtilebilir.

```go
tests := []struct{
  give string
  want int
}{
  {give: "0", want: 0},
  // ...
}
```

#### Sıfır Değerli Struct'lar için `var` Terimini Kullanın

Bir struct'tın tüm alanları bir bildirimde atlandığında, yapıyı bildirmek için `var` terimini kullanın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{}
```

</td><td>

```go
var user User
```

</td></tr>
</tbody></table>

Bu, sıfır değerli struct'ları, [map başlatma] için oluşturulan ayrıma benzer olarak sıfır değerli olmayan alanlara sahip olmayanlardan ayırır ve nasıl boş slice tanımlama'da böyle bir tercih yapıyorsak, aynı sebebtendir.

[map başlatma]: #initializing-maps

#### Struct Referanslarını Başlatma

Struct referanslarını başlatırken, struct başlatma ile tutarlı olması için `new(T)` yerine `&T{}` kullanın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
sval := T{Name: "foo"}

// tutarsız
sptr := new(T)
sptr.Name = "bar"
```

</td><td>

```go
sval := T{Name: "foo"}

sptr := &T{Name: "bar"}
```

</td></tr>
</tbody></table>

### Map Başlatma

Boş map'ler ve programlı olarak doldurulmuş map'ler için `make(..)`'i tercih edin. Bu, map başlatmayı bildirimden görsel olarak farklı kılar ve varsa daha sonra boyut _(capasity)_ eklemeyi kolaylaştırır.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
var (
  // m1, okumak ve yazmak için güvelidir
  // m2, yazarken panic oluşturur.
  m1 = map[T1]T2{}
  m2 map[T1]T2
)
```

</td><td>

```go
var (
  // m1, okumak ve yazmak için güvelidir
  // m2, yazarken panic oluşturur.
  m1 = make(map[T1]T2)
  m2 map[T1]T2
)
```

</td></tr>
<tr><td>

Bildirim ve başlatma görsel olarak benzerdir.

</td><td>

Bildirim ve başlatma görsel olarak farklıdır.

</td></tr>
</tbody></table>

Mümkün olduğunda, map'leri `make()` ile başlatırken kapasite ipuçları sağlayın. 
[Map Kapasite İpuçları Belirtme](#map-kapasitesi-i̇puçlarını-belirtme)
bölümünü okuyarak daha fazla bilgi edinebilirsiniz.

Öte yandan, map sabit bir öğe listesi içeriyorsa, map başlatmak için map değişmezlerini (literals) kullanın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[T1]T2, 3)
m[k1] = v1
m[k2] = v2
m[k3] = v3
```

</td><td>

```go
m := map[T1]T2{
  k1: v1,
  k2: v2,
  k3: v3,
}
```

</td></tr>
</tbody></table>


Temel kural, başlatma zamanında sabit bir öğe kümesi eklerken map değişmezlerini kullanmaktır, aksi takdirde `make` kullanın (ve varsa bir boyut ipucu belirtin).

### Printf Dışındaki Format String'leri

Eğer `Printf` fonksiyonu dışında bir format string'i tanımladıysanız, bunu `const` ile sabit hale getirin.

Bu, `go vet`'in format string'de statik analiz yapmasına yardımcı olur.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
msg := "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td><td>

```go
const msg = "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td></tr>
</tbody></table>

### Printf-Stili Fonksiyonları İsimlendirme

`Printf`-stili bir fonksiyon bildirimi yaptığınızda, `go vet`'in bunu algılayabildiğinden emin olun ve format string'ini kontrol edin.

Bu, mümkünse önceden tanımlanmış `Printf`-stili fonksiyon adlarını kullanmanız gerektiği anlamına gelir. `go vet` varsayılan olarak bunları kontrol edecektir. Daha fazla bilgi için [Printf Family] bölümüne bakın.

[Printf ailesi]: https://golang.org/cmd/vet/#hdr-Printf_family

Önceden tanımlanmış adları kullanmak bir seçenek değilse, seçtiğiniz adı **f** ile bitirin: `Wrapf`, `Wrap` değil. `go vet`'ten belirli `Printf`-stili adları kontrol etmesi istenebilir, ancak bunlar **f** ile bitmelidir.

```shell
$ go vet -printfuncs=wrapf,statusf
```

Bkz: [go vet: Printf family check].

[go vet: Printf family check]: https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/

## Desenler

### Test Tabloları

Temel test mantığı tekrarlandığında kodun kopyalanmasını önlemek için [alt testler] ile tabloya dayalı _(table-driven)_ testleri kullanın.

[alt testler]: https://blog.golang.org/subtests

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestSplitHostPort(t *testing.T)

host, port, err := net.SplitHostPort("192.0.2.0:8000")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("192.0.2.0:http")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "http", port)

host, port, err = net.SplitHostPort(":8000")
require.NoError(t, err)
assert.Equal(t, "", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("1:8")
require.NoError(t, err)
assert.Equal(t, "1", host)
assert.Equal(t, "8", port)
```

</td><td>

```go
// func TestSplitHostPort(t *testing.T)

tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  {
    give:     "192.0.2.0:8000",
    wantHost: "192.0.2.0",
    wantPort: "8000",
  },
  {
    give:     "192.0.2.0:http",
    wantHost: "192.0.2.0",
    wantPort: "http",
  },
  {
    give:     ":8000",
    wantHost: "",
    wantPort: "8000",
  },
  {
    give:     "1:8",
    wantHost: "1",
    wantPort: "8",
  },
}

for _, tt := range tests {
  t.Run(tt.give, func(t *testing.T) {
    host, port, err := net.SplitHostPort(tt.give)
    require.NoError(t, err)
    assert.Equal(t, tt.wantHost, host)
    assert.Equal(t, tt.wantPort, port)
  })
}
```

</td></tr>
</tbody></table>

Test tabloları, hata mesajlarına bağlam _(context)_ eklemeyi, yinelenen mantığı azaltmayı ve yeni test senaryoları eklemeyi kolaylaştırır.

Struct slice'ının `tests` ve her bir test senaryosunun `tt` olarak adlandırılması kuralına uyuyoruz. Ayrıca, her bir test durumu için girdi ve çıktı değerlerinin `give` ve `want` önekleriyle açıklanmasını teşvik ediyoruz.

```go
tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  // ...
}

for _, tt := range tests {
  // ...
}
```

Bazı özel döngüler gibi paralel testler (örneğin, döngü gövdesinin bir parçası olarak goroutinler oluşturan veya referansları yakalayanlar), beklenen değerleri tuttuklarından emin olmak için döngü kapsamı içinde döngü değişkenlerini açıkça atamaya özen göstermelidir.

```go
tests := []struct{
  give string
  // ...
}{
  // ...
}

for _, tt := range tests {
  tt := tt // t.Parallel için
  t.Run(tt.give, func(t *testing.T) {
    t.Parallel()
    // ...
  })
}
```

Yukarıdaki örnekte, aşağıdaki `t.Parallel()` kullanımından dolayı, döngü yineleme kapsamına alınmış bir `tt` değişkeni bildirmeliyiz. Bunu yapmazsak, testlerin çoğu veya tümü `tt` için beklenmeyen bir değer veya çalışırken değişen bir değer alır.

### Fonksiyonel Seçenekler _(Options)_

Fonksiyonel seçenekler _(options)_, bazı dahili struct'lardaki bilgileri kaydeden opak bir `Option` türü bildirdiğiniz bir kalıptır. Bu seçeneklerin değişken bir sayısını _(variadic)_ kabul edersiniz ve dahili struct'taki seçenekler tarafından kaydedilen tüm bilgilere göre hareket edersiniz.

Özellikle bu fonksiyonlar üzerinde zaten üç veya daha fazla argümanınız varsa, genişletilmesi gerektiğini öngördüğünüz yapıcılarda _(constructors)_ ve diğer genel API'lerde isteğe bağlı argümanlar için bu kalıbı kullanın.

<table>
<thead><tr><th>Kötü</th><th>İyi</th></tr></thead>
<tbody>
<tr><td>

```go
// package db

func Open(
  addr string,
  cache bool,
  logger *zap.Logger
) (*Connection, error) {
  // ...
}
```

</td><td>

```go
// package db

type Option interface {
  // ...
}

func WithCache(c bool) Option {
  // ...
}

func WithLogger(log *zap.Logger) Option {
  // ...
}

// Open bir bağlantı yaratır.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  // ...
}
```

</td></tr>
<tr><td>

Kullanıcı varsayılanı kullanmak istese bile, önbellek _(cache)_ ve kaydedici _(logger)_ parametreleri her zaman sağlanmalıdır.

```go
db.Open(addr, db.DefaultCache, zap.NewNop())
db.Open(addr, db.DefaultCache, log)
db.Open(addr, false /* cache */, zap.NewNop())
db.Open(addr, false /* cache */, log)
```

</td><td>

Seçenekler _(options)_ yalnızca gerektiğinde sağlanır.

```go
db.Open(addr)
db.Open(addr, db.WithLogger(log))
db.Open(addr, db.WithCache(false))
db.Open(
  addr,
  db.WithCache(false),
  db.WithLogger(log),
)
```

</td></tr>
</tbody></table>

Bu kalıbı uygulamak için önerdiğimiz yol, dışa aktarılmamış bir metodu tutan, seçenekleri dışa aktarılmamış bir `options` struct'ına kaydeden bir `Options` arabirimidir _(interface)_.

```go
type options struct {
  cache  bool
  logger *zap.Logger
}

type Option interface {
  apply(*options)
}

type cacheOption bool

func (c cacheOption) apply(opts *options) {
  opts.cache = bool(c)
}

func WithCache(c bool) Option {
  return cacheOption(c)
}

type loggerOption struct {
  Log *zap.Logger
}

func (l loggerOption) apply(opts *options) {
  opts.logger = l.Log
}

func WithLogger(log *zap.Logger) Option {
  return loggerOption{Log: log}
}

// Open bir bağlantı yaratır.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  options := options{
    cache:  defaultCache,
    logger: zap.NewNop(),
  }

  for _, o := range opts {
    o.apply(&options)
  }

  // ...
}
```

Bu tasarımı kapanışlar _(closures)_ ile uygulamanın bir yöntemi olduğunu unutmayın, ancak yukarıdaki kalıbın yazarlar için daha fazla esneklik sağladığına ve kullanıcılar için daha kolay hata ayıklama _(debug)_ ve test yazma imkanı olduğuna inanıyoruz.

Özellikle, testlerde ve denemelerde seçeneklerin birbirleriyle, bunun imkansız olduğu durumlarda kapanışlara _(closure)_ karşı karşılaştırılmasına izin verir. Ayrıca, seçeneklerin kullanıcılar tarafından okunabilen string temsillerine izin veren `fmt.Stringer` dahil olmak üzere diğer arabirimleri _(interface)_ uygulamasına izin verir.

Ayrıca bkz,

- [Self-referential functions and the design of options]
- [Functional options for friendly APIs]

  [Self-referential functions and the design of options]: https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html
  [Functional options for friendly APIs]: https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->

## Linting

Aşağıdaki satırları minimumda kullanmanızı öneririz, çünkü bunların en yaygın sorunları yakalamaya yardımcı olduğunu ve ayrıca gereksiz yere kuralcı olmadan kod kalitesi için yüksek bir seviye oluşturduğunu düşünüyoruz.:

- [errcheck] : error'ların kontrol edildiğinden emin olmak için
- [goimports] : kod formatlama ve otomatik içe aktarma _(import)_ ekleme
- [golint] : sıkça yapılan stil hataları için uyarı gösterme
- [govet] : sıkça yapılan hatalar için kod analizi yapma
- [staticcheck] : çeşitli statik analiz kontrolleri yapma

  [errcheck]: https://github.com/kisielk/errcheck
  [goimports]: https://godoc.org/golang.org/x/tools/cmd/goimports
  [golint]: https://github.com/golang/lint
  [govet]: https://golang.org/cmd/vet/
  [staticcheck]: https://staticcheck.io/


### Lint Çalıştırıcılar (Runners)

Büyük ölçüde daha büyük kod tabanlarındaki performansı ve birçok kurallı linter'ı aynı anda yapılandırma ve kullanma yeteneği nedeniyle Go kodu için go-to lint çalıştırıcısı olarak [golangci-lint]'i öneriyoruz. Bu depo, önerilen linter'lar ve ayarlarla örnek bir [.golangci.yml] yapılandırma dosyasına sahiptir.

golangci-lint'in kullanıma hazır [çeşitli linter'lar] vardır. Yukarıdaki linter'lar temel set olarak önerilir ve ekipleri, projeleri için anlamlı olan ek linter'lar eklemeye teşvik ediyoruz.

[golangci-lint]: https://github.com/golangci/golangci-lint
[.golangci.yml]: https://github.com/uber-go/guide/blob/master/.golangci.yml
[çeşitli linter'lar]: https://golangci-lint.run/usage/linters/
