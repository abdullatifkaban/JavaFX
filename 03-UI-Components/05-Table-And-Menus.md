# Tablo ve Menü Yapıları

## 🎯 Öğrenme Hedefleri
- TableView bileşenini kullanarak tablo oluşturabilmek  
- Menü çubuğu (MenuBar) oluşturmayı öğrenmek  
- Sağ tıklama menüsü (ContextMenu) ekleyebilmek  
- Menü ve tablo yapısını bir arada kullanarak küçük bir uygulama geliştirebilmek  

---

## 5.1 TableView ile Tablo Kullanımı
**TableView**, verileri satır ve sütun yapısında göstermek için kullanılır.  
Her sütun bir özelliği, her satır bir nesneyi temsil eder.  

Aşağıdaki örnekte basit bir öğrenci listesi tabloya aktarılmıştır:

```java
import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class TableViewExample extends Application {
    public static class Student {
        private final String name;
        private final int age;
        private final String department;

        public Student(String name, int age, String department) {
            this.name = name;
            this.age = age;
            this.department = department;
        }

        public String getName() { return name; }
        public int getAge() { return age; }
        public String getDepartment() { return department; }
    }

    @Override
    public void start(Stage stage) {
        TableView<Student> table = new TableView<>();

        TableColumn<Student, String> nameCol = new TableColumn<>("Ad");
        nameCol.setCellValueFactory(new PropertyValueFactory<>("name"));

        TableColumn<Student, Integer> ageCol = new TableColumn<>("Yaş");
        ageCol.setCellValueFactory(new PropertyValueFactory<>("age"));

        TableColumn<Student, String> deptCol = new TableColumn<>("Bölüm");
        deptCol.setCellValueFactory(new PropertyValueFactory<>("department"));

        table.getColumns().addAll(nameCol, ageCol, deptCol);

        ObservableList<Student> students = FXCollections.observableArrayList(
            new Student("Ahmet", 21, "Bilgisayar"),
            new Student("Zeynep", 22, "Matematik"),
            new Student("Mehmet", 23, "Fizik")
        );

        table.setItems(students);

        VBox root = new VBox(table);
        Scene scene = new Scene(root, 400, 300);
        stage.setTitle("TableView Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 5.2 Menü Çubuğu (MenuBar) Oluşturma

`MenuBar`, uygulama penceresinin üst kısmında yer alan menüleri oluşturur.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

public class MenuBarExample extends Application {
    @Override
    public void start(Stage stage) {
        Menu menuFile = new Menu("Dosya");
        MenuItem newItem = new MenuItem("Yeni");
        MenuItem exitItem = new MenuItem("Çıkış");
        menuFile.getItems().addAll(newItem, exitItem);

        Menu menuHelp = new Menu("Yardım");
        MenuItem aboutItem = new MenuItem("Hakkında");
        menuHelp.getItems().add(aboutItem);

        MenuBar menuBar = new MenuBar(menuFile, menuHelp);

        exitItem.setOnAction(e -> stage.close());
        aboutItem.setOnAction(e -> new Alert(Alert.AlertType.INFORMATION, "JavaFX Menü Örneği").show());

        BorderPane root = new BorderPane();
        root.setTop(menuBar);

        Scene scene = new Scene(root, 400, 250);
        stage.setTitle("MenuBar Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 5.3 ContextMenu (Sağ Tıklama Menüsü)

`ContextMenu`, fareyle sağ tıklandığında açılan menüdür.
Özellikle tablo veya liste bileşenlerinde sıkça kullanılır.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.ContextMenu;
import javafx.scene.control.Label;
import javafx.scene.control.MenuItem;
import javafx.scene.input.MouseButton;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class ContextMenuExample extends Application {
    @Override
    public void start(Stage stage) {
        Label label = new Label("Sağ tıklayınız");

        ContextMenu contextMenu = new ContextMenu();
        MenuItem copy = new MenuItem("Kopyala");
        MenuItem paste = new MenuItem("Yapıştır");
        contextMenu.getItems().addAll(copy, paste);

        label.setOnMouseClicked(e -> {
            if (e.getButton() == MouseButton.SECONDARY) {
                contextMenu.show(label, e.getScreenX(), e.getScreenY());
            }
        });

        StackPane root = new StackPane(label);
        Scene scene = new Scene(root, 300, 200);
        stage.setTitle("ContextMenu Örneği");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

## 5.4 Uygulama: Öğrenci Bilgi Tablosu

Bu örnekte, `TableView` ve `MenuBar` birlikte kullanılmıştır.

```java
import javafx.application.Application;
import javafx.application.Platform;
import javafx.beans.property.SimpleStringProperty;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ContextMenu;
import javafx.scene.control.Label;
import javafx.scene.control.Menu;
import javafx.scene.control.MenuBar;
import javafx.scene.control.MenuItem;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import javafx.scene.control.TextField;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class OgrenciBilgiTablosu extends Application {
    public class Student {
        private final SimpleStringProperty name; 
        private final SimpleStringProperty surname;
        private final SimpleStringProperty number;

        public Student(String name, String surname, String number) {
            this.name = new SimpleStringProperty(name);
            this.surname = new SimpleStringProperty(surname);
            this.number = new SimpleStringProperty(number);
        }

        // Property Metotları (TableView bağlama için kullanılır)
        public SimpleStringProperty nameProperty() { return name; }
        public SimpleStringProperty surnameProperty() { return surname; }
        public SimpleStringProperty numberProperty() { return number; }

        // Getter Metotları
        public String getName() { return name.get(); }
        public String getSurname() { return surname.get(); }
        public String getNumber() { return number.get(); }    
    }
    
    @Override
    public void start(Stage primaryStage) {
        // -- Tablo Oluşturma --
        TableView<Student> studentTableView = new TableView<>();
        
        TableColumn<Student, String> nameCol = new TableColumn<>("Ad");
        nameCol.setCellValueFactory(new PropertyValueFactory<>("name")); 
        
        TableColumn<Student, String> surnameCol = new TableColumn<>("Soyad");
        surnameCol.setCellValueFactory(new PropertyValueFactory<>("surname"));
        
        TableColumn<Student, String> numberCol = new TableColumn<>("Numara");
        numberCol.setCellValueFactory(new PropertyValueFactory<>("number"));

        studentTableView.getColumns().addAll(nameCol, surnameCol, numberCol);
        
        // -- Veri Listesi Tanımlama --
        ObservableList<Student> studentData = FXCollections.observableArrayList();
        
        // ObservableList tabloya bağlanır.
        studentTableView.setItems(studentData); 

        // -- Form Tasarımı --
        TextField nameField = new TextField();
        TextField surnameField = new TextField();
        TextField numberField = new TextField();
        Button addButton = new Button("Ekle");
        Button updateButton = new Button("Güncelle");
        Button deleteButton = new Button("Sil");
        
        addButton.setOnAction(e -> {
            String name = nameField.getText();
            String surname = surnameField.getText();
            String number = numberField.getText();

            if (!name.isEmpty() && !surname.isEmpty() && !number.isEmpty()) {
                Student newStudent = new Student(name, surname, number);
                studentData.add(newStudent); 
                nameField.clear();
                surnameField.clear();
                numberField.clear(); 
            }
        });
        
        // -- Menü Çubuğu Ekleme --
        Menu fileMenu = new Menu("Dosya");        
        MenuItem newItem = new MenuItem("Yeni");        
        MenuItem openItem = new MenuItem("Aç");
        MenuItem saveItem = new MenuItem("Kaydet");
        MenuItem exitItem = new MenuItem("Çıkış");
        exitItem.setOnAction(e -> Platform.exit());
        
        Menu editMenu = new Menu("Düzen");
        MenuItem cutItem = new MenuItem("Kes");
        MenuItem copyItem = new MenuItem("Kopyala");
        MenuItem pasteItem = new MenuItem("Yapıştır");

        fileMenu.getItems().addAll(newItem, openItem, saveItem, exitItem);
        editMenu.getItems().addAll(cutItem, copyItem, pasteItem);
        
        MenuBar menuBar = new MenuBar();
        menuBar.getMenus().addAll(fileMenu, editMenu);
                
        // -- ContextMenu Tanımlama --
        MenuItem ctxEditItem = new MenuItem("Düzenle"); 
        MenuItem ctxDeleteItem = new MenuItem("Sil"); 
        
        ctxDeleteItem.setOnAction(e -> {
            Student selectedStudent = studentTableView.getSelectionModel().getSelectedItem();
            if (selectedStudent != null) {
                studentData.remove(selectedStudent); 
            }
        });
        
        ContextMenu contextMenu = new ContextMenu();
        contextMenu.getItems().addAll(ctxEditItem, ctxDeleteItem);
        
        studentTableView.setContextMenu(contextMenu);
        
        // -- Pencere & Sahne Tasarımı --
        GridPane grid = new GridPane();
        grid.setPadding(new Insets(10));
        grid.setHgap(10);
        grid.setVgap(10);

        grid.addRow(0, new Label("Ad:"), nameField);
        grid.addRow(1, new Label("Soyad:"), surnameField);
        grid.addRow(2, new Label("Numara:"), numberField);

        HBox hbButtons = new HBox(10, addButton, updateButton, deleteButton);
        VBox formContainer = new VBox(10, grid, hbButtons);
        formContainer.setPadding(new Insets(10));
        
        BorderPane root = new BorderPane();
        root.setTop(menuBar);
        root.setCenter(studentTableView);
        root.setBottom(formContainer);

        Scene scene = new Scene(root, 400, 600);
        
        primaryStage.setTitle("Öğrenci Bilgi Tablosu Uygulaması");
        primaryStage.setScene(scene);
        primaryStage.show();
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
 
- TableView kullanarak veri görüntüleyebilir
- Menü çubuğu ve sağ tıklama menüsü oluşturabilir
- Tablo ve menüleri birleştirerek küçük bir veri tabanlı arayüz geliştirebilirsiniz