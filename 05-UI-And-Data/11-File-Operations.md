# Dosya İşlemleri

## Özet

Bu bölümde, JavaFX uygulamalarında dosya işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz. Temel dosya açma, okuma, yazma işlemlerinden başlayarak, `FileChooser` ile kullanıcı dostu dosya seçim dialogları oluşturmaya ve sonunda basit bir metin editörü uygulaması geliştirmeye kadar ilerliyoruz.

---

## 11.1 Dosya İşlemlerinin Temel Kavramları

### Dosya Nedir?

Dosya, bilgisayarda saklanan her türlü veri birimidir. Yazılım geliştirmede en çok kullanılan dosya türleri:

- **Metin Dosyaları (Text Files):** İçinde sadece karakter tabanlı veriler bulunur (.txt, .csv)
- **İkili Dosyalar (Binary Files):** Resim, PDF, ses, video gibi veriler içerir

### Dosya Yolu (Path) Kavramı

- **Mutlak Yol (Absolute Path):** Dosyanın kök dizinden başlayarak tam konumunu gösterir.
  - Windows: `C:\Users\Ali\Documents\notlar.txt`
  - Linux/Mac: `/home/ali/notlar.txt`

- **Göreli Yol (Relative Path):** Programın çalıştığı klasöre göre tanımlanan yoldur.
  - `data/notlar.txt` → Programın bulunduğu klasördeki data alt klasöründe aranır.

Göreli yol kullanmak projenin farklı bilgisayarlarda sorunsuz çalışmasını sağlar.

---

## 11.2 Java'da Dosya Sınıfları (File, Path, Files)

### File Sınıfı

`java.io.File` sınıfı, bir dosya veya klasör hakkında bilgi almak için kullanılır. Dosyanın var olup olmadığını, yazılabilir durumda olup olmadığını ve boyutunu sorgulamak mümkündür.

```java
import java.io.File;

File file = new File("notlar.txt");
if (file.exists()) {
    System.out.println("Dosya bulundu!");
    System.out.println("Boyut: " + file.length() + " byte");
} else {
    System.out.println("Dosya yok.");
}
```

### Path Sınıfı (Modern Yaklaşım)

`java.nio.file.Path` sınıfı, dosya yollarını yönetmek için daha modern bir yaklaşım sunar:

```java
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.Files;

Path path = Paths.get("notlar.txt");

// Dosya var mı kontrol et
if (Files.exists(path)) {
    System.out.println("Dosya bulundu!");
    System.out.println("Boyut: " + Files.size(path) + " byte");
}
```

---

## 11.3 Text Dosyalarını Okuma

### BufferedReader ile Dosya Okuma

`BufferedReader` sınıfı, metin dosyalarını satır satır okunması için en çok kullanılan sınıftır:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

try (BufferedReader br = new BufferedReader(new FileReader("notlar.txt"))) {
    String satir;
    while ((satir = br.readLine()) != null) {
        System.out.println(satir);
    }
} catch (IOException e) {
    System.out.println("Dosya okunurken hata oluştu: " + e.getMessage());
}
```

### Scanner ile Dosya Okuma

`Scanner` sınıfı, küçük boyutlu dosyalar için daha basit ve anlaşılır bir kullanım sunar:

```java
import java.io.File;
import java.util.Scanner;

try {
    Scanner scanner = new Scanner(new File("notlar.txt"));
    while (scanner.hasNextLine()) {
        String satir = scanner.nextLine();
        System.out.println(satir);
    }
    scanner.close();
} catch (Exception e) {
    System.out.println("Dosya bulunamadı: " + e.getMessage());
}
```

### Files Sınıfı (En Modern Yaklaşım)

Tüm satırları bir kez okumak için:

```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

try {
    List<String> satirlar = Files.readAllLines(Paths.get("notlar.txt"));
    satirlar.forEach(System.out::println);
} catch (Exception e) {
    System.out.println("Hata: " + e.getMessage());
}
```

---

## 11.4 Text Dosyalarına Yazma

### FileWriter ile Dosyaya Yazma

```java
import java.io.FileWriter;
import java.io.IOException;

try (FileWriter writer = new FileWriter("notlar.txt")) {
    writer.write("Merhaba Java\n");
    writer.write("Bu dosya FileWriter kullanılarak yazıldı.\n");
    System.out.println("Dosya başarıyla kaydedildi.");
} catch (IOException e) {
    System.out.println("Dosya yazılırken hata oluştu: " + e.getMessage());
}
```

### PrintWriter ile Dosyaya Yazma

`PrintWriter` farklı veri türlerini kolayca yazabilir:

```java
import java.io.PrintWriter;

try (PrintWriter pw = new PrintWriter("cikti.txt")) {
    pw.println("Merhaba Dünya");
    pw.println(123);
    pw.println(45.67);
    System.out.println("Dosya kaydedildi.");
} catch (Exception e) {
    System.out.println("Hata: " + e.getMessage());
}
```

### Files.write() (En Modern Yaklaşım)

```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.Arrays;

try {
    Files.write(Paths.get("notlar.txt"), 
                Arrays.asList("Satır 1", "Satır 2", "Satır 3"));
    System.out.println("Dosya kaydedildi.");
} catch (Exception e) {
    System.out.println("Hata: " + e.getMessage());
}
```

---

## 11.5 FileChooser ile Dosya Seçimi

### Dosya Açma (Open) Diyaloğu

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.VBox;
import javafx.stage.FileChooser;
import javafx.stage.Stage;
import java.io.File;

public class FileChooserApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        VBox root = new VBox(10);
        
        Button openButton = new Button("Dosya Aç");
        openButton.setOnAction(e -> {
            FileChooser fileChooser = new FileChooser();
            fileChooser.setTitle("Bir dosya seçiniz");
            fileChooser.getExtensionFilters().add(
                new FileChooser.ExtensionFilter("Metin Dosyaları", "*.txt")
            );
            
            File selectedFile = fileChooser.showOpenDialog(primaryStage);
            if (selectedFile != null) {
                System.out.println("Seçilen dosya: " + selectedFile.getAbsolutePath());
            }
        });
        
        root.getChildren().add(openButton);
        Scene scene = new Scene(root, 400, 300);
        primaryStage.setScene(scene);
        primaryStage.setTitle("Dosya Seçimi");
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

### Dosya Kaydetme (Save) Diyaloğu

```java
Button saveButton = new Button("Dosya Kaydet");
saveButton.setOnAction(e -> {
    FileChooser fileChooser = new FileChooser();
    fileChooser.setTitle("Dosya Kaydet");
    fileChooser.getExtensionFilters().add(
        new FileChooser.ExtensionFilter("Metin Dosyaları", "*.txt")
    );
    
    File file = fileChooser.showSaveDialog(primaryStage);
    if (file != null) {
        try (PrintWriter pw = new PrintWriter(file)) {
            pw.println("Kaydedilecek içerik");
            System.out.println("Dosya kaydedildi: " + file.getAbsolutePath());
        } catch (Exception ex) {
            System.out.println("Hata: " + ex.getMessage());
        }
    }
});
```

### Çoklu Dosya Seçme

```java
Button multiButton = new Button("Birden Fazla Dosya Seç");
multiButton.setOnAction(e -> {
    FileChooser fileChooser = new FileChooser();
    fileChooser.setTitle("Dosyaları seçiniz");
    
    List<File> files = fileChooser.showOpenMultipleDialog(primaryStage);
    if (files != null) {
        files.forEach(f -> System.out.println(f.getAbsolutePath()));
    }
});
```

---

## 11.6 Klasörlerle Çalışma

### Klasör Oluşturma ve Listeleme

```java
import java.io.File;
import java.nio.file.Files;
import java.nio.file.Paths;

// Klasör oluştur
File folder = new File("myFolder");
if (!folder.exists()) {
    folder.mkdir();
    System.out.println("Klasör oluşturuldu.");
}

// Klasör içeriğini listele
File[] files = folder.listFiles();
if (files != null) {
    for (File f : files) {
        System.out.println(f.getName());
    }
}
```

### Dosya/Klasör Silme

```java
File file = new File("notlar.txt");
if (file.delete()) {
    System.out.println("Dosya silindi.");
} else {
    System.out.println("Dosya silinemedi.");
}
```

---

## 11.7 Uygulama: Basit Metin Editörü

Dosya işlemlerini pekiştirmek için basit bir metin editörü uygulaması:

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.BorderPane;
import javafx.stage.FileChooser;
import javafx.stage.Stage;
import java.io.File;
import java.io.PrintWriter;
import java.nio.file.Files;

public class SimpleTextEditor extends Application {
    private TextArea textArea;
    private Label statusLabel;
    private File currentFile;
    
    @Override
    public void start(Stage primaryStage) {
        BorderPane root = new BorderPane();
        
        // Menu Bar
        MenuBar menuBar = new MenuBar();
        Menu fileMenu = new Menu("Dosya");
        
        MenuItem openItem = new MenuItem("Aç");
        openItem.setOnAction(e -> openFile(primaryStage));
        
        MenuItem saveItem = new MenuItem("Kaydet");
        saveItem.setOnAction(e -> saveFile());
        
        MenuItem exitItem = new MenuItem("Çık");
        exitItem.setOnAction(e -> primaryStage.close());
        
        fileMenu.getItems().addAll(openItem, saveItem, new SeparatorMenuItem(), exitItem);
        menuBar.getMenus().add(fileMenu);
        
        // Text Area
        textArea = new TextArea();
        textArea.setWrapText(true);
        
        // Status Label
        statusLabel = new Label("Hazır");
        
        root.setTop(menuBar);
        root.setCenter(textArea);
        root.setBottom(statusLabel);
        
        Scene scene = new Scene(root, 600, 400);
        primaryStage.setScene(scene);
        primaryStage.setTitle("Basit Metin Editörü");
        primaryStage.show();
    }
    
    private void openFile(Stage stage) {
        FileChooser fileChooser = new FileChooser();
        fileChooser.setTitle("Dosya Aç");
        fileChooser.getExtensionFilters().add(
            new FileChooser.ExtensionFilter("Metin Dosyaları", "*.txt")
        );
        
        File file = fileChooser.showOpenDialog(stage);
        if (file != null) {
            try {
                String content = new String(Files.readAllBytes(file.toPath()));
                textArea.setText(content);
                currentFile = file;
                statusLabel.setText("Açıldı: " + file.getName());
            } catch (Exception e) {
                statusLabel.setText("Hata: " + e.getMessage());
            }
        }
    }
    
    private void saveFile() {
        if (currentFile != null) {
            try (PrintWriter pw = new PrintWriter(currentFile)) {
                pw.print(textArea.getText());
                statusLabel.setText("Kaydedildi: " + currentFile.getName());
            } catch (Exception e) {
                statusLabel.setText("Hata: " + e.getMessage());
            }
        }
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 11.8 Uygulama Alıştırmaları

1. **Dosya Yedekleme Aracı:** Seçilen bir dosyayı başka bir konuma kopyalayan uygulama yazın.
2. **CSV Veri Okuycusu:** Bir CSV dosyasını okuyup içeriğini TableView'de gösteren uygulama.
3. **Log Dosyası İzleyici:** Bir log dosyasının son satırlarını izleyip güncelleyen uygulama.
4. **Not Defteri Uygulaması:** Yukarıdaki örneği geliştirerek "Farklı Kaydet" ve "Yeni" özellikleri ekleyin.

---

## 11.9 Önemli Notlar

- **try-with-resources:** Dosya işlemlerinde mutlaka kullanın; dosyaların otomatik kapanmasını sağlar.
- **Hata Yönetimi:** IOException şiddetle beklenir ve mutlaka işlenmelidir.
- **Göreli Yollar:** Proje taşınabilirliği için mutlak yol yerine göreli yollar tercih edin.
- **FileChooser:** Kullanıcı dostu uygulamalar için daima FileChooser kullanın.
