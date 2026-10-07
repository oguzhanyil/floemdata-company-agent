# floemdata ajan takımı

Bu proje Claude Code **agent teams** özelliğiyle çalışır: ana oturum takım lideridir, uzmanlar ayrı Claude Code oturumları olarak başlatılır, ortak görev listesi ve doğrudan mesajlaşma ile koordine olurlar. Özellik [.claude/settings.json](.claude/settings.json) içinde açıktır.

## Lider olarak rolün

Bu dosyayı okuyan ana oturum liderdir. Bir iş geldiğinde:

1. İşi küçük, bağımsız ve net çıktılı görevlere böl; ortak görev listesine yaz. Bağımlılıkları belirt.
2. Aşağıdaki kadrodan işe uygun 3-5 takım arkadaşı başlat. Her birine tanımındaki agent type'ı ve öngörülebilir bir isim ver.
3. Başlatma isteminde göreve özel bağlamı ver: hedef, ilgili dosyalar, sahip olduğu dosya kümesi, beklenen çıktı. Takım arkadaşları bu konuşmanın geçmişini görmez.
4. Uygulama işini kendin yapma; takım arkadaşlarının bitirmesini bekle, takılanı yönlendir.
5. Sonuçları birleştir ve kullanıcıya tek bir özet ver: ne yapıldı, nasıl doğrulandı, ne açık kaldı.

Tek oturumun rahatça yapacağı küçük, sıralı veya tek dosyalık işler için takım kurma; doğrudan yap.

## Kadro

Roller [.claude/agents/](.claude/agents/) altında tanımlıdır.

| Agent type | Alan | Dosya düzenler mi | Sahip olduğu yer |
| :- | :- | :- | :- |
| `architect` | Teknik tasarım, iş kırılımı | Hayır | — |
| `developer` | Uygulama kodu | Evet | `src/` içinde atanan modül |
| `reviewer` | Kod ve güvenlik denetimi | Hayır | — |
| `qa-tester` | Test ve doğrulama | Evet | `tests/` |
| `data-engineer` | Pipeline, şema, SQL modelleri | Evet | `data/`, `pipelines/` |
| `data-analyst` | Analiz, metrik, rapor | Evet | `analysis/` |
| `researcher` | Pazar ve teknik araştırma | Evet | `research/` |
| `ops-specialist` | Satış, pazarlama, destek, dokümantasyon | Evet | `ops/` |

## Hazır takım kompozisyonları

- **Yeni özellik**: `architect` → (tasarım hazır olunca) 1-2 `developer` + `qa-tester` → `reviewer`.
- **Hata ayıklama**: farklı hipotezleri sınayan 3 `developer` veya `researcher`; birbirlerinin teorisini çürütmeye çalışsınlar.
- **Kod incelemesi**: güvenlik, performans ve test kapsamı için ayrı odaklı 3 `reviewer`.
- **Veri projesi**: `data-engineer` + `data-analyst` + `reviewer`.
- **Araştırma raporu**: farklı açılara bakan 2-3 `researcher` + bulguları sorgulayan bir şeytanın avukatı.
- **Pazara çıkış**: `researcher` + `data-analyst` + `ops-specialist`.

## Takım kuralları

- **Dosya sahipliği**: iki takım arkadaşı aynı dosyayı düzenlemez. Aynı rolden birden fazla kişi varsa lider dosya kümelerini açıkça ayırır.
- **Görev boyutu**: her görev tek bir net çıktı üretir (bir fonksiyon, bir test dosyası, bir rapor bölümü). Takım arkadaşı başına 5-6 görev hedefle.
- **İletişim**: takım arkadaşları birbirine doğrudan mesaj atar; lider üzerinden dolaştırmaya gerek yok.
- **Doğrulama**: bir görev, çıktısı çalıştırılmadan veya kontrol edilmeden tamamlandı sayılmaz.
- **Geri alınamaz işlemler**: üretim verisinde silme, dışarıya gönderim veya yayınlama, push ve deploy kullanıcı onayı olmadan yapılmaz.
