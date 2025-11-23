# Antigravity IDE - Best Practices

Bu doküman Antigravity IDE kullanırken dikkat edilmesi gereken en iyi uygulamaları içerir.

## 🔒 1. Sürüm Kontrolü (GIT) - ZORUNLU

### Neden Önemli?
Ajanlar güçlü ama hata yapabilir. Bir adımda beklenmedik bir değişiklik yapması durumunda geri dönebilmelisin.

### Öneriler:
- ✅ Her proje için **mutlaka Git kullan**
- ✅ Agent çalıştırmadan önce **commit yap**
- ✅ Düzenli olarak **yedek al**
- ✅ Önemli milestone'larda **branch oluştur**
- ✅ `.gitignore` dosyasını doğru yapılandır

```bash
# Her agent çalıştırmadan önce:
git status
git add .
git commit -m "Before agent task: [task description]"

# Agent tamamladıktan sonra:
git diff  # Değişiklikleri incele
git add .
git commit -m "After agent task: [task description]"
```

---

## 🧠 2. Kodu Anlamaya Devam Et

### Kural: Agent'ın Çıktısını Sorgusuz Kabul Etme

Ajanlar güçlü araçlardır, sihirli değnek değildir.

### Yapılması Gerekenler:
- ✅ **İncele**: Agent'ın yazdığı her kodu oku
- ✅ **Doğrula**: Mantık hataları var mı kontrol et
- ✅ **Yorum Yap**: Artifact'lara yorum bırak, feedback ver
- ✅ **Test Et**: Agent'ın test ettiğini sen de test et
- ✅ **Anla**: Kodun ne yaptığını mutlaka anla

```
❌ YANLIŞ: "Agent yaptı, çalışıyorsa sorun yok"
✅ DOĞRU: "Agent yaptı, kod mantıklı mı bakayım, test edeyim"
```

---

## 🚦 3. Kademeli Kullanım

### Henüz Önizleme Aşamasında

Antigravity hala **public preview** döneminde. Hatalar olabilir.

### Öneriler:
- 🟢 **Küçük Projelerle Başla**: İlk önce yan projelerde dene
- 🟡 **Kritik Production'a Dikkatli Taşı**: Sınırlarını gör, sonra karar ver
- 🔴 **Mission-Critical İşlerde Yedeksiz Kullanma**: Mutlaka backup stratejisi olsun

### İdeal Kullanım Senaryoları:
1. Prototip geliştirme
2. Boilerplate kod oluşturma
3. Test yazma
4. Dokümantasyon oluşturma
5. Refactoring işlemleri
6. UI iteration (hızlı tasarım denemeleri)

---

## ⚡ 4. Oran Sınırlarını Bil

### Mevcut Limitleri:
- **Gemini 3 Pro**: Generous rate limit (cömert ama sınırlı)
- **Diğer modeller**: Daha konservatif limit
- **Yenilenme**: Her 5 saatte bir

### İpuçları:
- Büyük projelerde sınıra takılabilirsin
- Basit görevler için **Fast mode** kullan (daha az token)
- Karmaşık görevler için **Planning mode** kullan (daha çok token ama daha kaliteli)
- Rate limit dolduğunda 5 saat beklemen gerekebilir

```
💡 TİP: Küçük görevleri Fast mode'da yap, rate limit'i koru.
      Büyük refactoring'lerde Planning mode'u kullan.
```

---

## 🔄 5. Agent Workflow - En İyi Akış

### Önerilen İş Akışı:

```
1. GIT COMMIT → 2. AGENT TASK → 3. REVIEW → 4. TEST → 5. GIT COMMIT
```

#### Adım 1: Başlamadan Önce
```bash
git status
git commit -m "Clean state before agent task"
```

#### Adım 2: Agent'a Görev Ver
- Net ve spesifik talimatlar ver
- Örnek: ❌ "Bir uygulama yap" → ✅ "React ile TODO uygulaması yap, local storage kullan, CRUD işlemleri olsun"

#### Adım 3: Artifact'ları İncele
- Implementation Plan'ı oku
- Yorum bırak, feedback ver
- Gerekirse "bu kısmı şöyle yap" de

#### Adım 4: Kodu İncele
- Review Changes panelini aç
- Dosya dosya değişiklikleri gör
- Mantık hataları var mı kontrol et

#### Adım 5: Test Et
- Agent'ın test ettiği senaryoları sen de test et
- Edge case'leri dene
- Browser'da gerçekten çalışıyor mu kontrol et

#### Adım 6: Commit Et
```bash
git add .
git commit -m "Agent task completed: [description]"
```

---

## 🎯 6. Çoklu Agent Kullanımı

### Paralel Agent Stratejisi

Büyük projelerde birden fazla agent çalıştırabilirsin:

```
Agent 1: UI geliştirme
Agent 2: Test yazma
Agent 3: Bug fixing
```

### İpuçları:
- Her agent için **ayrı workspace** kullan (Agent Manager'da)
- **Inbox**'u kullanarak tüm agent'ların durumunu takip et
- Pending approval'ları düzenli kontrol et

---

## 🧪 7. Planning vs Fast Mode

### Ne Zaman Planning Mode?
- ✅ Kompleks görevler
- ✅ Araştırma gerektiren işler
- ✅ Çok dosyalı değişiklikler
- ✅ Artifact'lara ihtiyaç var
- ✅ Kalite > Hız

**Özellikler:**
- Task Groups oluşturur
- Artifact üretir (plan, diagram, walkthrough)
- Daha detaylı düşünür
- Daha çok token harcar

### Ne Zaman Fast Mode?
- ✅ Basit görevler
- ✅ Değişken yeniden adlandırma
- ✅ Birkaç bash komutu
- ✅ Küçük, lokalize değişiklikler
- ✅ Hız > Detay

**Özellikler:**
- Direkt execute eder
- Artifact üretmez
- Daha az token harcar
- Daha hızlı

---

## 🛡️ 8. Güvenlik Best Practices

### Agent Non-Workspace File Access
Varsayılan olarak agent sadece workspace dosyalarına erişir.

⚠️ **DİKKAT**: Settings'ten "Agent Non-Workspace File Access" açarsan:
- Agent workspace dışındaki dosyaları görebilir
- **Risk**: Secret dosyalar, `.env`, credentials açığa çıkabilir
- **Tavsiye**: Gerekmedikçe açma

### Terminal Auto Execution
```
OFF: Terminal komutlarını hiç otomatik çalıştırma
AUTO: Agent karar verir
TURBO: Her komutu otomatik çalıştır
```

**Tavsiye:**
- Güvenli işler için: **TURBO**
- Karışık işler için: **AUTO**
- Kritik production'da: **OFF** + Allow list yapılandır

---

## 📊 9. Artifact Review Policy

### 3 Seçenek:

| Policy | Ne Zaman Kullan |
|--------|-----------------|
| **Always Proceed** | Hızlı iteration, agent'a güveniyorsun |
| **Agent Decides** | Dengeli yaklaşım (önerilen) |
| **Request Review** | Kritik projeler, her adımı kontrol etmek istiyorsun |

**İlk kullanımlarda:** `Request Review` ile başla, agent'ı tanı, sonra `Agent Decides`'a geç.

---

## 🔍 10. MCP Server Kullanımı

### Python MCP Serverlar İçin:
- ⚠️ `print()` kullanma, `print(..., file=sys.stderr)` kullan
- ⚠️ stdout'a log yazma
- ✅ MCP Inspector ile test et
- ✅ Sadece stdio transport kullanan serverları tercih et

### Önerilen MCP Serverlar:
- **Firebase**: Backend işlemleri için
- **BigQuery**: Veri analizi için
- **GitHub**: Repo yönetimi için
- **Supabase/Neon**: Database işlemleri için

---

## 📈 11. Performans İpuçları

### Daha Hızlı Sonuçlar İçin:
1. **Spesifik ol**: "Bir app yap" yerine "React + Tailwind ile TODO app yap"
2. **Context ver**: Hangi tech stack, hangi stil, hangi pattern
3. **Knowledge Items kullan**: Agent önceki çalışmalarını hatırlasın
4. **Artifact'lara yorum yap**: Hızlı feedback döngüsü
5. **Fast mode'u kullan**: Basit işler için

### Daha Az Token Harcamak İçin:
- Küçük görevlere böl
- Fast mode tercih et
- Gereksiz artifact üretme
- Çok uzun conversation'lardan kaçın (yeni conversation aç)

---

## 🎓 12. Öğrenme Eğrisi

### İlk Hafta:
- Küçük görevlerle başla
- Planning mode'da artifact'ları incele
- Agent'ın nasıl düşündüğünü anla

### İkinci Hafta:
- Daha büyük görevler dene
- Çoklu agent kullanmayı öğren
- Knowledge Items'ı gözlemle

### Üçüncü Hafta+:
- Kendi workflow'unu oluştur
- MCP serverları entegre et
- Browser agent ile UI testleri yap

---

## ✅ Checklist - Her Agent Görevinde

```markdown
[ ] Git commit yaptım mı?
[ ] Görev net ve spesifik mi?
[ ] Doğru mode seçtim mi? (Planning vs Fast)
[ ] Artifact'ları inceledim mi?
[ ] Kodu okudum mu?
[ ] Manuel test yaptım mı?
[ ] Git commit yapacak mıyım?
[ ] Rate limit durumumu kontrol ettim mi?
```

---

## 🚀 Özet: En Önemli 5 Kural

1. **GIT KULLAN** - Her zaman, her projede
2. **KODU ANLA** - Agent'a kör güvenme
3. **KÜÇÜK BAŞLA** - Production'a kademeli geç
4. **FEEDBACK VER** - Artifact'lara yorum yap
5. **TEST ET** - Agent'ın testini sen de yap

---

## 📚 İlgili Dokümanlar

- [Resmi Antigravity Dokümantasyonu](./ANTIGRAVITY.md)
- [Bilinen Sorunlar](./COMMON_ISSUES.md)
- [Öğrenme Notları](./NOTES.md)
