# Görsel Programlamaya Giriş

## 🎯 Öğrenme Hedefleri
- Görsel programlama kavramını açıklayabilmek  
- Java programlama dilinin görsel programlamadaki yerini kavrayabilmek  
- JavaFX teknolojisine giriş yapmak  
- NetBeans IDE’yi kurmak ve temel özelliklerini tanımak  
- İlk JavaFX projesini oluşturmak ("Merhaba Dünya")  

---

## 1.1 Görsel Programlama Nedir?
Görsel programlama, yazılım geliştirme sürecinde kullanıcı arayüzü tasarımını ve kod yazmayı **görsel öğelerle** destekleyen bir yaklaşımı ifade eder. Geleneksel metin tabanlı programlamaya kıyasla, sürükle-bırak yöntemleri, görsel bileşenler ve etkileşimli arayüz tasarımları ön plandadır.  

**Avantajları:**  
- Kullanıcı arayüzleri hızlı tasarlanabilir  
- Kodlama hataları azaltılabilir  
- Öğrenme süreci daha kolay hale gelir  

---

## 1.2 Java’nın Görsel Programlamadaki Yeri
Java, platform bağımsızlığı (JVM) ve geniş kütüphane desteği sayesinde görsel programlamada yaygın olarak kullanılan bir dildir.  

- Eski kütüphane: **Swing**  
- Modern kütüphane: **JavaFX**  
  - CSS desteği  
  - Medya entegrasyonu  
  - Scene Graph mimarisi  

---

## 1.3 JavaFX’e Giriş
**JavaFX**, Java tabanlı modern bir kullanıcı arayüzü geliştirme platformudur.  

**Özellikleri:**  
- 2D/3D grafik desteği  
- CSS ile stil uygulama  
- Ses ve video oynatma desteği  
- Scene Builder ile sürükle-bırak tasarım yapabilme  

---

## 1.4 NetBeans IDE Kurulumu
NetBeans, JavaFX uygulamaları geliştirmek için en çok tercih edilen IDE’lerden biridir.  

### Kurulum Adımları
1. [NetBeans resmi sitesi](https://netbeans.apache.org) üzerinden en güncel sürümü indirin.  
2. JDK kurulumunu kontrol edin (`java -version` komutu ile).  
3. NetBeans açıldığında yeni proje oluşturmak için:  
   - **File > New Project** yolunu izleyin.  
   - **Java with Ant > JavaFX > JavaFX Application** seçeneğini seçin.  

### Temel Özellikler
- Proje yapısı yönetimi  
- Kod tamamlama özelliği  
- Entegre hata ayıklayıcı  
- Scene Builder entegrasyonu  

---

## 1.5 İlk JavaFX Projesi: "Merhaba Dünya"

Aşağıdaki kod, JavaFX ile yazılmış basit bir "Merhaba Dünya" uygulamasıdır:  

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.stage.Stage;

public class MerhabaDunya extends Application {
    @Override
    public void start(Stage primaryStage) {
        Label etiket = new Label("Merhaba Dünya!");
        Scene scene = new Scene(etiket, 300, 200);
        primaryStage.setTitle("İlk JavaFX Uygulaması");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

### Çalıştırma Adımları

1. NetBeans üzerinde yeni proje oluşturun.
2. MerhabaDunya.java dosyasına yukarıdaki kodu yazın.
3. Projeyi çalıştırın (▶ Run).
4. Ekranda "Merhaba Dünya!" yazılı küçük bir pencere açılacaktır. 🎉

## 📌 Önemli Notlar

- JavaFX projeleri için JDK 8 veya üstü kullanılmalıdır.
- Eğer NetBeans’te JavaFX seçenekleri görünmüyorsa, JavaFX SDK ayrıca kurulmalıdır.
- Scene Builder kullanarak görsel arayüz tasarımını ilerleyen bölümlerde uygulayacağız.

## ✅ Kazanımlar

Bu bölümü tamamladığınızda:

- Görsel programlamanın ne olduğunu açıklayabilir
- Java ve JavaFX’in temel rolünü anlayabilir
- NetBeans kurulumunu yapabilir
- İlk JavaFX projesini çalıştırabilirsiniz