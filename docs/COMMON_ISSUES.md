# Antigravity IDE - Bilinen Sorunlar ve Çözümleri

Bu doküman Antigravity IDE kullanırken karşılaşılan yaygın sorunları ve çözümlerini içerir.

## 📋 İçindekiler
- [MCP Server Sorunları](#mcp-server-sorunları)
- [Platform Spesifik Sorunlar](#platform-spesifik-sorunlar)
- [Performans Sorunları](#performans-sorunları)
- [Browser Agent Sorunları](#browser-agent-sorunları)
- [Genel Sorunlar](#genel-sorunlar)

---

## 🔌 MCP Server Sorunları

### 1. Python MCP Server: "invalid trailing data at the end of stream"

#### Hata Mesajı:
```
Error: failed to get tools: calling "tools/list": invalid trailing data at the end of stream.
Error: calling "initialize": invalid trailing data at the end of stream.
```

#### Sebep:
Python MCP serverlar **stdout**'a log veya print mesajları yazıyor. MCP protokolü sadece JSON-RPC mesajları bekler, başka bir şey stdout'a yazılırsa protokol bozulur.

#### Çözüm:

**1. Print Statements'ları Kontrol Et**
```python
# ❌ YANLIŞ - stdout'a yazıyor
print("Server starting...")
print(f"Processing: {data}")

# ✅ DOĞRU - stderr'e yaz
import sys
print("Server starting...", file=sys.stderr)
print(f"Processing: {data}", file=sys.stderr)
```

**2. Logging'i Düzelt**
```python
import logging

# ✅ DOĞRU - logging zaten stderr'e yazar
logging.basicConfig(level=logging.INFO)
logging.info("Server started")
logging.error("Error occurred")
```

**3. Kütüphane Banner'larını Kapat**
Bazı kütüphaneler başlarken banner yazdırır:
```python
# Örnek: Rich kütüphanesinin banner'ını kapat
import os
os.environ['TERM'] = 'dumb'  # Rich'in fancy output'unu devre dışı bırak
```

**4. MCP Inspector ile Test Et**
```bash
# MCP Inspector kullanarak serverı test et
npx @modelcontextprotocol/inspector python your_mcp_server.py
```

Inspector, stdout'a ne yazıldığını gösterir. Sadece JSON-RPC mesajları olmalı.

---

### 2. Python MCP Server: EOF Error

#### Hata Mesajı:
```
Error: calling "initialize": EOF.
```

#### Sebep:
- Server processi beklenmedik şekilde sonlanıyor
- Environment değişkenleri eksik (API key gibi)
- Python modül hatası

#### Çözüm:

**1. Server Loglarını Kontrol Et**
```bash
# Server'ı manuel çalıştır, hataları gör
python your_mcp_server.py 2> error.log
cat error.log
```

**2. Environment Değişkenlerini Ekle**
```json
// mcp_config.json
{
  "mcpServers": {
    "your-server": {
      "command": "python",
      "args": ["path/to/server.py"],
      "env": {
        "API_KEY": "your-api-key-here",
        "DEBUG": "true"
      }
    }
  }
}
```

**3. Python Environment Kontrolü**
```bash
# Doğru Python interpreter'ı kullanıyor musun?
which python
python --version

# Gerekli paketler yüklü mü?
pip list | grep mcp
pip install mcp
```

---

### 3. MCP Server Antigravity'de Görünmüyor

#### Çözüm:

**1. mcp_config.json Formatını Kontrol Et**
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "package-name"],
      "env": {}
    }
  }
}
```

**2. Serverı Restart Et**
- Antigravity'yi tamamen kapat
- Yeniden aç
- MCP Store'dan serverı kontrol et

**3. Logs Kontrol Et**
Settings → Developer → Open Logs → MCP bölümünü incele

---

### 4. Python MCP Server: SSE Connection Error

#### Hata:
```
Error: SSE connection not established
```

#### Çözüm:
Antigravity **stdio transport** kullanır, SSE değil.

```json
// ❌ YANLIŞ - SSE transport
{
  "transport": "sse"
}

// ✅ DOĞRU - stdio transport
{
  "command": "python",
  "args": ["server.py"]
}
```

---

## 🖥️ Platform Spesifik Sorunlar

### 1. macOS: Oturum Açma Sorunu

#### Sorun:
Mac kullanıcıları authentication sorunları yaşıyor.

#### Çözüm:

**1. Gmail Hesabı Kullan**
```
❌ Workspace Google hesabı (örn: user@company.com)
✅ Personal Gmail hesabı (örn: user@gmail.com)
```

Google Antigravity şu anda Workspace hesaplarını tam desteklemiyor.

**2. Tarayıcı Cache Temizle**
- Safari/Chrome cache'i temizle
- Antigravity'yi yeniden başlat
- Tekrar login dene

**3. Keychain Reset**
```bash
# Antigravity keychain'ini sıfırla (dikkatli!)
rm -rf ~/Library/Keychains/Antigravity*
```

---

### 2. macOS: Rosetta 2 (Intel Chip)

#### Sorun:
X86 (Intel) Mac desteklenmiyor.

#### Geçici Çözüm:
```bash
# Rosetta 2 ile çalıştırmayı dene
arch -x86_64 /Applications/Antigravity.app/Contents/MacOS/Antigravity
```

**Not:** Resmi destek yok, sorunlar çıkabilir.

---

### 3. Linux: glibc Version Error

#### Hata:
```
version `GLIBC_2.28' not found
```

#### Çözüm:

**1. Sistem Gereksinimlerini Kontrol Et**
```bash
ldd --version  # glibc versiyonunu kontrol et
```

Gereksinim: **glibc >= 2.28**

**2. Distro Upgrade**
- Ubuntu 18.04 → Ubuntu 20.04+
- Debian 9 → Debian 10+
- Fedora 35 → Fedora 36+

---

### 4. Windows: Uç Birim (Terminal) Sorunları

#### Sorun:
PowerShell komutları düzgün çalışmıyor.

#### Çözüm:

**1. Execution Policy**
```powershell
# PowerShell'i admin olarak aç
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**2. WSL2 Kullan**
Linux komutları için WSL2 kullanmak daha stabil:
```bash
# Settings → Terminal → Default Shell → WSL2
```

---

## ⚡ Performans Sorunları

### 1. Rate Limit'e Takıldım

#### Hata:
```
Error: Rate limit exceeded. Please try again in X hours.
```

#### Çözüm:

**1. Bekle**
- Rate limit her **5 saatte bir** yenilenir
- Timer'ı kontrol et, ne kadar kaldığını gör

**2. Model Değiştir**
Gemini 3 Pro limit dolduğunda:
```
Gemini 3 Pro → Claude Sonnet 4.5
veya
Gemini 3 Pro → GPT-OSS
```

**3. Fast Mode Kullan**
Planning mode daha çok token harcar:
```
Planning mode → Fast mode (basit görevler için)
```

**4. Görevleri Böl**
Büyük bir görev yerine 3-4 küçük göreve böl.

---

### 2. Antigravity Yavaş Çalışıyor

#### Sebep:
- Yetersiz RAM
- Çok büyük workspace
- Çok fazla extension

#### Çözüm:

**1. RAM Kontrolü**
```
Minimum: 8 GB
Önerilen: 16 GB
```

**2. Workspace Boyutunu Küçült**
```bash
# node_modules gibi büyük klasörleri .gitignore'a ekle
echo "node_modules/" >> .gitignore
echo "dist/" >> .gitignore
echo "build/" >> .gitignore
```

**3. Extension'ları Azalt**
Settings → Extensions → Gereksiz olanları devre dışı bırak

**4. Logs Temizle**
Settings → Developer → Clear Logs

---

### 3. Agent Uzun Süre Cevap Vermiyor

#### Çözüm:

**1. Cancel Edip Restart**
```
1. Cancel tuşuna bas
2. "Agent stuck, trying to restart" mesajını bekle
3. Görevi yeniden başlat
```

**2. Conversation'ı Yeniden Başlat**
Çok uzun conversation'lar yavaşlatır:
```
Eski conversation: 50+ mesaj → Yavaş
Yeni conversation: Temiz başlangıç → Hızlı
```

**3. Network Kontrolü**
```bash
# Bağlantıyı test et
ping google.com
```

Gemini 3 Pro cloud'da çalışır, internet gerekir.

---

## 🌐 Browser Agent Sorunları

### 1. Chrome Extension Yüklenmedi

#### Çözüm:

**1. Manuel Yükleme**
```
1. Antigravity'de Chrome ikonuna tıkla
2. Tarayıcı açılacak
3. Chrome Web Store'dan extension'ı yükle
4. Antigravity'yi restart et
```

**2. Extension URL**
```
chrome://extensions/
→ Developer mode ON
→ "Load unpacked" ile manuel yükle
```

---

### 2. Browser URL Denylist Hatası

#### Hata:
```
Error: URL is on the denylist and cannot be accessed.
```

#### Sebep:
Google Superroots BadUrlsChecker servisi URL'yi tehlikeli buluyor.

#### Çözüm:

**1. Allowlist'e Ekle**
Settings → Browser → Allowlist → URL ekle

**2. Güvenli URL Kullan**
Localhost dışında:
```
✅ https://example.com
❌ http://suspicious-site.com
```

---

### 3. Browser Subagent Stuck (Takılı Kaldı)

#### Sorun:
Browser agent bir sayfada takılı kalıyor.

#### Çözüm:

**1. Browser'ı Kapat**
```
Chrome separate profile'ı tamamen kapat
Antigravity'de Chrome ikonuna tekrar tıkla
```

**2. Task'ı Cancel Et**
Agent Manager → Cancel running task

**3. Browser Cache Temizle**
```
Chrome (Antigravity profile) → Settings → Clear browsing data
```

---

## 🐛 Genel Sorunlar

### 1. Antigravity Crash (Çöküyor)

#### Çözüm:

**1. Logs'u Kontrol Et**
```
Settings → Developer → Open Logs
Son crash logunu incele
```

**2. Clean Reinstall**
```bash
# macOS
rm -rf ~/Library/Application\ Support/Antigravity
rm -rf ~/.antigravity

# Linux
rm -rf ~/.config/Antigravity
rm -rf ~/.antigravity

# Windows
%APPDATA%\Antigravity klasörünü sil
```

Sonra yeniden yükle.

**3. Safe Mode**
```
Antigravity'yi extension'sız başlat:
Settings → Developer → Disable all extensions
```

---

### 2. Git Integration Çalışmıyor

#### Sorun:
Source control paneli boş.

#### Çözüm:

**1. Git Yüklü mü?**
```bash
git --version
```

Değilse: https://git-scm.com/downloads

**2. Workspace Git Repo mu?**
```bash
cd /path/to/workspace
git init  # Eğer repo değilse
```

**3. Git Config**
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

### 3. Knowledge Items Güncellenmiyor

#### Sorun:
Conversation'dan knowledge item oluşmuyor.

#### Çözüm:

**1. Planning Mode Kullan**
Knowledge items sadece **Planning mode**'da oluşur.

**2. Yeterli Context Ver**
Kısa "hello world" görevler knowledge oluşturmaz. Daha detaylı, öğrenilebilir görevler ver.

**3. Manuel Knowledge Oluştur**
Agent Manager → Knowledge → "+" → Manuel knowledge item ekle

---

### 4. Artifact Görünmüyor

#### Çözüm:

**1. Planning Mode Aç**
Fast mode'da artifact üretilmez.

**2. Artifact Review Policy**
```
Settings → Agent → Artifact Review Policy
"Request Review" veya "Agent Decides" seçili olmalı
```

**3. Conversation View**
Agent Manager → Changes Sidebar → Artifact'ları kontrol et

---

### 5. Terminal Komutları Auto-Execute Olmuyor

#### Çözüm:

**1. Settings Kontrolü**
```
Settings → Agent → Terminal Command Auto Execution
"Auto" veya "Turbo" seçili olmalı
```

**2. Allow List Yapılandır**
```
Settings → Agent → Terminal Allow List
İzin vermek istediğin komutları ekle:
npm
python
node
```

**3. Deny List Kontrol Et**
Komut deny list'te olabilir:
```
Settings → Agent → Terminal Deny List
```

---

## 🔄 Genel Troubleshooting Adımları

Her sorun için bu adımları dene:

### 1. Restart Sequence
```
1. Antigravity'yi tamamen kapat
2. 10 saniye bekle
3. Yeniden aç
4. Görevi tekrar dene
```

### 2. Log Kontrolü
```
Settings → Developer → Open Logs
Son hata mesajlarını incele
```

### 3. Clean State
```bash
# Git'te clean state'e dön
git status
git stash  # Değişiklikleri sakla
git clean -fd  # Untracked dosyaları sil
```

### 4. Update Check
```
Help → Check for Updates
En son versiyonu kullanıyor musun?
```

### 5. Community'ye Sor
```
GitHub Issues: github.com/google/antigravity (eğer varsa)
Email: antigravity-support@google.com
```

---

## 📊 Sorun Rapor Etme Template

Bir sorunla karşılaştığında şu bilgileri topla:

```markdown
**Antigravity Versiyonu:**
Help → About → Version

**Platform:**
macOS 14.2 / Windows 11 / Ubuntu 22.04

**Sorun:**
[Kısa açıklama]

**Adımlar:**
1. ...
2. ...
3. ...

**Beklenen Davranış:**
[Ne olmasını bekliyordun]

**Gerçek Davranış:**
[Ne oldu]

**Logs:**
[Settings → Developer → Copy Logs]

**Ekran Görüntüsü:**
[Varsa ekle]
```

---

## ✅ Hızlı Sorun Giderme Checklist

Herhangi bir sorun için:

```markdown
[ ] Antigravity'yi restart ettim mi?
[ ] En son versiyonu mu kullanıyorum?
[ ] Logs'u kontrol ettim mi?
[ ] Git clean state'te miyim?
[ ] Internet bağlantım var mı?
[ ] Rate limit doldu mu?
[ ] Doğru mode seçili mi? (Planning vs Fast)
[ ] Environment variables doğru mu?
[ ] Browser extension yüklü mü?
```

---

## 📚 İlgili Dokümanlar

- [Resmi Antigravity Dokümantasyonu](./ANTIGRAVITY.md)
- [Best Practices](./BEST_PRACTICES.md)
- [Öğrenme Notları](./NOTES.md)

---

**Son Güncelleme:** 2025-11-23
**Antigravity Versiyonu:** Public Preview (v1.11.3+)
