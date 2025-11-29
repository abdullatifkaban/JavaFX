# Form ve Veri İşleme

## Özet
Bu bölümde JavaFX ile form tasarımı, kullanıcıdan veri alma, veri doğrulama ve küçük bir öğrenci kayıt formu örneği üzerinde çalışacağız. Ayrıca `FileChooser` ile dosya seçimi ve basit veri saklama yaklaşımlarını ele alacağız.

---

## 9.1 Form Tasarımı ve Bileşenleri

JavaFX form bileşenleri:

- `Label` — Etiket
- `TextField` — Tek satır metin girişi
- `TextArea` — Çok satırlı metin
- `PasswordField` — Parola girişi
- `ComboBox` — Açılır liste
- `CheckBox` ve `RadioButton` — Seçimler
- `DatePicker` — Tarih seçme
- `Button` — Eylem tetikleme

### Basit Form (FXML veya kodla)

Aşağıda kodla basit bir form örneği bulunmaktadır:

```java
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class SimpleFormApp extends Application {
    @Override
    public void start(Stage stage) {
        GridPane grid = new GridPane();
        grid.setPadding(new Insets(10));
        grid.setHgap(10);
        grid.setVgap(10);

        Label nameLabel = new Label("Ad:");
        TextField nameField = new TextField();

        Label emailLabel = new Label("E-posta:");
        TextField emailField = new TextField();

        Label dobLabel = new Label("Doğum:");
        DatePicker dobPicker = new DatePicker();

        Button submit = new Button("Gönder");
        submit.setOnAction(e -> {
            System.out.println("Ad: " + nameField.getText());
            System.out.println("E-posta: " + emailField.getText());
            System.out.println("Doğum: " + dobPicker.getValue());
        });

        grid.addRow(0, nameLabel, nameField);
        grid.addRow(1, emailLabel, emailField);
        grid.addRow(2, dobLabel, dobPicker);
        grid.addRow(3, submit);

        Scene scene = new Scene(grid, 400, 200);
        stage.setScene(scene);
        stage.setTitle("Basit Form");
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 9.2 Veri Alma ve Doğrulama

Kullanıcı verisini alırken temel adımlar:

- Zorunlu alanları kontrol et
- Format doğrulaması (e-posta, sayı, tarih)
- Uzunluk/kapsam kontrolleri
- Geri bildirim (hataları kullanıcıya göster)

### Örnek: E-posta doğrulama

```java
private boolean isValidEmail(String email) {
    if (email == null || email.isEmpty()) return false;
    return email.matches("^[A-Za-z0-9+_.-]+@(.+)$");
}
```

### Kullanıcıya hata göstermek (Alert)

```java
if (!isValidEmail(emailField.getText())) {
    Alert alert = new Alert(Alert.AlertType.ERROR, "Geçerli bir e-posta girin.", ButtonType.OK);
    alert.showAndWait();
    return;
}
```

---

## 9.3 Form Durum Yönetimi ve Modelleme

Form verilerini bir POJO (Plain Old Java Object) ile modelleyin:

```java
public class Student {
    private String name;
    private String email;
    private LocalDate birthDate;
    // getter/setter
}
```

Form gönderildiğinde, UI verilerini model nesnesine aktarın, doğrulayın, sonra saklayın veya gösterin.

---

## 9.4 FileChooser ile Dosya İşlemleri

Basit dosya seçimi örneği:

```java
FileChooser fileChooser = new FileChooser();
fileChooser.setTitle("Resim Seç");
fileChooser.getExtensionFilters().add(new FileChooser.ExtensionFilter("Image Files", "*.png", "*.jpg"));
File file = fileChooser.showOpenDialog(stage);
if (file != null) {
    Image img = new Image(file.toURI().toString());
    imageView.setImage(img);
}
```

Dosyaya yazma/okuma için `Files` API'sı veya `BufferedWriter/Reader` kullanabilirsiniz. Küçük veri setleri için JSON veya CSV formatı tercih edilebilir.

---

## 9.5 Küçük Uygulama: Öğrenci Kayıt Formu

- Alanlar: Ad, Soyad, E-posta, Doğum Tarihi, Cinsiyet (RadioButton), Dersler (CheckBox list)
- Doğrulama: Ad ve E-posta zorunlu; e-posta formatı
- Kaydetme: Geçici listeye ekle ve TableView'de göster

### Örnek Kaydetme Mantığı

```java
// submit handler
Student s = new Student();
s.setName(nameField.getText());
s.setEmail(emailField.getText());
s.setBirthDate(dobPicker.getValue());

if (!isValidEmail(s.getEmail())) {
    showError("Geçerli bir e-posta girin");
    return;
}

studentList.add(s); // ObservableList backing TableView
```

---

## 9.6 İleri Konular ve İpuçları

- Form durumunu `Bindings` ve `Properties` ile bağlayın (MVVM yaklaşımı)
- Büyük formlar için `TabPane` veya wizard (çok adımlı form) tasarlayın
- Giriş verilerini locale-aware olarak parse edin (tarih, sayı formatları)
- Birim testleri için doğrulama metodlarını izole edin

---

## Alıştırmalar

- Formda zorunlu alan vurgulama (kırmızı kenarlık) ekleyin
- TableView ile kayıtları düzenleme/silme işlevi ekleyin
- Form verisini JSON'a dönüştürüp dosyaya kaydedin ve tekrar yükleyin

---

## Kaynaklar

- JavaFX Controls API
- Java NIO Files API
- Gson / Jackson (JSON için)
