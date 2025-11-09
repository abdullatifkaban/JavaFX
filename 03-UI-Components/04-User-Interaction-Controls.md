# Kullanıcı Etkileşimli Kontroller

## 🎯 Öğrenme Hedefleri
- CheckBox, RadioButton, ComboBox ve ListView bileşenlerini tanımak  
- Kullanıcının seçimlerine göre işlem yapabilmek  
- Birden fazla kontrolü birlikte kullanarak basit etkileşimli arayüzler geliştirebilmek  

---

## 4.1 CheckBox Kullanımı
**CheckBox**, kullanıcıya **bir veya birden fazla** seçeneği işaretleme imkânı sunar.  

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.CheckBox;
import javafx.scene.control.Label;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class CheckBoxExample extends Application {
    @Override
    public void start(Stage stage) {
        Label lbl = new Label("Seçimleriniz:");
        CheckBox cb1 = new CheckBox("JavaFX");
        CheckBox cb2 = new CheckBox("Swing");
        CheckBox cb3 = new CheckBox("AWT");
        Button btn = new Button("Göster");

        btn.setOnAction(e -> {
            StringBuilder secimler = new StringBuilder("Seçimleriniz: ");
            if (cb1.isSelected()) secimler.append("JavaFX ");
            if (cb2.isSelected()) secimler.append("Swing ");
            if (cb3.isSelected()) secimler.append("AWT ");
            lbl.setText(secimler.toString());
        });

        VBox root = new VBox(10);
        root.getChildren().addAll(cb1, cb2, cb3, btn, lbl);

        Scene scene = new Scene(root, 300, 200);
        stage.setTitle("CheckBox Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```
## 4.2 RadioButton ile Seçenek Kontrolü

`RadioButton`, kullanıcıya bir grup içerisinden **yalnızca bir seçenek** seçme imkânı sağlar.

Bir grup tanımlamak için `ToggleGroup` kullanılır.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.control.RadioButton;
import javafx.scene.control.ToggleGroup;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class RadioButtonExample extends Application {
    @Override
    public void start(Stage stage) {
        Label lbl = new Label("Cinsiyet Seçiniz:");
        RadioButton rb1 = new RadioButton("Kadın");
        RadioButton rb2 = new RadioButton("Erkek");

        ToggleGroup group = new ToggleGroup();
        rb1.setToggleGroup(group);
        rb2.setToggleGroup(group);

        group.selectedToggleProperty().addListener((obs, oldVal, newVal) -> {
            if (newVal != null) {
                RadioButton secilen = (RadioButton) newVal;
                lbl.setText("Seçiminiz: " + secilen.getText());
            }
        });

        VBox root = new VBox(10);
        root.getChildren().addAll(rb1, rb2, lbl);

        Scene scene = new Scene(root, 250, 150);
        stage.setTitle("RadioButton Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 4.3 ComboBox ile Liste Seçimi

`ComboBox`, kullanıcıya **açılır bir menüden** seçim yapma olanağı sağlar.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.ComboBox;
import javafx.scene.control.Label;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class ComboBoxExample extends Application {
    @Override
    public void start(Stage stage) {
        Label lbl = new Label("Bir şehir seçiniz:");
        ComboBox<String> combo = new ComboBox<>();
        combo.getItems().addAll("Ankara", "İstanbul", "İzmir", "Bursa");

        combo.setOnAction(e -> lbl.setText("Seçiminiz: " + combo.getValue()));

        VBox root = new VBox(10);
        root.getChildren().addAll(combo, lbl);

        Scene scene = new Scene(root, 250, 150);
        stage.setTitle("ComboBox Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 4.4 ListView ile Çoklu Seçim

`ListView`, kullanıcıya bir **liste** sunar ve ister tekli ister çoklu seçim yapılabilir.

```java
import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.ListView;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class ListViewExample extends Application {
    @Override
    public void start(Stage stage) {
        Label lbl = new Label("Seçilen dersler:");
        ListView<String> listView = new ListView<>(FXCollections.observableArrayList(
                "Java", "Python", "C#", "C++", "Kotlin"
        ));
        listView.getSelectionModel().setSelectionMode(javafx.scene.control.SelectionMode.MULTIPLE);

        Button btn = new Button("Seçimleri Göster");
        btn.setOnAction(e -> {
            var selected = listView.getSelectionModel().getSelectedItems();
            lbl.setText("Seçilen: " + String.join(", ", selected));
        });

        VBox root = new VBox(10);
        root.getChildren().addAll(listView, btn, lbl);

        Scene scene = new Scene(root, 300, 250);
        stage.setTitle("ListView Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 4.5 Uygulama: Mini Anket

Aşağıdaki örnekte `CheckBox`, `RadioButton` ve `ComboBox` bileşenleri birleştirilmiştir.

```java
package usercontrollers;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.CheckBox;
import javafx.scene.control.Label;
import javafx.scene.control.RadioButton;
import javafx.scene.control.ToggleGroup;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;


public class UserControllers extends Application {
    
    @Override
    public void start(Stage primaryStage) {
        Label lblMesaj = new Label("Hobileriniz: ");
        Label lblSecimler = new Label("Seçimleriniz: ");
        Label lblProgramlar = new Label("Şu an kullandığınız program");
        Label lblProgramSecim = new Label("Seçtiğiniz Program: ");
        
        //CheckBoc Nesneleri
        CheckBox cbMuzik = new CheckBox();
        cbMuzik.setText("Müzik ");
        
        CheckBox cbSanat = new CheckBox("Sanat ");
        CheckBox cbSpor = new CheckBox("Spor ");
        cbSpor.setAllowIndeterminate(true);
        
        cbMuzik.setOnAction(e -> updateLabel(lblSecimler, cbMuzik, cbSanat, cbSpor));
        cbSanat.setOnAction(e -> updateLabel(lblSecimler, cbMuzik, cbSanat, cbSpor));
        cbSpor.setOnAction(e -> updateLabel(lblSecimler, cbMuzik, cbSanat, cbSpor));

        //RadioButton Nesneleri
        RadioButton rbJava = new RadioButton("Java");
        RadioButton rbPython = new RadioButton("Python");
        RadioButton rbCSharp = new RadioButton("C#");
        
        ToggleGroup tgProgramlar = new ToggleGroup();
        rbJava.setToggleGroup(tgProgramlar);
        rbPython.setToggleGroup(tgProgramlar);
        rbCSharp.setToggleGroup(tgProgramlar);
        
        rbJava.setOnAction(e -> lblProgramSecim.setText("Seçtiğiniz Program: Java"));
        rbPython.setOnAction(e -> lblProgramSecim.setText("Seçtiğiniz Program: Python"));
        rbCSharp.setOnAction(e -> lblProgramSecim.setText("Seçtiğiniz Program: C#"));

        VBox root = new VBox(10);
        root.getChildren().addAll(lblMesaj, cbMuzik, cbSanat, cbSpor, lblSecimler,
                lblProgramlar, rbJava, rbPython, rbCSharp, lblProgramSecim
                );
        
        Scene scene = new Scene(root, 300, 250);
        
        primaryStage.setTitle("Çoktan Seçim Araçları");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
    
    private void updateLabel(Label lbl, CheckBox cbm, CheckBox cbsn, CheckBox cbsp){
        String secimler = "Seçimleriniz: ";
        if (cbm.isSelected()) secimler += cbm.getText();
        if (cbsn.isSelected()) secimler += cbsn.getText();
        if (cbsp.isSelected()) secimler += cbsp.getText();
        if (cbsp.isIndeterminate()) secimler += "Belki " + cbsp.getText();
        lbl.setText(secimler);
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        launch(args);
    }
    
}
```

## ✅ Kazanımlar

Bu bölümü tamamladığınızda:

- Farklı kullanıcı etkileşim kontrollerini (CheckBox, RadioButton, ComboBox, ListView) kullanabilir
- Kullanıcı seçimlerini işleyebilir
- Birden fazla kontrolü bir arayüzde birleştirerek dinamik uygulamalar geliştirebilirsiniz