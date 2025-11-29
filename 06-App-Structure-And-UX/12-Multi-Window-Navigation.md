# Çoklu Pencere ve Navigasyon

## Özet

Bu bölümde, JavaFX uygulamalarında çoklu pencere (multi-window) yönetimi ve etkili navigasyon sistemleri nasıl oluşturulacağını öğreneceksiniz. Temel pencere oluşturmadan başlayarak, modal pencereleri, pencereler arası veri paylaşımını ve navigasyon menülerini inceleyeceğiz.

---

## 12.1 Çoklu Pencere Kavramı ve Önemi

### Nedir Çoklu Pencere?

Çoklu pencere yönetimi, bir uygulamanın aynı anda birden fazla bağımsız veya ilişkili pencere açabilmesini ve bu pencereler arasında veri paylaşımını mümkün kılar.

**Faydaları:**
- Kullanıcıların aynı anda birden fazla işlemi takip etme imkânı
- Modüler geliştirme ve bakım kolaylığı
- Daha verimli ve özelleştirilmiş kullanıcı deneyimi

**Örnek Senaryolar:**
- Metin editörü: ana pencerede kod yazarken, ayrı pencerede dosya seçimi
- Muhasebe yazılımı: müşteri bilgilerini ana pencerede göstermek, ödeme işlemini ayrı pencerede başlatmak

---

## 12.2 Temel Pencere Yönetimi

### Pencere Oluşturma

JavaFX'te bir pencere oluşturmak için `Stage` (pencere) ve `Scene` (içerik) kullanılır:

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class WindowManagementApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        // Kök panel
        StackPane root = new StackPane();
        Button btn = new Button("Başlat");
        root.getChildren().add(btn);
        
        // Sahne oluştur
        Scene scene = new Scene(root, 400, 300);
        
        // Pencereyi yapılandır
        primaryStage.setTitle("Ana Pencere");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

### Pencere Özellikleri

```java
// Başlık ayarlama
primaryStage.setTitle("Uygulamam");

// Boyut ayarlama
primaryStage.setWidth(800);
primaryStage.setHeight(600);

// İkon ayarlama
primaryStage.getIcons().add(new Image("file:icon.png"));

// Tam ekran modu
primaryStage.setFullScreen(true);

// Resizable (yeniden boyutlandırılabilir)
primaryStage.setResizable(false);

// Başlangıç konumu
primaryStage.setX(100);
primaryStage.setY(100);

// Arka plan rengi
root.setStyle("-fx-background-color: #f0f0f0;");
```

---

## 12.3 Modal Pencereler

Modal pencereleri kullanıcı ile etkileşim yaparken, arka plandaki pencerenin etkileşimini engeller:

```java
import javafx.scene.control.Button;
import javafx.scene.layout.VBox;
import javafx.stage.Modality;
import javafx.stage.Stage;

public class ModalWindowApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        VBox root = new VBox(10);
        Button openModalBtn = new Button("Modal Pencere Aç");
        
        openModalBtn.setOnAction(e -> openModalWindow(primaryStage));
        
        root.getChildren().add(openModalBtn);
        
        Scene scene = new Scene(root, 400, 300);
        primaryStage.setScene(scene);
        primaryStage.setTitle("Ana Pencere");
        primaryStage.show();
    }
    
    private void openModalWindow(Stage owner) {
        Stage modalStage = new Stage();
        modalStage.initModality(Modality.APPLICATION_MODAL);
        modalStage.initOwner(owner);
        
        VBox content = new VBox(10);
        Button closeBtn = new Button("Kapat");
        closeBtn.setOnAction(e -> modalStage.close());
        
        content.getChildren().addAll(
            new Label("Bu bir modal penceredir"),
            closeBtn
        );
        
        Scene scene = new Scene(content, 300, 150);
        modalStage.setScene(scene);
        modalStage.setTitle("Modal Pencere");
        modalStage.showAndWait(); // Ana pencerenin engellenmesini sağlar
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 12.4 Pop-up Pencereleri

Pop-up pencereleri ana pencerenin etkileşimini engellemeyen geçici bildirimlerdir:

```java
import javafx.geometry.Bounds;
import javafx.scene.control.PopupControl;
import javafx.scene.control.Label;

private void showPopup(Button button) {
    PopupControl popup = new PopupControl();
    Label label = new Label("Bu bir popup mesajıdır!");
    popup.getScene().setRoot(label);
    
    Bounds bounds = button.localToScreen(button.getBoundsInLocal());
    popup.show(button.getScene().getWindow(), bounds.getCenterX(), bounds.getCenterY() + 30);
    
    // 3 saniye sonra kapat
    PauseTransition pause = new PauseTransition(Duration.seconds(3));
    pause.setOnFinished(e -> popup.hide());
    pause.play();
}
```

---

## 12.5 Dialog Pencereleri

JavaFX'in yerleşik dialog pencereleri:

```java
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;
import javafx.scene.control.ButtonType;
import java.util.Optional;

// Bilgi diyaloğu
Alert infoAlert = new Alert(AlertType.INFORMATION);
infoAlert.setTitle("Bilgi");
infoAlert.setHeaderText("İşlem Başarılı");
infoAlert.setContentText("Veriler kaydedildi.");
infoAlert.showAndWait();

// Onay diyaloğu
Alert confirmAlert = new Alert(AlertType.CONFIRMATION);
confirmAlert.setTitle("Onay");
confirmAlert.setContentText("Devam etmek istiyor musunuz?");
Optional<ButtonType> result = confirmAlert.showAndWait();

if (result.isPresent() && result.get() == ButtonType.OK) {
    System.out.println("Kullanıcı onayladı");
}

// Hata diyaloğu
Alert errorAlert = new Alert(AlertType.ERROR);
errorAlert.setTitle("Hata");
errorAlert.setHeaderText("İşlem Başarısız");
errorAlert.setContentText("Dosya bulunamadı!");
errorAlert.showAndWait();
```

---

## 12.6 Pencereler Arası Veri Paylaşımı

### Yöntem 1: Constructor Üzerinden Veri Aktarma

```java
public class ModalWindow extends Stage {
    private Label dataLabel;
    
    public ModalWindow(String data) {
        VBox root = new VBox();
        dataLabel = new Label("Alınan Veri: " + data);
        root.getChildren().add(dataLabel);
        
        Scene scene = new Scene(root, 300, 150);
        this.setScene(scene);
        this.setTitle("Modal Pencere");
    }
}

// Ana pencereden açma
private void openModalWithData() {
    String dataToShare = "Önemli Bilgi";
    ModalWindow modal = new ModalWindow(dataToShare);
    modal.initModality(Modality.APPLICATION_MODAL);
    modal.showAndWait();
}
```

### Yöntem 2: Statik Değişken (Paylaşılan Veri Deposu)

```java
public class SharedData {
    private static String sharedValue;
    
    public static void setSharedValue(String value) {
        sharedValue = value;
    }
    
    public static String getSharedValue() {
        return sharedValue;
    }
}

// Pencere 1'den veri set etme
SharedData.setSharedValue("Paylaşılacak Veri");

// Pencere 2'de veri okuma
String data = SharedData.getSharedValue();
```

### Yöntem 3: Callback (Geri Çağırma)

```java
public class ModalWithCallback extends Stage {
    public ModalWithCallback(Stage owner, Callback<String, Void> callback) {
        VBox root = new VBox(10);
        TextField textField = new TextField();
        Button okBtn = new Button("Gönder");
        
        okBtn.setOnAction(e -> {
            callback.call(textField.getText());
            this.close();
        });
        
        root.getChildren().addAll(textField, okBtn);
        
        Scene scene = new Scene(root, 300, 150);
        this.setScene(scene);
        this.initModality(Modality.APPLICATION_MODAL);
        this.initOwner(owner);
    }
}

// Kullanım
private void openModalWithCallback() {
    ModalWithCallback modal = new ModalWithCallback(primaryStage, (data) -> {
        System.out.println("Modal'dan gelen veri: " + data);
        return null;
    });
    modal.showAndWait();
}
```

---

## 12.7 Navigasyon Sistemleri

### Yöntem 1: StackPane ile Ekran Değiştirme

```java
public class NavigationApp extends Application {
    private StackPane root;
    
    @Override
    public void start(Stage primaryStage) {
        root = new StackPane();
        
        // Üç farklı ekran oluştur
        VBox screen1 = createScreen1();
        VBox screen2 = createScreen2();
        VBox screen3 = createScreen3();
        
        root.getChildren().add(screen1);
        
        Scene scene = new Scene(root, 500, 400);
        primaryStage.setScene(scene);
        primaryStage.setTitle("Navigasyon Örneği");
        primaryStage.show();
    }
    
    private VBox createScreen1() {
        VBox screen = new VBox(15);
        screen.setPadding(new Insets(20));
        screen.setStyle("-fx-background-color: #e0e0e0;");
        
        Label label = new Label("Ekran 1");
        label.setStyle("-fx-font-size: 24;");
        
        Button btn2 = new Button("Ekran 2'ye Git");
        btn2.setOnAction(e -> navigateTo(createScreen2()));
        
        screen.getChildren().addAll(label, btn2);
        return screen;
    }
    
    private VBox createScreen2() {
        VBox screen = new VBox(15);
        screen.setPadding(new Insets(20));
        screen.setStyle("-fx-background-color: #fff9c4;");
        
        Label label = new Label("Ekran 2");
        label.setStyle("-fx-font-size: 24;");
        
        Button btn1 = new Button("Ekran 1'e Dön");
        btn1.setOnAction(e -> navigateTo(createScreen1()));
        
        Button btn3 = new Button("Ekran 3'e Git");
        btn3.setOnAction(e -> navigateTo(createScreen3()));
        
        screen.getChildren().addAll(label, btn1, btn3);
        return screen;
    }
    
    private VBox createScreen3() {
        VBox screen = new VBox(15);
        screen.setPadding(new Insets(20));
        screen.setStyle("-fx-background-color: #c8e6c9;");
        
        Label label = new Label("Ekran 3");
        label.setStyle("-fx-font-size: 24;");
        
        Button btn2 = new Button("Ekran 2'ye Dön");
        btn2.setOnAction(e -> navigateTo(createScreen2()));
        
        screen.getChildren().addAll(label, btn2);
        return screen;
    }
    
    private void navigateTo(VBox screen) {
        root.getChildren().clear();
        root.getChildren().add(screen);
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

### Yöntem 2: TabPane ile Sekmeler

```java
TabPane tabPane = new TabPane();

Tab tab1 = new Tab("Ev", new Label("Anasayfa içeriği"));
tab1.setClosable(false);

Tab tab2 = new Tab("Hakkında", new Label("Hakkında içeriği"));
tab2.setClosable(false);

Tab tab3 = new Tab("İletişim", new Label("İletişim içeriği"));
tab3.setClosable(false);

tabPane.getTabs().addAll(tab1, tab2, tab3);

Scene scene = new Scene(tabPane, 600, 400);
primaryStage.setScene(scene);
```

### Yöntem 3: MenuBar ile Navigasyon

```java
MenuBar menuBar = new MenuBar();

Menu fileMenu = new Menu("Dosya");
MenuItem newItem = new MenuItem("Yeni");
MenuItem openItem = new MenuItem("Aç");
MenuItem exitItem = new MenuItem("Çık");

newItem.setOnAction(e -> navigateToNewScreen());
openItem.setOnAction(e -> navigateToOpenScreen());
exitItem.setOnAction(e -> primaryStage.close());

fileMenu.getItems().addAll(newItem, openItem, new SeparatorMenuItem(), exitItem);

Menu editMenu = new Menu("Düzen");
// Düzen menüsü öğeleri ekleyin

menuBar.getMenus().addAll(fileMenu, editMenu);
```

---

## 12.8 Geçiş Animasyonları

Ekranlar arasında geçişleri animasyonlu yapmak:

```java
private void navigateWithAnimation(VBox newScreen) {
    FadeTransition fadeOut = new FadeTransition(Duration.millis(300), root);
    fadeOut.setFromValue(1.0);
    fadeOut.setToValue(0.0);
    
    fadeOut.setOnFinished(e -> {
        root.getChildren().clear();
        root.getChildren().add(newScreen);
        
        FadeTransition fadeIn = new FadeTransition(Duration.millis(300), root);
        fadeIn.setFromValue(0.0);
        fadeIn.setToValue(1.0);
        fadeIn.play();
    });
    
    fadeOut.play();
}
```

---

## 12.9 Uygulama Alıştırmaları

1. **Çoklu Pencere Uygulaması:** Ana pencereden açılabilen ve veri paylaşabilen 3-4 modal pencere oluşturun.
2. **Navigasyon Sistemi:** StackPane ve butonlar kullanarak 4-5 ekranlı bir navigasyon sistemi tasarlayın.
3. **TabPane Uygulaması:** Müşteri, ürün ve sipariş sekmelerinden oluşan bir yönetim aracı yapın.
4. **Geçiş Animasyonları:** Ekran geçişlerine SlideTransition veya RotateTransition efektleri ekleyin.

---

## 12.10 Önemli Notlar

- **Modal vs Non-Modal:** Modal pencereleri kritik işlemler için, pop-up'ları geçici bilgiler için kullanın.
- **Veri Paylaşımı:** Callback yöntemi çoğu durumda en güvenli ve temiz yöntemdir.
- **Navigasyon:** Küçük uygulamalarda StackPane, daha büyük uygulamalarda TabPane veya MenuBar tercih edin.
- **Performans:** Çok fazla pencere açıl/kapa işlemi bellek sorunlarına neden olabilir; gerekirse pencereleri yeniden kullanın.
