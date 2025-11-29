# Yapay Zekâ ile Geliştirme

## Amaç
Modern yazılım geliştirmeyi hızlandırmak ve optimize etmek için üretken yapay zekâ (ÜYZ/GenAI) araçlarının entegrasyonu: kod üretimi, hata bulma, performans optimizasyonu ve etik sorumluluk.

## Öğrenme Çıktıları
Öğrenci aşağıdakileri yapabilmeli:
- **AI Araçlarını Seçme & Entegre Etme:** ChatGPT, GitHub Copilot, Codeium, Claude, Google Gemini gibi araçları kendi workflow'una entegre etmek
- **Prompt Engineering:** Yapay zekâya etkili komutlar yazarak kaliteli kod ve açıklamalar elde etmek
- **AI Destekli Hata Bulma & Debugging:** AI'ı bir hata ayıklama partner olarak kullanmak
- **Performans Optimizasyonu:** AI tarafından önerilen iyileştirmeleri değerlendirmek ve uygulamak
- **Etik AI Kullanımı:** AI çıktılarının doğruluğu, telif hakkı ve sorumluluğunu anlamak

## Konular

### 1. AI Araçları Karşılaştırması

#### GitHub Copilot
- **Özellikleri:** IDE entegrasyonu (VSCode, IntelliJ, Visual Studio), inline suggestions, chat interface
- **Güçlü Yanları:** Hızlı, context-aware, JavaFX kodu da dahil
- **Sınırlamaları:** Abonelik gerekli, internet bağımlı
- **JavaFX Örneği:**
```java
// Copilot tarafından önerilen kod
Button btn = new Button("Tıkla");
btn.setStyle("-fx-font-size: 14px; -fx-padding: 10px;");
btn.setOnAction(e -> System.out.println("Tıklandı"));
```

#### ChatGPT (OpenAI)
- **Özellikleri:** Web-based veya API, kod açıklama, tam dersler ve rehberlik
- **Güçlü Yanları:** Uzun açıklamalar, kavram öğretimi, çoklu diller
- **Sınırlamaları:** Bellek limiti, real-time kodlanmamış bilgi

#### Codeium
- **Özellikleri:** Ücretsiz alternatif, IDE plugins, hızlı suggestions
- **Güçlü Yanları:** Bedava, offline desteği, gizlilik odaklı
- **Sınırlamaları:** Copilot kadar kapsamlı değil

#### Claude (Anthropic)
- **Özellikleri:** Güçlü mantık yürütme, detaylı açıklamalar, dosya analizi (web/API)
- **Güçlü Yanları:** Etik-driven, kod inceleme için üstün
- **Sınırlamaları:** API bağımlı, IDE native plugins sınırlı

#### Google Gemini
- **Özellikleri:** Google Workspace entegrasyonu, multimodal (metin+görüntü), yeni teknolojiler hakkında güncel
- **Güçlü Yanları:** Güncel veriler, multimodal özellikler
- **Sınırlamaları:** JavaFX spesifik bilgisi henüz gelişiyor

---

### 2. Prompt Engineering

#### İyi Prompt Yazma İlkeleri

**Bağlam Sağlayın:**
```
KÖTÜ: "Bir kullanıcı girişi formu yap"
İYİ: "JavaFX'te bir kullanıcı girişi formu yap. VBox layout, 2 TextField (isim, email), 
      1 PasswordField (şifre), 1 Button (Gir). Validation: email formatı ve şifre 8+ karakter. 
      Hata mesajlarını Label'de göster. CSS ile şık tasarım."
```

**Spesifik Çıktı İsteyin:**
```
"Bana şu formu için unit test kodunu TestFX kullanarak yaz:
 - Test 1: Geçersiz email ile başarısız submit
 - Test 2: Başarılı login senaryosu
 - Test 3: Şifre validation kuralları"
```

**Iteratif Refinement:**
```
1. İlk request: "Veritabanı bağlantısı yap"
2. AI response: Generic JDBC kodu
3. Follow-up: "Bunu SQLite'e özel hale getir, connection pooling ekle, prepared statements kullan"
4. AI response: Optimized code
```

---

### 3. Pratik AI Destekli Geliştirme İş Akışları

#### İş Akışı A: Hızlı Kod Üretimi
```
1. AI'a istediğinizi açık şekilde anlatın
   "JavaFX'te bir TableView yap; MySQL'den çektiğim User nesnelerini göster. 
    Sütunlar: ID, Ad, Email, Kayıt Tarihi. Sorting, filtering ekle."

2. AI kod üretir

3. Siz bunu IDE'ye kopyalar, test edersiniz

4. AI'a sorular sorun: "Bu code'da thread-safety sorunu var mı?" veya 
   "Bu performans için büyük veri setleri (100k rows) için optimize edilebilir mi?"
```

#### İş Akışı B: Hata Bulma & Hata Ayıklama
```java
// Sorunlu kod:
@FXML
private TableView<Product> productTable;

public void initialize() {
    // AI'a verdiğiniz kod ve hata mesajı:
    // "Bu kod çalışmıyor: productTable null pointer exception veriyor"
    
    productTable.setItems(loadProducts()); // NullPointerException
}

// AI'ın tanısı:
// "FXML loader'ı @FXML injection yapmıyor. Çözüm: 
//  1. Scene Builder'de productTable'ı kurmayı kontrol et
//  2. FXMLLoader.setController(this) ve load() öncesi
//  3. Veya programatik: productTable = new TableView<>();"
```

#### İş Akışı C: Kod İyileştirme & Optimizasyon
```
AI'dan: "Bu kod'u şu açılardan refactor et:
- DRY (Don't Repeat Yourself) prensibine uygun hale getir
- Lambda expression kullan
- Null checks ekle
- Performans iyileştirmeleri yap (ne varsa)"

Eski (verbose):
List<String> names = new ArrayList<>();
for (User u : users) {
    names.add(u.getName());
}

Yeni (AI önerisi):
List<String> names = users.stream()
    .map(User::getName)
    .collect(Collectors.toList());
```

---

### 4. Etik AI Kullanımı & Sorumluluk

#### Dikkat Edilecek Noktalar

**Telif Hakkı (Copyright):**
- AI'ın öğrendiği kodlar genellikle açık kaynaklardan
- Ticari projede kullanırken: AI çıktısını "işlenmiş türev eser" olarak değerlendirin
- Lisan: GitHub Copilot, ChatGPT vs. sözleşmelerine uymalısınız

**Doğruluk (Accuracy):**
```
AI bazen yanlış kod önerir:
"AI bu JavaFX kodu verdi ama çalışmıyor. Neden?"
→ Sebep: AI ImageView'ün constructor signature'ını yanlış hatırladı

Çözüm: Her zaman AI çıktısını test edin ve gerekirse sorgulamaya devam edin
```

**Güvenlik (Security):**
```
TEHLIKE: Gizli API keys ve passwords AI'ya göndermek
ÇÖZÜM: AI'a asla credentials vermeyin; anonymize edin:
  - "Bu URL'nin yetkilendirmesi nasıl yapılır?" 
  - DEĞİL: "My API key is sk-xyz123... how do I use it?"
```

**Bias & Tutarlılık:**
- AI, eğitim verisindeki biases'i yansıtabilir
- Farklı AI araçlardan çıktılar karşılaştırın ve logic check yapın

---

### 5. AI Destekli Geliştirme İçin En İyi Uygulamalar

| Yapın | Yapmayın |
|------|---------|
| AI'ı brainstorm partner olarak kullanın | AI'nın çıktısını doğrulamadan production'a koymayın |
| Anladığınız kodu kullanın | Copy-paste'i sorgulamadan yapın |
| Farklı AI araçlardan opinions alın | Tek bir AI'ya güvenin |
| AI tarafından refactor edilen kodu test edin | Refactoring'in doğruluğunu varsayın |
| API documentation'u referans olarak tutun | Sadece AI'ı referans olarak alın |
| AI'ın açıklamalarını eleştiren gözle okuyun | AI'nın açıklamasını kesin kabul edin |

---

### 6. JavaFX'e Özgü AI Görevleri

#### Örnek 1: FXML ile AI Yardımı
```
AI Prompt:
"JavaFX FXML dosyasından, programatik olarak Button'ların setText() 
sonucu UI'da update olmuyor. Initialize() method'da çağırıyorum ama görünmüyor. 
Çözümü explain et ve kod örneği ver."

AI Yanıtı:
"FXML injectionu ve Platform.runLater() sırası önemli. 
Thread sequencing'i kontrol edin. Önerilen pattern:
@FXML
private Label titleLabel;

@FXML
public void initialize() {
    Platform.runLater(() -> {
        titleLabel.setText("Updated");
    });
}
"
```

#### Örnek 2: CSS Stil Optimizasyonu
```
AI Prompt:
"Bu JavaFX uygulamasının CSS dosyası 500 satır. 
Redundancy'leri ortadan kaldır ve CSS variables kullan. 
Best practices nelerdir?"

AI Yanıtı: SCSS benzeri nesting, color schemes, font hierarchies...
```

#### Örnek 3: Performans Hata Ayıklaması
```
AI: "Bu TableView 10000+ row'da laggy. 
Sorunu identify et ve solution öner (virtualization, lazy loading, etc.)"
```

---

### 7. Uygulamalı Alıştırmalar

**Alıştırma 1: AI ile Prototip Geliştirme**
- Gereksinim: "Kullanıcı yönetimi (CRUD) uygulaması"
- Adım:
  1. ChatGPT/Claude'a UI tasarım fikirleri isteyin
  2. Copilot'tan scaffold kod üretin
  3. Bunu test edin ve hatalar bulun
  4. AI'ya debug yardımı isteyin
  5. Final çıktıyı optimize edin

**Alıştırma 2: AI ile Kod İncelemesi**
- Kendi veya arkadaş kodunuzu Claude'a verip code review isteyin
- Önerilen iyileştirmeleri uygulayın
- AI vs. peer review'i karşılaştırın

**Alıştırma 3: İstem Mühendisliği Uygulaması**
- Tek prompt yazıp kodunu anlaşılmaz bulun
- Prompt'u iteratively refine edin
- 3-5 iterasyon sonra "perfect" prompt oluşturun

**Alıştırma 4: Etik Değerlendirme**
- AI tarafından refactor edilen kodu inceleyin
- Telif hakkı sorunları var mı? Denetlenebilir mi?
- Test coverage'ı AI'dan daha iyisini yapabilir misiniz?

---

### 8. Araçlar & Entegrasyon

| Tool | Integration | Best For |
|------|-------------|----------|
| Copilot | VSCode, IntelliJ, Visual Studio | In-IDE suggestions |
| ChatGPT | Web/API | Detaylı explanations |
| Claude | Web/API | Code review, debugging |
| Codeium | VSCode, JetBrains | Free alternative |
| Gemini | Web/API | Multimodal, current events |

---

## Kapanış Notları

- **AI araçları sentez edici:** Birden fazla AI'dan görüş alın, sonuçları combine edin
- **AI'ınızı Hata Ayıklayın:** AI çıktısını sorgulamak normal—bu işin bir parçası
- **Etik Sorumluluk:** Telif hakkı, güvenlik ve doğruluğa dikkat edin
- **İnsan Döngüsünde:** AI'nın önerileri değerlendiren ve final karar veren insan olun
- **Sürekli Öğrenme:** AI teknolojileri hızla gelişiyor; en güncel araçları takip edin

---

**Son Unit Kapanışı:**
Bu unit'in amacı, yapay zekâyı bir *"coding partner"* olarak nasıl kullanacağınızı öğretmektir—ne code'u tamamen AI'ya devretmek, ne de görmezden gelmek. Doğru dengeyi bulun ve yazılım geliştirme verimliliğinizi artırın.

<div align="center">

# 🌟 THE END 🌟

</div>