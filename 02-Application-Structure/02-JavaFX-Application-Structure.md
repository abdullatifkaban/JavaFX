# JavaFX Uygulama Yapısı

## 🎯 Öğrenme Hedefleri
- JavaFX uygulamalarının yaşam döngüsünü açıklayabilmek  
- Stage, Scene ve Node kavramlarını öğrenmek  
- Scene Graph mantığını kavramak  
- Farklı layout yapılarının ne olduğunu tanımak  
- Basit arayüz uygulamaları geliştirebilmek  

---

## 2.1 JavaFX Uygulama Yaşam Döngüsü
JavaFX uygulamaları **Application** sınıfından türetilerek oluşturulur ve üç temel adımı vardır:  

1. **init()** → Uygulama başlatıldığında ilk çalışan metottur (opsiyonel).  
2. **start(Stage primaryStage)** → Uygulamanın ana sahnesi burada tanımlanır.  
3. **stop()** → Uygulama kapatıldığında çalışan metottur.  

```java
import javafx.application.Application;
import javafx.stage.Stage;

public class LifecycleExample extends Application {
    @Override
    public void init() {
        System.out.println("Uygulama başlatılıyor...");
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Yaşam Döngüsü Örneği");
        primaryStage.show();
    }

    @Override
    public void stop() {
        System.out.println("Uygulama kapatılıyor...");
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 2.2 Stage, Scene ve Node Kavramları

- **Stage:** Uygulamanın ana penceresidir.
- **Scene:** Pencere içindeki tüm içerikleri barındırır.
- **Node:** Arayüzde yer alan her bir görsel bileşendir (Label, Button, TextField vb.).

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.stage.Stage;

public class StageSceneExample extends Application {
    @Override
    public void start(Stage primaryStage) {
        Button btn = new Button("Tıkla!");
        Scene scene = new Scene(btn, 300, 200);
        primaryStage.setTitle("Stage ve Scene Örneği");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 2.3 Scene Graph Mantığı

JavaFX, tüm görsel bileşenleri bir **ağaç yapısı (Scene Graph)** şeklinde organize eder.

- Kök (root) düğüm → Genellikle bir layout bileşenidir (VBox, HBox, BorderPane vb.).
- Alt düğümler → Label, Button, TextField gibi bileşenler.

Bu yapı sayesinde:
- Her bileşen tek bir üst elemana sahiptir.
- Tüm arayüz hiyerarşik bir yapıda yönetilir.

---

## 2.4 Layout Kavramına Giriş

Layout bileşenleri, kullanıcı arayüzündeki elemanların düzenlenmesini sağlar.

Sık kullanılan bazı layout türleri:

- HBox → Elemanları yatayda yan yana dizer.
- VBox → Elemanları dikeyde alt alta dizer.
- GridPane → Satır ve sütun tabanlı düzenleme.
- BorderPane → Kuzey, güney, doğu, batı ve merkez bölgelerine yerleşim.

Örnek (VBox kullanımı):

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class VBoxExample extends Application {
    @Override
    public void start(Stage primaryStage) {
        VBox root = new VBox();
        root.getChildren().addAll(
            new Button("Buton 1"),
            new Button("Buton 2"),
            new Button("Buton 3")
        );

        Scene scene = new Scene(root, 200, 150);
        primaryStage.setTitle("VBox Layout Örneği");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 2.5 Basit Arayüz Uygulaması

Aşağıdaki örnekte VBox kullanılarak birden fazla buton ve etiket aynı sahneye eklenmiştir:

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class SimpleUI extends Application {
    @Override
    public void start(Stage primaryStage) {
        Label lbl = new Label("JavaFX Arayüzü");
        Button btn1 = new Button("Kaydet");
        Button btn2 = new Button("İptal");

        VBox root = new VBox(10); // 10 px aralık
        root.getChildren().addAll(lbl, btn1, btn2);

        Scene scene = new Scene(root, 250, 150);
        primaryStage.setTitle("Basit Arayüz Uygulaması");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## ✅ Kazanımlar

Bu bölümü tamamladığınızda:

- JavaFX yaşam döngüsünü açıklayabilir
- Stage, Scene ve Node kavramlarını ayırt edebilir
- Scene Graph mantığını kavrayabilir
- Layout yöneticilerini tanıyabilir
- Basit bir kullanıcı arayüzü geliştirebilirsiniz