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

<details>
<summary><b>Uygulama kodları</b></summary>

```java
import javafx.application.Application;
import javafx.beans.property.SimpleStringProperty;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;
import java.time.LocalDate;

public class Grafik extends Application {
// Öğrenci sınıfı (Tablo için)

    public static class Student {

        private final SimpleStringProperty name;
        private final SimpleStringProperty surname;
        private final SimpleStringProperty email;
        private final SimpleStringProperty gender;
        private final SimpleStringProperty birthDate;
        private final SimpleStringProperty department;
        private final SimpleStringProperty hobbies;

        public Student(String name, String surname, String email, String gender,
                String birthDate, String department, String hobbies) {
            this.name = new SimpleStringProperty(name);
            this.surname = new SimpleStringProperty(surname);
            this.email = new SimpleStringProperty(email);
            this.gender = new SimpleStringProperty(gender);
            this.birthDate = new SimpleStringProperty(birthDate);
            this.department = new SimpleStringProperty(department);
            this.hobbies = new SimpleStringProperty(hobbies);
        }

        public String getName() {
            return name.get();
        }

        public void setName(String value) {
            name.set(value);
        }

        public String getSurname() {
            return surname.get();
        }

        public void setSurname(String value) {
            surname.set(value);
        }

        public String getEmail() {
            return email.get();
        }

        public void setEmail(String value) {
            email.set(value);
        }

        public String getGender() {
            return gender.get();
        }

        public void setGender(String value) {
            gender.set(value);
        }

        public String getBirthDate() {
            return birthDate.get();
        }

        public void setBirthDate(String value) {
            birthDate.set(value);
        }

        public String getDepartment() {
            return department.get();
        }

        public void setDepartment(String value) {
            department.set(value);
        }

        public String getHobbies() {
            return hobbies.get();
        }

        public void setHobbies(String value) {
            hobbies.set(value);
        }
    }
    private final ObservableList<Student> studentList = FXCollections.observableArrayList();

    @Override
    public void start(Stage primaryStage) {
// GridPane - Form
        GridPane grid = new GridPane();
        grid.setPadding(new Insets(20));
        grid.setHgap(10);
        grid.setVgap(10);
// Form alanları
        Label nameLabel = new Label("Ad:");
        TextField nameField = new TextField();
        Label surnameLabel = new Label("Soyad:");
        TextField surnameField = new TextField();
        Label emailLabel = new Label("E-posta:");
        TextField emailField = new TextField();
        Label passLabel = new Label("Şifre:");
        PasswordField passwordField = new PasswordField();
        Label genderLabel = new Label("Cinsiyet:");
        RadioButton male = new RadioButton("Erkek");
        RadioButton female = new RadioButton("Kadın");
        ToggleGroup genderGroup = new ToggleGroup();
        male.setToggleGroup(genderGroup);
        female.setToggleGroup(genderGroup);
        Label birthLabel = new Label("Doğum Tarihi:");
        DatePicker birthDatePicker = new DatePicker();
        Label deptLabel = new Label("Bölüm:");
        ComboBox<String> deptBox = new ComboBox<>();
        deptBox.getItems().addAll("Bilgisayar", "Endüstri", "Elektrik", "Makine");
        Label hobbiesLabel = new Label("Hobiler:");
        CheckBox cb1 = new CheckBox("Spor");
        CheckBox cb2 = new CheckBox("Müzik");
        CheckBox cb3 = new CheckBox("Kitap");
        Button registerBtn = new Button("Kayıt Ol");
        Button updateBtn = new Button("Güncelle");
        Button deleteBtn = new Button("Sil");
// GridPane düzeni
        grid.add(nameLabel, 0, 0);
        grid.add(nameField, 1, 0);
        grid.add(surnameLabel, 0, 1);
        grid.add(surnameField, 1, 1);
        grid.add(emailLabel, 0, 2);
        grid.add(emailField, 1, 2);
        grid.add(passLabel, 0, 3);
        grid.add(passwordField, 1, 3);
        grid.add(genderLabel, 0, 4);
        grid.add(male, 1, 4);
        grid.add(female, 2, 4);
        grid.add(birthLabel, 0, 5);
        grid.add(birthDatePicker, 1, 5);
        grid.add(deptLabel, 0, 6);
        grid.add(deptBox, 1, 6);
        grid.add(hobbiesLabel, 0, 7);
        grid.add(cb1, 1, 7);
        grid.add(cb2, 2, 7);
        grid.add(cb3, 3, 7);
        grid.add(registerBtn, 1, 8);
        grid.add(updateBtn, 2, 8);
        grid.add(deleteBtn, 3, 8);
// Tablo - Öğrencileri listeleme
        TableView<Student> table = new TableView<>(studentList);
        table.setPrefHeight(200);
        table.getColumns().addAll(
                createColumn("Ad", "name", 100),
                createColumn("Soyad", "surname", 100),
                createColumn("E-posta", "email", 150),
                createColumn("Cinsiyet", "gender", 80),
                createColumn("Doğum Tarihi", "birthDate", 100),
                createColumn("Bölüm", "department", 120),
                createColumn("Hobiler", "hobbies", 150)
        );
// Kayıt Ol butonu
        registerBtn.setOnAction(e -> {
            Student student = validateAndCreateStudent(
                    nameField, surnameField, emailField, passwordField,
                    genderGroup, birthDatePicker, deptBox, cb1, cb2, cb3
            );
            if (student != null) {
                studentList.add(student);
                clearForm(nameField, surnameField, emailField, passwordField,
                        genderGroup, birthDatePicker, deptBox, cb1, cb2, cb3);
            }
        });
// Güncelle butonu
        updateBtn.setOnAction(e -> {
            Student selected = table.getSelectionModel().getSelectedItem();
            if (selected == null) {
                showError("Güncellenecek bir öğrenci seçiniz!");
                return;
            }
            Student updated = validateAndCreateStudent(
                    nameField, surnameField, emailField, passwordField,
                    genderGroup, birthDatePicker, deptBox, cb1, cb2, cb3
            );
            if (updated != null) {
                selected.setName(updated.getName());
                selected.setSurname(updated.getSurname());
                selected.setEmail(updated.getEmail());
                selected.setGender(updated.getGender());
                selected.setBirthDate(updated.getBirthDate());
                selected.setDepartment(updated.getDepartment());
                selected.setHobbies(updated.getHobbies());
                table.refresh();
                clearForm(nameField, surnameField, emailField, passwordField,
                        genderGroup, birthDatePicker, deptBox, cb1, cb2, cb3);
            }
        });
// Sil butonu
        deleteBtn.setOnAction(e -> {
            Student selected = table.getSelectionModel().getSelectedItem();
            if (selected != null) {
                studentList.remove(selected);
            } else {
                showError("Silinecek bir öğrenci seçiniz!");
            }
        });
// Tablo seçimi ile formu doldurma
        table.getSelectionModel().selectedItemProperty().addListener((obs, oldSel, newSel) -> {
            if (newSel != null) {
                nameField.setText(newSel.getName());
                surnameField.setText(newSel.getSurname());
                emailField.setText(newSel.getEmail());
                if (newSel.getGender().equals("Erkek")) {
                    genderGroup.selectToggle(male);
                } else {
                    genderGroup.selectToggle(female);
                }
                birthDatePicker.setValue(LocalDate.parse(newSel.getBirthDate()));
                deptBox.setValue(newSel.getDepartment());
                cb1.setSelected(newSel.getHobbies().contains("Spor"));
                cb2.setSelected(newSel.getHobbies().contains("Müzik"));
                cb3.setSelected(newSel.getHobbies().contains("Kitap"));
            }
        });
        VBox root = new VBox(15, grid, table);
        root.setPadding(new Insets(15));
        Scene scene = new Scene(root, 800, 600);
        primaryStage.setTitle("Öğrenci Kayıt Sistemi");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
// Tablo için kolon oluşturma

    private TableColumn<Student, String> createColumn(String title, String property, int width) {
        TableColumn<Student, String> col = new TableColumn<>(title);
        col.setCellValueFactory(new javafx.scene.control.cell.PropertyValueFactory<>(property));
        col.setPrefWidth(width);
        return col;
    }
// Öğrenci doğrulama ve nesne oluşturma

    private Student validateAndCreateStudent(TextField nameField, TextField surnameField,
            TextField emailField, PasswordField passwordField,
            ToggleGroup genderGroup, DatePicker birthDatePicker,
            ComboBox<String> deptBox, CheckBox cb1, CheckBox cb2, CheckBox cb3) {
        String name = nameField.getText().trim();
        String surname = surnameField.getText().trim();
        String email = emailField.getText().trim();
        String password = passwordField.getText().trim();
        RadioButton selectedGender = (RadioButton) genderGroup.getSelectedToggle();
        LocalDate birthDate = birthDatePicker.getValue();
        String dept = deptBox.getSelectionModel().getSelectedItem();
        StringBuilder hobbies = new StringBuilder();
        if (cb1.isSelected()) {
            hobbies.append("Spor ");
        }
        if (cb2.isSelected()) {
            hobbies.append("Müzik ");
        }
        if (cb3.isSelected()) {
            hobbies.append("Kitap ");
        }
// Doğrulamalar
        if (name.isEmpty() || surname.isEmpty()) {
            showError("Ad ve Soyad boş bırakılamaz!");
            return null;
        }
        if (!email.matches("^[A-Za-z0-9+_.-]+@(.+)$")) {
            showError("Geçerli bir e-posta adresi giriniz!");
            return null;
        }
        if (password.length() < 6) {
            showError("Şifre en az 6 karakter olmalıdır!");
            return null;
        }
        if (selectedGender == null) {
            showError("Cinsiyet seçiniz!");
            return null;
        }
        if (birthDate == null || birthDate.isAfter(LocalDate.now())) {
            showError("Geçerli bir doğum tarihi seçiniz!");
            return null;
        }
        if (dept == null) {
            showError("Lütfen bir bölüm seçiniz!");
            return null;
        }
        return new Student(name, surname, email, selectedGender.getText(),
                birthDate.toString(), dept, hobbies.toString());
    }
// Formu temizleme

    private void clearForm(TextField nameField, TextField surnameField, TextField emailField,
            PasswordField passwordField, ToggleGroup genderGroup,
            DatePicker birthDatePicker, ComboBox<String> deptBox,
            CheckBox cb1, CheckBox cb2, CheckBox cb3) {
        nameField.clear();
        surnameField.clear();
        emailField.clear();
        passwordField.clear();
        genderGroup.selectToggle(null);
        birthDatePicker.setValue(null);
        deptBox.setValue(null);
        cb1.setSelected(false);
        cb2.setSelected(false);
        cb3.setSelected(false);
    }
// Hata mesajı

    private void showError(String message) {
        Alert alert = new Alert(Alert.AlertType.ERROR);
        alert.setTitle("Hata");
        alert.setHeaderText(null);
        alert.setContentText(message);
        alert.showAndWait();
    }

    public static void main(String[] args) {
        launch(args);
    }
}

```
</details>

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
