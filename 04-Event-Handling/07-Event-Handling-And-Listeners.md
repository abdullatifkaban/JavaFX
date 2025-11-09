# Olay Yönetimi ve Dinleyiciler (Event Handling and Listeners)

## 🎯 Öğrenme Hedefleri
- Olay (event) kavramını anlamak  
- Olayların JavaFX’te nasıl yakalandığını ve işlendiğini öğrenmek  
- `EventHandler` ve `ChangeListener` arayüzlerini uygulamak  
- Lambda ifadeleriyle olayları kısa ve etkili biçimde yönetebilmek  
- Fare, klavye ve buton olaylarını yönetmek  

---

## 7.1 Olay (Event) Nedir?
Kullanıcının uygulama ile etkileşime girdiği her işlem bir **olay (event)** oluşturur.  
Örnekler:  
- Bir **butona tıklamak**  
- Bir **tuşa basmak**  
- Fareyi bir bileşenin üzerine getirmek  
- Metin alanına veri girmek  

JavaFX’te her olay, bir **Event** nesnesiyle temsil edilir ve genellikle bir **dinleyici (listener)** veya **işleyici (handler)** tarafından yakalanır.

---

## 7.2 Buton Olayları (ActionEvent)
En temel olay türü **ActionEvent**’tir.  
Genellikle bir butona tıklama ile tetiklenir.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class ButtonEventExample extends Application {
    @Override
    public void start(Stage stage) {
        Button btn = new Button("Tıklayın");
        btn.setOnAction(event -> System.out.println("Butona tıklandı!"));

        StackPane root = new StackPane(btn);
        Scene scene = new Scene(root, 300, 200);

        stage.setTitle("Button Olayı");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

**📘 Açıklama:**
- setOnAction() metodu bir olay dinleyici atar.
- Lambda ifadesi (event -> ...) doğrudan olayı yakalar.

## 7.3 Fare (Mouse) Olayları

`MouseEvent`, fare tıklamaları, sürükleme ve hareketleri yakalamak için kullanılır.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.input.MouseEvent;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class MouseEventExample extends Application {
    @Override
    public void start(Stage stage) {
        Label label = new Label("Fare ile etkileşime geçin");

        label.setOnMouseEntered(e -> label.setText("Fare üzerine geldi"));
        label.setOnMouseExited(e -> label.setText("Fare çıktı"));
        label.setOnMouseClicked(e -> {
            if (e.getClickCount() == 2)
                label.setText("Çift tıklama algılandı!");
        });

        StackPane root = new StackPane(label);
        Scene scene = new Scene(root, 300, 200);
        stage.setTitle("MouseEvent Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 7.4 Klavye (KeyEvent) Olayları

`KeyEvent`, bir tuşa basıldığında veya bırakıldığında tetiklenir.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.input.KeyEvent;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class KeyEventExample extends Application {
    @Override
    public void start(Stage stage) {
        Label label = new Label("Bir tuşa basın");

        StackPane root = new StackPane(label);
        Scene scene = new Scene(root, 300, 200);

        scene.setOnKeyPressed((KeyEvent e) -> {
            label.setText("Basılan tuş: " + e.getCode());
        });

        stage.setTitle("KeyEvent Örneği");
        stage.setScene(scene);
        stage.show();

        root.requestFocus(); // Klavye odaklanması için
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 7.5 ChangeListener Kullanımı

`ChangeListener`, bir özelliğin değeri değiştiğinde çalışır. Örneğin, bir Slider bileşeninin değeri değiştiğinde.

```java
import javafx.application.Application;
import javafx.beans.value.ChangeListener;
import javafx.beans.value.ObservableValue;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.control.Slider;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class ChangeListenerExample extends Application {
    @Override
    public void start(Stage stage) {
        Slider slider = new Slider(0, 100, 50);
        Label label = new Label("Değer: 50");

        slider.valueProperty().addListener(new ChangeListener<Number>() {
            @Override
            public void changed(ObservableValue<? extends Number> obs, Number oldVal, Number newVal) {
                label.setText("Değer: " + newVal.intValue());
            }
        });

        VBox root = new VBox(10, slider, label);
        Scene scene = new Scene(root, 300, 150);
        stage.setTitle("ChangeListener Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```
> 💡 Kısa sürüm (lambda)

```java
slider.valueProperty().addListener((obs, oldVal, newVal) -> 
    label.setText("Değer: " + newVal.intValue())
);
```

## 7.6 Uygulama: Renk Değiştiren Düğme

Aşağıdaki örnekte, bir düğmeye her tıklandığında arka plan rengi değişir.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class ColorChangeButton extends Application {
    private boolean isBlue = false;

    @Override
    public void start(Stage stage) {
        Button btn = new Button("Rengi Değiştir");

        btn.setOnAction(e -> {
            isBlue = !isBlue;
            String color = isBlue ? "lightblue" : "lightgreen";
            btn.setStyle("-fx-background-color: " + color);
        });

        StackPane root = new StackPane(btn);
        Scene scene = new Scene(root, 300, 200);
        stage.setTitle("Renk Değiştiren Düğme");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## ✅ Kazanımlar

Bu bölümü tamamladığınızda:

- JavaFX’te olayların nasıl işlendiğini açıklayabilir
- Fare, klavye ve buton olaylarını yönetebilir
- ChangeListener kullanarak bileşen değişimlerini izleyebilir
- Lambda ifadeleriyle kısa ve etkili olay yönetimi yapabilirsiniz
