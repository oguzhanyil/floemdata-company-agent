# floemdata ajan takımı

Bu proje Claude Code **agent teams** özelliğiyle çalışır: ana oturum takım lideridir, uzmanlar ayrı Claude Code oturumları olarak başlatılır, ortak görev listesi ve doğrudan mesajlaşma ile koordine olurlar. Özellik [.claude/settings.json](.claude/settings.json) içinde açıktır.

Bu dosyayı hem lider hem takım arkadaşları okur. Ana oturumsan lidersin ve "Lider" bölümü sana aittir. Takım arkadaşı olarak başlatıldıysan "Lider" bölümünü atla; "Organizasyon" ve "Takım kuralları" senin için geçerlidir.

## Organizasyon

Roller [.claude/agents/](.claude/agents/) altında tanımlıdır. Ad ön eki departmanı gösterir.

### IT departmanı — Yazılım Geliştirme grubu (`dev-`)

| Agent type | Rol | Sahip olduğu yer |
| :- | :- | :- |
| `dev-backend` | Backend geliştirici | `src/backend/`, `tests/backend/` |
| `dev-frontend` | Frontend geliştirici | `src/frontend/`, `tests/frontend/` |
| `dev-uiux` | UI/UX tasarımcı | `design/` |

### IT departmanı — Veri Bilimi grubu (`data-`)

| Agent type | Rol | Sahip olduğu yer |
| :- | :- | :- |
| `data-statistics` | İstatistik uzmanı | `analysis/statistics/` |
| `data-ml` | Makine öğrenmesi uzmanı | `ml/` |
| `data-collector` | Veri toplayıcı | `data/collectors/<kaynak>/` |
| `data-warehouse` | Cloud Data Warehouse uzmanı | `warehouse/` |
| `data-lakehouse` | Lakehouse / modern veri platformu uzmanı | `lakehouse/`, `docs/platform/` |
| `data-engineering` | Data Engineering / ETL / ELT uzmanı | `pipelines/` |
| `data-bi` | BI / Analytics uzmanı | `analytics/` |
| `data-bigquery` | BigQuery uzmanı | `warehouse/bigquery/` |

### İş Analizi departmanı (`ba-`)

| Agent type | Alan | Sahip olduğu yer |
| :- | :- | :- |
| `ba-erp-accounting` | ERP / Muhasebe | `business/erp-accounting/` |
| `ba-ecommerce` | E-Ticaret / E-Pazar | `business/ecommerce/` |
| `ba-project-management` | Proje / İş Yönetimi | `business/project-management/` |
| `ba-marketing` | Sosyal Medya / Pazarlama | `business/marketing/` |
| `ba-restaurant-pos` | Kafe / Restoran / POS | `business/restaurant-pos/` |
| `ba-manufacturing-iot` | Üretim / MES / IoT | `business/manufacturing-iot/` |
| `ba-finance-banking` | Finans / Bankacılık | `business/finance-banking/` |
| `ba-crm` | CRM / Müşteri İlişkileri | `business/crm/` |
| `ba-helpdesk` | Destek / Helpdesk | `business/helpdesk/` |
| `ba-devops-monitoring` | DevOps / Monitoring | `business/devops-monitoring/` |

### Araştırma

| Agent type | Rol | Sahip olduğu yer |
| :- | :- | :- |
| `market-researcher` | Pazar ve teknoloji araştırmacısı | `research/` |

## Lider

Ana oturum liderdir ve bütün departmanları yönetir. Bir iş geldiğinde:

1. İşin hangi departmanlara dokunduğunu belirle. Çoğu iş şu sırayı izler: İş Analizi gereksinimi tanımlar → Veri Bilimi veriyi getirir ve modeller → Yazılım Geliştirme ürünü yapar.
2. İşi küçük, bağımsız ve net çıktılı görevlere böl; ortak görev listesine yaz ve bağımlılıkları belirt.
3. Kadrodan yalnızca işe gereken 3-5 takım arkadaşını başlat. 22 rolün hepsini aynı anda başlatma; iş ilerledikçe işi biteni kapat, sıradakini başlat.
4. Her takım arkadaşını tablodaki agent type ile başlat ve öngörülebilir bir ad ver. Başlatma isteminde göreve özel bağlamı yaz: hedef, ilgili dosyalar, sahip olduğu dosya kümesi, beklenen çıktı, kiminle konuşacağı. Takım arkadaşları bu konuşmanın geçmişini görmez.
5. Uygulama işini kendin yapma; takım arkadaşlarının bitirmesini bekle, takılanı yönlendir.
6. Sonuçları birleştir ve kullanıcıya tek bir özet ver: ne yapıldı, nasıl doğrulandı, ne açık kaldı.

Tek oturumun rahatça yapacağı küçük, sıralı veya tek dosyalık işler için takım kurma; doğrudan yap.

### Hazır takım kompozisyonları

- **Yeni veri kaynağı entegrasyonu**: ilgili `ba-*` uzmanı (veri modeli ve eşleme) + `data-collector` + `data-engineering`.
- **Sektöre özel dashboard**: ilgili `ba-*` uzmanı (metrik tanımları) + `data-bi` + `dev-uiux` + `dev-frontend`.
- **Yeni ürün özelliği**: `dev-uiux` + `dev-backend` + `dev-frontend`; iş kuralı gerekiyorsa ilgili `ba-*` uzmanı.
- **Veri platformu kurulumu veya seçimi**: `data-lakehouse` + `data-warehouse` + `data-bigquery` + `data-engineering`.
- **Tahmin veya ML projesi**: ilgili `ba-*` uzmanı + `data-ml` + `data-statistics` + `data-engineering`.
- **Pazar taraması**: farklı kanal veya sektörlere bakan 2-3 `market-researcher` + bulguları doğrulayan ilgili `ba-*` uzmanı.
- **Yeni sektöre giriş**: `market-researcher` + ilgili `ba-*` uzmanı + `data-bi`.

## Takım kuralları

Bütün roller için geçerlidir.

- **Dosya sahipliği**: yalnızca kendi klasörünü düzenle. Başkasının dosyasında değişiklik gerekiyorsa kendin yapma, sahibine mesaj at. Aynı rolden birden fazla kişi varsa lider dosya kümelerini açıkça ayırır.
- **İletişim**: ihtiyaç duyduğun takım arkadaşına doğrudan mesaj at; lider üzerinden dolaştırma. Takımda o rol yoksa lidere bildir.
- **Görev boyutu**: her görev tek bir net çıktı üretir (bir uç, bir model, bir rapor bölümü).
- **Doğrulama**: bir görev, çıktısı çalıştırılmadan veya kontrol edilmeden tamamlandı sayılmaz. Doğrulayamadıysan bunu açıkça söyle.
- **Kaynak gösterme**: ürün özelliği, fiyat, API ayrıntısı ve mevzuat gibi değişebilen bilgileri güncel resmi kaynakla doğrula; doğrulayamadığını "doğrulanmadı" diye işaretle.
- **Bitirirken**: lidere kısa bir özet gönder; ne ürettin, nasıl doğruladın, ne açık kaldı.
- **Geri alınamaz işlemler**: üretim verisinde silme veya üzerine yazma, dışarıya gönderim veya yayınlama, push ve deploy kullanıcı onayı olmadan yapılmaz.
- **Gizlilik**: kimlik bilgilerini dosyalara yazma; kişisel ve finansal veriyi gereğinden fazla taşıma.

### İş Analizi departmanı çıktıları

`ba-*` uzmanları uygulama kodu yazmaz; kendi klasörlerinde şunları üretir:

- **Gereksinim dokümanı**: kullanıcı, ihtiyaç, kapsam, kapsam dışı, kabul kriterleri.
- **Veri eşlemesi**: kaynak sistemin varlıkları ve alanları → hedef model; anahtarlar, birimler, durum sözlükleri.
- **Metrik tanımları**: pay, payda, zaman aralığı, filtreler ve alana özgü istisnalar.
- **Süreç akışı**: mevcut ve hedef iş akışı, sorun noktaları.

Bir sistemin veri yapısını veya API'sini anlatırken belleğe güvenme; güncel resmi dokümantasyonla doğrula ve sürümünü belirt.
