# Arayüz Tasarımı ve Stil

## Özet

Bu bölümde, JavaFX uygulamalarında profesyonel arayüzler tasarlamayı, CSS ile stil uygulamayı, FXML ile bildirimsel tasarım yapmayı ve responsive düzenler oluşturmayı öğreneceksiniz.

---

## 13.1 Arayüz Tasarım İlkeleri

İyi bir arayüzün temelini dört ana ilke oluşturur:

### Görsel Hiyerarşi (Visual Hierarchy)

Öğelerin önem sırasına göre düzenlenmesi. Başlıklar belirgin, alt başlıklar sade, birincil butonlar farklı renk/boyutla vurgulanır.

```
Başlık (24px, Bold)
├─ Alt Başlık (16px, Semi-bold)
├─ İçerik Metni (12px, Regular)
└─ Birincil Buton (18px, Bold, Dikkat Çekici Renk)
```

### Tutarlılık (Consistency)

Aynı işlevler için aynı simgeler, renkler ve etkileşim dilinin kullanılması.

```
❌ Yanlış:
- "Kaydet" → Floppy disk simgesi
- "Kapat" → X simgesi
- Yine de "Kaydet" → Diskette simgesi (değişken!)

✅ Doğru:
- Tüm "Kaydet" işlemleri → Aynı disk simgesi
- Tüm "Kapat" işlemleri → Aynı X simgesi
```

### Kontrast (Contrast)

Öne çıkarılması gereken öğelerin vurgulanması. Renk, boyut veya tipografi aracılığıyla sağlanır.

```java
// Birincil buton (dikkat çekici)
.primary-button {
    -fx-background-color: #2f80ed;
    -fx-text-fill: white;
    -fx-font-size: 14px;
}

// İkincil buton (daha sade)
.secondary-button {
    -fx-background-color: #e0e0e0;
    -fx-text-fill: #333333;
    -fx-font-size: 12px;
}
```

### Boşluk (Whitespace)

Öğeler arasındaki uygun boşluk, ekranın sıkışık görünmesini engeller ve okunabilirliği artırır.

---

## 13.2 Yerleşim (Layout) Stratejileri

### BorderPane - Klasik Uygulama İskeleti

```java
BorderPane root = new BorderPane();

// Üst: Menü çubuğu
MenuBar menuBar = new MenuBar();
root.setTop(menuBar);

// Sol: Navigasyon paneli
VBox sidebar = new VBox(10);
sidebar.setPrefWidth(150);
root.setLeft(sidebar);

// Merkez: Ana içerik
AnchorPane content = new AnchorPane();
root.setCenter(content);

// Sağ: İstatistikler veya araçlar
VBox rightPanel = new VBox();
root.setRight(rightPanel);

// Alt: Durum çubuğu
HBox statusBar = new HBox();
root.setBottom(statusBar);
```

### VBox ve HBox - Dikey ve Yatay Düzen

```java
// Dikey düzen
VBox vbox = new VBox(10);  // 10px boşluk
vbox.getChildren().addAll(
    new Label("Ad:"),
    new TextField(),
    new Label("E-Posta:"),
    new TextField(),
    new Button("Gönder")
);

// Yatay düzen
HBox hbox = new HBox(10);  // 10px boşluk
hbox.getChildren().addAll(
    new Button("Evet"),
    new Button("Hayır"),
    new Button("İptal")
);
```

### GridPane - Tablo Benzeri Yapı

```java
GridPane grid = new GridPane();
grid.setHgap(10);
grid.setVgap(10);

// Satır 0
grid.add(new Label("Ad:"), 0, 0);
grid.add(new TextField(), 1, 0);

// Satır 1
grid.add(new Label("E-Posta:"), 0, 1);
grid.add(new TextField(), 1, 1);

// Satır 2
grid.add(new Label("Telefon:"), 0, 2);
grid.add(new TextField(), 1, 2);

// Butonlar
HBox buttons = new HBox(10);
buttons.getChildren().addAll(new Button("Kaydet"), new Button("İptal"));
grid.add(buttons, 0, 3, 2, 1); // 2 sütun, 1 satır
```

### FlowPane ve TilePane - Esnek Düzenler

```java
// FlowPane: Satırı taştığında otomatik satır atlar
FlowPane flowPane = new FlowPane(10, 10);
for (int i = 0; i < 10; i++) {
    flowPane.getChildren().add(new Button("Buton " + i));
}

// TilePane: Eşit boyutlu hücreler
TilePane tilePane = new TilePane(10, 10);
tilePane.setPrefColumns(3);  // 3 sütun
for (int i = 0; i < 9; i++) {
    tilePane.getChildren().add(new Button("Buton " + i));
}
```

---

## 13.3 Bileşenler (Controls) ve Durum

JavaFX'in temel bileşenleri ve durumlarının yönetimi:

```java
// Buton - Tıklanabilir eylem
Button btn = new Button("Kaydet");
btn.setStyle("-fx-font-size: 14px;");
btn.setOnAction(e -> System.out.println("Tıklandı!"));

// Etiket - Bilgi gösterimi
Label label = new Label("Hoş geldiniz!");

// Metin Alanı
TextField textField = new TextField();
textField.setPromptText("İsminizi girin...");

// Metin Alanı (Çok satır)
TextArea textArea = new TextArea();
textArea.setWrapText(true);

// ComboBox - Seçim yapma
ComboBox<String> comboBox = new ComboBox<>();
comboBox.getItems().addAll("Seçenek 1", "Seçenek 2", "Seçenek 3");

// CheckBox - Çoklu seçim
CheckBox checkbox = new CheckBox("Beni hatırla");

// RadioButton - Tek seçim
RadioButton radio1 = new RadioButton("Evet");
RadioButton radio2 = new RadioButton("Hayır");
ToggleGroup group = new ToggleGroup();
radio1.setToggleGroup(group);
radio2.setToggleGroup(group);

// ListView - Liste gösterimi
ListView<String> listView = new ListView<>();
listView.getItems().addAll("Öğe 1", "Öğe 2", "Öğe 3");

// TableView - Tablo gösterimi
TableView<String> tableView = new TableView<>();
TableColumn<String, String> col = new TableColumn<>("Başlık");
tableView.getColumns().add(col);
```

### Durum Yönetimi - CSS ile

```css
/* Normal durum */
.button {
    -fx-background-color: #2f80ed;
    -fx-text-fill: white;
}

/* Üzerine gelindikçe */
.button:hover {
    -fx-background-color: #1e5fc9;
}

/* Seçili */
.button:pressed {
    -fx-background-color: #0d47a1;
}

/* Devre dışı */
.button:disabled {
    -fx-background-color: #cccccc;
    -fx-text-fill: #666666;
    -fx-opacity: 0.5;
}

/* Odaklanmış */
.button:focused {
    -fx-border-color: #ffeb3b;
    -fx-border-width: 2;
}
```

---

## 13.4 FXML ile Görsel Tasarım

FXML (JavaFX Markup Language), arayüzü XML olarak tanımlamanıza olanak verir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>

<BorderPane xmlns="http://javafx.com/javafx"
            xmlns:fx="http://javafx.com/fxml"
            fx:controller="com.example.MainController">
    
    <!-- Üst: Araç çubuğu -->
    <top>
        <ToolBar>
            <Button text="Yeni" onAction="#onNew"/>
            <Button text="Aç" onAction="#onOpen"/>
            <Separator/>
            <Button text="Kaydet" onAction="#onSave"/>
        </ToolBar>
    </top>
    
    <!-- Merkez: Ana içerik -->
    <center>
        <VBox spacing="10" style="-fx-padding: 10;">
            <Label text="Hoşgeldiniz!" style="-fx-font-size: 18; -fx-font-weight: bold;"/>
            <TextField fx:id="nameField" promptText="Adınızı girin"/>
            <TextArea fx:id="contentArea" wrapText="true" prefRowCount="10"/>
            <HBox spacing="10">
                <Button text="Gönder" onAction="#onSubmit" style="-fx-padding: 8;"/>
                <Button text="Temizle" onAction="#onClear"/>
            </HBox>
        </VBox>
    </center>
    
    <!-- Alt: Durum çubuğu -->
    <bottom>
        <HBox style="-fx-background-color: #f0f0f0; -fx-padding: 5;">
            <Label text="Hazır" fx:id="statusLabel"/>
        </HBox>
    </bottom>
</BorderPane>
```

### Controller Sınıfı

```java
import javafx.fxml.FXML;
import javafx.scene.control.*;

public class MainController {
    @FXML private TextField nameField;
    @FXML private TextArea contentArea;
    @FXML private Label statusLabel;
    
    @FXML
    private void onNew() {
        nameField.clear();
        contentArea.clear();
        statusLabel.setText("Yeni belge oluşturuldu");
    }
    
    @FXML
    private void onOpen() {
        statusLabel.setText("Dosya açılıyor...");
    }
    
    @FXML
    private void onSave() {
        statusLabel.setText("Dosya kaydedildi: " + nameField.getText());
    }
    
    @FXML
    private void onSubmit() {
        String content = contentArea.getText();
        System.out.println("Gönderilen içerik: " + content);
        statusLabel.setText("İçerik gönderildi!");
    }
    
    @FXML
    private void onClear() {
        contentArea.clear();
        statusLabel.setText("İçerik temizlendi");
    }
}
```

### FXML Yükleme

```java
@Override
public void start(Stage primaryStage) throws IOException {
    FXMLLoader loader = new FXMLLoader(getClass().getResource("main.fxml"));
    BorderPane root = loader.load();
    
    Scene scene = new Scene(root, 800, 600);
    primaryStage.setScene(scene);
    primaryStage.setTitle("FXML Uygulaması");
    primaryStage.show();
}
```

---

## 13.5 CSS ile Stil Mimarisi

### CSS Temel Seçiciler

```css
/* Tüm düğümler */
.root {
    -fx-font-family: "Arial";
    -fx-font-size: 12px;
}

/* Tüm butonlar */
.button {
    -fx-padding: 8px 16px;
    -fx-font-size: 14px;
}

/* Sınıf seçicisi */
.primary-button {
    -fx-background-color: #2f80ed;
    -fx-text-fill: white;
}

.secondary-button {
    -fx-background-color: #e0e0e0;
    -fx-text-fill: #333333;
}

/* ID seçicisi */
#saveButton {
    -fx-padding: 10px 20px;
}

/* Başına göre seçme */
Button.large-button {
    -fx-font-size: 18px;
    -fx-padding: 12px;
}

/* Pseudo-class */
.button:hover {
    -fx-background-color: #1e5fc9;
}

.button:pressed {
    -fx-background-color: #0d47a1;
}

.button:disabled {
    -fx-opacity: 0.5;
}

/* Hiyerarşik seçici */
.vbox .label {
    -fx-text-fill: #333333;
}
```

### Tema Dosyaları

**light.css** - Açık Tema:
```css
.root {
    -fx-background-color: #ffffff;
    -fx-text-fill: #333333;
}

.button {
    -fx-background-color: #2f80ed;
    -fx-text-fill: white;
    -fx-border-radius: 4;
}

.button:hover {
    -fx-background-color: #1e5fc9;
}

.text-field, .text-area {
    -fx-control-inner-background: #f5f5f5;
    -fx-border-color: #cccccc;
}

.menu-bar {
    -fx-background-color: #f0f0f0;
}

.table-view {
    -fx-background-color: #ffffff;
}

.table-view:focused {
    -fx-focus-color: transparent;
}
```

**dark.css** - Koyu Tema:
```css
.root {
    -fx-background-color: #1e1e1e;
    -fx-text-fill: #ffffff;
}

.button {
    -fx-background-color: #ff9800;
    -fx-text-fill: #ffffff;
    -fx-border-radius: 4;
}

.button:hover {
    -fx-background-color: #fb8c00;
}

.text-field, .text-area {
    -fx-control-inner-background: #2d2d2d;
    -fx-text-fill: #ffffff;
    -fx-border-color: #454545;
}

.menu-bar {
    -fx-background-color: #2d2d2d;
}

.table-view {
    -fx-background-color: #2d2d2d;
}

.table-view .table-cell {
    -fx-text-fill: #ffffff;
}
```

### Tema Değiştirme

```java
public class ThemeController {
    private Scene scene;
    
    public void switchToLight() {
        scene.getStylesheets().clear();
        scene.getStylesheets().add(getClass().getResource("light.css").toExternalForm());
    }
    
    public void switchToDark() {
        scene.getStylesheets().clear();
        scene.getStylesheets().add(getClass().getResource("dark.css").toExternalForm());
    }
}
```

---

## 13.6 Responsive (Duruma Duyarlı) Tasarım

Farklı ekran boyutlarına uyum sağlayan arayüzler:

```java
import javafx.geometry.Insets;
import javafx.scene.layout.*;

public class ResponsiveApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        BorderPane root = new BorderPane();
        
        // Dinamik padding
        root.setPadding(new Insets(10));
        
        // Yan menü (pencere küçüldüğünde gizlenir)
        VBox sidebar = createSidebar();
        
        // Ana içerik (dinamik boyutlandırılır)
        AnchorPane content = createContent();
        AnchorPane.setTopAnchor(content, 0.0);
        AnchorPane.setBottomAnchor(content, 0.0);
        AnchorPane.setLeftAnchor(content, 0.0);
        AnchorPane.setRightAnchor(content, 0.0);
        
        HBox mainArea = new HBox();
        HBox.setHgrow(content, Priority.ALWAYS);
        mainArea.getChildren().addAll(sidebar, content);
        
        root.setCenter(mainArea);
        
        // Pencere boyutu değiştiğinde yan menüyü gizle/göster
        primaryStage.widthProperty().addListener((obs, oldVal, newVal) -> {
            if (newVal.doubleValue() < 600) {
                sidebar.setVisible(false);
            } else {
                sidebar.setVisible(true);
            }
        });
        
        Scene scene = new Scene(root, 900, 600);
        primaryStage.setScene(scene);
        primaryStage.setTitle("Responsive Uygulaması");
        primaryStage.show();
    }
    
    private VBox createSidebar() {
        VBox sidebar = new VBox(10);
        sidebar.setPrefWidth(200);
        sidebar.setStyle("-fx-background-color: #f0f0f0;");
        sidebar.setPadding(new Insets(10));
        
        sidebar.getChildren().addAll(
            new Label("Menü"),
            new Button("Anasayfa"),
            new Button("Profil"),
            new Button("Ayarlar"),
            new Button("Çıkış")
        );
        
        return sidebar;
    }
    
    private AnchorPane createContent() {
        AnchorPane content = new AnchorPane();
        VBox vbox = new VBox(15);
        vbox.setPadding(new Insets(20));
        
        vbox.getChildren().addAll(
            new Label("Ana İçerik"),
            new TextField("Metin girin"),
            new TextArea()
        );
        
        content.getChildren().add(vbox);
        return content;
    }
}
```

---

## 13.7 Erişilebilirlik ve Yerelleştirme

### Erişilebilirlik

```java
// Anlamlı buton metinleri
Button saveBtn = new Button("Dosyayı Kaydet");
saveBtn.setAccessibleText("Dosyayı kaydet düğmesi");

// Görsel olmayan alternatifler
ImageView imageView = new ImageView("image.png");
imageView.setAccessibleText("Şirket logosu");

// Başlıkları erişilebilir kılma
Label heading = new Label("Kullanıcı Yönetimi");
heading.setStyle("-fx-font-size: 18px; -fx-font-weight: bold;");
```

### Yerelleştirme (ResourceBundle)

**messages_tr.properties:**
```properties
button.save=Kaydet
button.cancel=İptal
label.name=Adı:
label.email=E-Posta:
```

**messages_en.properties:**
```properties
button.save=Save
button.cancel=Cancel
label.name=Name:
label.email=Email:
```

**Java Kodu:**
```java
ResourceBundle bundle = ResourceBundle.getBundle("messages", Locale.getDefault());

Button saveBtn = new Button(bundle.getString("button.save"));
Label nameLabel = new Label(bundle.getString("label.name"));
```

---

## 13.8 Performans ve En İyi Uygulamalar

1. **Scene Graph Optimizasyonu:** Gereksiz node kullanmaktan kaçının
2. **CSS Seçicileri:** Çok karmaşık seçicilerden kaçının
3. **Binding Kullanımı:** Yalnızca gerekli yerlerde binding yapın
4. **Caching:** Statik grafikler için `setCache(true)` kullanın
5. **Modüler Yapı:** Arayüz, stil ve mantığı ayrı tutun

---

## 13.9 Uygulama Alıştırmaları

1. BorderPane kullanarak 3 sekmeli bir uygulaması tasarlayın
2. FXML + CSS ile light/dark tema geçişi yapan uygulama geliştirin
3. TableView ile veri gösteren responsive tasarım yapın
4. Erişilebilirlik özellikleri ekleyin (accessibleText, screen reader uyumluluğu)
5. ResourceBundle kullanarak en az 2 dillik yerelleştirme yapın

---

## 13.10 Önemli Notlar

- **FXML Avantajları:** Mantık ve sunumu ayırır, Scene Builder ile görsel tasarım yapılabilir
- **CSS Gücü:** Web'deki gibi tema, kontrast ve durum yönetimini sağlar
- **Responsive Tasarım:** VBox, HBox, FlowPane gibi esnek paneller kullanın
- **Erişilebilirlik:** Modern yazılımlarda mutlaka göz önüne alınması gereken konu
