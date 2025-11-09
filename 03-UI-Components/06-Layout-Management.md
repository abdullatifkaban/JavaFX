# Düzen (Layout) Yönetimi

## 🎯 Öğrenme Hedefleri
- Layout kavramını tanımak  
- Farklı düzen (layout) türlerinin nasıl kullanıldığını öğrenmek  
- HBox, VBox, GridPane, BorderPane, AnchorPane ve StackPane bileşenlerini uygulamak  
- Farklı düzenleri birleştirerek karmaşık arayüzler oluşturabilmek  

---

## 6.1 Layout Kavramı
**Layout**, JavaFX arayüzünde bileşenlerin (Button, Label, TextField vb.) **konumlandırılmasını ve hizalanmasını** sağlayan yapılardır.  
Her layout, bileşenleri farklı biçimlerde organize eder.  

---

## 6.2 HBox ve VBox ile Yatay / Dikey Düzen
- **HBox**: Elemanları yatay olarak yan yana dizer.  
- **VBox**: Elemanları dikey olarak alt alta dizer.  

### HBox
```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.HBox;
import javafx.stage.Stage;

public class HBoxExample extends Application {
    @Override
    public void start(Stage stage) {
        HBox hbox = new HBox(10); // 10 px boşluk
        hbox.getChildren().addAll(
            new Button("Kaydet"),
            new Button("Sil"),
            new Button("Güncelle")
        );

        Scene scene = new Scene(hbox, 300, 100);
        stage.setTitle("HBox Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

### VBox
```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class VBoxExample extends Application {
    @Override
    public void start(Stage stage) {
        VBox vbox = new VBox(10);
        vbox.getChildren().addAll(
            new Button("Ekle"),
            new Button("Sil"),
            new Button("Güncelle")
        );

        Scene scene = new Scene(vbox, 200, 150);
        stage.setTitle("VBox Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 6.3 GridPane ile Tablo Düzeni

`GridPane`, bileşenleri satır ve sütunlara yerleştirerek düzenler. Bu yapı, form veya tablo benzeri arayüzler için çok uygundur.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class GridPaneExample extends Application {
    @Override
    public void start(Stage stage) {
        GridPane grid = new GridPane();
        grid.setHgap(10);
        grid.setVgap(10);

        Label lblName = new Label("Ad:");
        TextField txtName = new TextField();
        Label lblDept = new Label("Bölüm:");
        TextField txtDept = new TextField();
        Button btnSave = new Button("Kaydet");

        grid.add(lblName, 0, 0);
        grid.add(txtName, 1, 0);
        grid.add(lblDept, 0, 1);
        grid.add(txtDept, 1, 1);
        grid.add(btnSave, 1, 2);

        Scene scene = new Scene(grid, 300, 200);
        stage.setTitle("GridPane Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 6.4 BorderPane Kullanımı

`BorderPane`, arayüzü beş bölgeye ayırır:
- Top
- Bottom 
- Left 
- Right 
- Center

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

public class BorderPaneExample extends Application {
    @Override
    public void start(Stage stage) {
        BorderPane border = new BorderPane();

        border.setTop(new Button("Üst"));
        border.setBottom(new Button("Alt"));
        border.setLeft(new Button("Sol"));
        border.setRight(new Button("Sağ"));
        border.setCenter(new Button("Merkez"));

        Scene scene = new Scene(border, 300, 200);
        stage.setTitle("BorderPane Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

6.5 AnchorPane ve StackPane Kullanımı

`AnchorPane`: Bileşenleri köşelere veya kenarlara sabitlemek için kullanılır.

`StackPane`: Bileşenleri üst üste (katmanlı biçimde) yerleştirir.

### AnchorPane

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.AnchorPane;
import javafx.stage.Stage;

public class AnchorPaneExample extends Application {
    @Override
    public void start(Stage stage) {
        Button btn = new Button("Sağ Alt Köşe");
        AnchorPane root = new AnchorPane(btn);

        AnchorPane.setBottomAnchor(btn, 10.0);
        AnchorPane.setRightAnchor(btn, 10.0);

        Scene scene = new Scene(root, 300, 200);
        stage.setTitle("AnchorPane Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

### StackPane

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class StackPaneExample extends Application {
    @Override
    public void start(Stage stage) {
        StackPane stack = new StackPane();
        stack.getChildren().add(new Button("Merkezde Buton"));

        Scene scene = new Scene(stack, 250, 150);
        stage.setTitle("StackPane Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 6.6 Karmaşık Arayüz Tasarımı

Aşağıda birden fazla layout yapısının birlikte kullanıldığı örnek bir arayüz gösterilmiştir:

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.*;
import javafx.stage.Stage;

public class ComplexLayout extends Application {
    @Override
    public void start(Stage stage) {
        BorderPane root = new BorderPane();

        // Üst kısım (HBox)
        HBox topBar = new HBox(10);
        topBar.getChildren().addAll(new Button("Yeni"), new Button("Kaydet"), new Button("Sil"));
        root.setTop(topBar);

        // Sol kısım (VBox)
        VBox sideBar = new VBox(10);
        sideBar.getChildren().addAll(new Button("Menü 1"), new Button("Menü 2"), new Button("Menü 3"));
        root.setLeft(sideBar);

        // Merkez (GridPane)
        GridPane form = new GridPane();
        form.setHgap(10);
        form.setVgap(10);
        form.add(new Label("Ad:"), 0, 0);
        form.add(new TextField(), 1, 0);
        form.add(new Label("Soyad:"), 0, 1);
        form.add(new TextField(), 1, 1);
        root.setCenter(form);

        Scene scene = new Scene(root, 500, 350);
        stage.setTitle("Karmaşık Arayüz Tasarımı");
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

- JavaFX’te layout kavramını anlayabilir
- Farklı düzen yöneticilerini kullanabilir
- Karmaşık arayüzlerde birden fazla layout’u bir arada uygulayabilirsiniz