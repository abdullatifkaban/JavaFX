# Temel Arayüz Bileşenleri

## 🎯 Öğrenme Hedefleri
- Label, Button, TextField ve TextArea gibi temel arayüz bileşenlerini tanımak  
- Olay (event) kavramını öğrenmek  
- Kullanıcı etkileşimine dayalı işlemleri gerçekleştirebilmek  
- Basit bir arayüz uygulaması (toplama işlemi yapan hesap makinesi) geliştirebilmek  

---

Kullanıcı arayüzü tasarımı (GUI), yazılımın başarısını doğrudan etkileyen kritik bir unsurdur; çünkü son kullanıcı açısından belirleyici olan, yazılımın ne kadar kolay, anlaşılır ve erişilebilir olduğudur.

JavaFX, modern masaüstü uygulamaları için tasarlanmış güçlü, nesne yönelimli bir platform sunar. Bu bölümde, bir yazılımın temel yapı taşları olan **Label, Button, TextField ve TextArea** bileşenleri incelenecek ve bu bileşenlerin kullanıcı etkileşimlerine nasıl tepki vereceği (Olay Yönetimi - Event Handling) açıklanacaktır.

## 3.1 Label ile Metin Gösterimi

`Label` bileşeni, kullanıcıya **metin göstermek** için kullanılan en temel yapı taşlarından biridir. Basit gibi görünse de, bilgilendirme, yönlendirme ve açıklama görevlerini üstlenir.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.stage.Stage;

public class LabelExample extends Application {
    @Override
    public void start(Stage stage) {
        Label label = new Label("Hoş geldiniz!");
        Scene scene = new Scene(label, 200, 100);
        stage.setTitle("Label Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

**Not:** Label sabit metinler için kullanılır; kullanıcı etkileşimi almaz.

## 3.2 Button ile Etkileşimli İşlemler

`Button`, kullanıcıdan **tıklama** ile etkileşim almak için kullanılır.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class ButtonExample extends Application {
    @Override
    public void start(Stage stage) {
        Label label = new Label("Henüz tıklanmadı.");
        Button button = new Button("Tıkla!");

        button.setOnAction(e -> label.setText("Butona tıklandı!"));

        VBox root = new VBox(10);
        root.getChildren().addAll(label, button);

        Scene scene = new Scene(root, 250, 150);
        stage.setTitle("Button Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 3.3 TextField ve TextArea ile Veri Girişi

Kullanıcıdan **metin girişi** almak için `TextField` (tek satırlı) veya `TextArea` (çok satırlı) bileşenleri kullanılır.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class TextFieldExample extends Application {
    @Override
    public void start(Stage stage) {
        Label result = new Label();
        TextField input = new TextField();
        input.setPromptText("Adınızı giriniz...");
        Button btn = new Button("Gönder");

        btn.setOnAction(e -> result.setText("Merhaba, " + input.getText() + "!"));

        VBox root = new VBox(10);
        root.getChildren().addAll(input, btn, result);

        Scene scene = new Scene(root, 300, 200);
        stage.setTitle("TextField Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 3.4 Olay Yönetimine Giriş (Event Handling)

Olay yönetimi, kullanıcının yaptığı eylemlerin (tıklama, tuş basma, yazı girme) bilgisayar tarafından algılanarak belirli bir işlemle eşleştirilmesi sürecidir. Bu, kullanıcı ile bilgisayar arasındaki etkileşim köprüsünü oluşturur.

### Olay Yönetiminin Temel Bileşenleri

JavaFX’te olay yönetimi üç temel bileşen üzerinden işler:
1. **Olay Kaynağı (Event Source):** Olayın tetiklendiği bileşendir (Örn: Tıklanan `Button` veya imlecin geldiği `TextField`)
.
2. **Olay Nesnesi (Event Object):** Gerçekleşen olay hakkında detaylı bilgileri taşır (Örn: Tıklama koordinatları, basılan tuşun kodu (`KeyCode`)). Tüm olay nesneleri `Event` sınıfından türetilir.
3. **Olay İşleyicisi (Event Handler):** Olay gerçekleştiğinde hangi işlemin yapılacağını tanımlayan yapıdır (Örn: Butona tıklandığında metnin değiştirilmesi).
### Olay İşleyici Yaklaşımları
**Lambda İfadeleri:** Java 8 ile eklenen lambda ifadeleri, olay işleyicilerinin daha kısa, okunabilir ve esnek bir şekilde tanımlanmasını sağlamıştır. Bu, kısa ve tek seferlik işlemler için idealdir.

```java
btn.setOnAction(e -> System.out.println("Tıklandı")); 
// 'e' ActionEvent nesnesini temsil eder.
```
**Anonim İç Sınıf:** Lambda ifadelerinden önce en sık kullanılan yöntemdir. Bu yöntemde, olay işleyicisi doğrudan bileşen üzerinde tanımlanan adsız bir sınıf olarak yazılır.

```java
button.setOnAction(new EventHandler<ActionEvent> {
    @Override
    public void handle(ActionEvent e) {
        System.out.println("Butona tıklandı");
    }
});
```

## 3.5 Uygulama Örneği: Basit Hesap Makinesi

Bu örnekte iki sayının toplanmasını sağlayan basit bir arayüz uygulaması hazırlanmıştır.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class CalculatorApp extends Application {
    @Override
    public void start(Stage stage) {
        Label lbl1 = new Label("1. Sayı:");
        TextField txt1 = new TextField();
        Label lbl2 = new Label("2. Sayı:");
        TextField txt2 = new TextField();
        Label result = new Label("Sonuç: ");
        Button btn = new Button("Topla");

        btn.setOnAction(e -> {
            try {
                double s1 = Double.parseDouble(txt1.getText());
                double s2 = Double.parseDouble(txt2.getText());
                result.setText("Sonuç: " + (s1 + s2));
            } catch (NumberFormatException ex) {
                result.setText("Lütfen geçerli sayılar girin!");
            }
        });

        VBox root = new VBox(10);
        root.getChildren().addAll(lbl1, txt1, lbl2, txt2, btn, result);

        Scene scene = new Scene(root, 300, 250);
        stage.setTitle("Basit Hesap Makinesi");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

### ✅ Kazanımlar

Bu bölümü tamamladığınızda:

- Temel arayüz bileşenlerini tanıyabilir
- Olay yönetimi (event handling) kavramını anlayabilir
- Kullanıcıdan veri alabilir
- Basit etkileşimli arayüzler oluşturabilirsiniz