# Grafik ve Medya Kullanımı

## Özet
Bu bölümde, JavaFX uygulamalarına grafik nesneler, renk efektleri ve medya desteği eklemeyi öğreneceksiniz. Temel şekillerden başlayarak, renk yönetimi, görsel efektleri ve son olarak ses ve video entegrasyonunu inceleyeceksiniz.

---

## 8.1 JavaFX Shapes: Çizim Nesneleri

### Temel Şekil Nesneleri

JavaFX, önceden tanımlanmış şekil sınıfları sunar:

- **Line**: Düz bir çizgi
- **Circle**: Daire
- **Rectangle**: Dikdörtgen
- **Ellipse**: Elips
- **Polygon**: Çokgen
- **Polyline**: Çoklu çizgi
- **Arc**: Yay

### Basit Şekil Örneği

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.layout.Pane;
import javafx.scene.shape.Circle;
import javafx.scene.shape.Rectangle;
import javafx.scene.shape.Line;
import javafx.stage.Stage;

public class BasicShapesApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        Pane root = new Pane();
        
        // Daire oluştur
        Circle circle = new Circle(100, 100, 50);
        circle.setFill(javafx.scene.paint.Color.BLUE);
        circle.setStroke(javafx.scene.paint.Color.BLACK);
        circle.setStrokeWidth(2);
        
        // Dikdörtgen oluştur
        Rectangle rectangle = new Rectangle(200, 100, 100, 80);
        rectangle.setFill(javafx.scene.paint.Color.RED);
        rectangle.setStroke(javafx.scene.paint.Color.BLACK);
        rectangle.setStrokeWidth(2);
        
        // Çizgi oluştur
        Line line = new Line(50, 50, 250, 150);
        line.setStroke(javafx.scene.paint.Color.GREEN);
        line.setStrokeWidth(3);
        
        root.getChildren().addAll(circle, rectangle, line);
        
        Scene scene = new Scene(root, 400, 300);
        primaryStage.setTitle("Temel Şekiller");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

### Çokgen (Polygon) Örneği

```java
import javafx.scene.shape.Polygon;

// Üçgen oluştur
Polygon triangle = new Polygon(100, 50, 150, 100, 50, 100);
triangle.setFill(javafx.scene.paint.Color.YELLOW);
triangle.setStroke(javafx.scene.paint.Color.BLACK);
triangle.setStrokeWidth(2);
```

### Yay (Arc) Örneği

```java
import javafx.scene.shape.Arc;
import javafx.scene.shape.ArcType;

// Yay oluştur
Arc arc = new Arc(100, 100, 80, 80, 0, 90);
arc.setType(ArcType.ROUND);
arc.setFill(javafx.scene.paint.Color.CYAN);
arc.setStroke(javafx.scene.paint.Color.BLACK);
arc.setStrokeWidth(2);
```

---

## 8.2 Renk, Degradeler ve Efektler

### Renk Kullanımı

JavaFX `Color` sınıfı ile renk yönetimi yapılır:

```java
import javafx.scene.paint.Color;

// Önceden tanımlanmış renkler
Color red = Color.RED;
Color blue = Color.BLUE;

// RGB renk oluştur
Color customColor = Color.color(0.5, 0.7, 0.2);

// ARGB renk (alfa kanalı ile saydamlık)
Color transparentRed = Color.color(1, 0, 0, 0.5); // %50 saydamlık

// Heksadesimel renk
Color hexColor = Color.web("#FF5733");

// HSB (Hue, Saturation, Brightness)
Color hsbColor = Color.hsb(120, 0.8, 0.9);
```

### Degradeler (Gradients)

**Linear Gradient (Doğrusal Gradyan)**

```java
import javafx.scene.paint.LinearGradient;
import javafx.scene.paint.Stop;
import javafx.scene.shape.Rectangle;

Stop[] stops = new Stop[] { 
    new Stop(0, Color.RED), 
    new Stop(1, Color.YELLOW) 
};

LinearGradient gradient = new LinearGradient(
    0, 0,      // başlangıç koordinatları
    100, 100,  // bitiş koordinatları
    false,     // proportional
    javafx.scene.paint.CycleMethod.NO_CYCLE,
    stops
);

Rectangle rect = new Rectangle(100, 100);
rect.setFill(gradient);
```

**Radial Gradient (Radyal Gradyan)**

```java
import javafx.scene.paint.RadialGradient;

Stop[] stops = new Stop[] { 
    new Stop(0, Color.WHITE), 
    new Stop(1, Color.BLUE) 
};

RadialGradient radialGradient = new RadialGradient(
    0,              // fokus açısı
    0,              // fokus mesafesi
    100, 100,       // merkez
    50,             // yarıçap
    false,          // proportional
    javafx.scene.paint.CycleMethod.NO_CYCLE,
    stops
);

Circle circle = new Circle(100);
circle.setFill(radialGradient);
```

### Görsel Efektler (Effects)

**Shadow Effect (Gölge Efekti)**

```java
import javafx.scene.effect.DropShadow;
import javafx.scene.shape.Circle;

Circle circle = new Circle(50);
circle.setFill(Color.BLUE);

DropShadow shadow = new DropShadow();
shadow.setRadius(10);
shadow.setColor(Color.BLACK);
shadow.setOffsetX(5);
shadow.setOffsetY(5);

circle.setEffect(shadow);
```

**Blur Effect (Bulanıklık Efekti)**

```java
import javafx.scene.effect.GaussianBlur;

Rectangle rect = new Rectangle(100, 100);
rect.setFill(Color.RED);

GaussianBlur blur = new GaussianBlur();
blur.setRadius(10);

rect.setEffect(blur);
```

**Light Effect (Işık Efekti)**

```java
import javafx.scene.effect.Lighting;
import javafx.scene.effect.Light;

Circle circle = new Circle(50);
circle.setFill(Color.YELLOW);

Lighting lighting = new Lighting();
lighting.setLight(new Light.Point());

circle.setEffect(lighting);
```

**Motion Blur Effect (Hareket Bulanıklığı)**

```java
import javafx.scene.effect.MotionBlur;

Rectangle rect = new Rectangle(100, 100);
rect.setFill(Color.GREEN);

MotionBlur motionBlur = new MotionBlur();
motionBlur.setRadius(5);
motionBlur.setAngle(45);

rect.setEffect(motionBlur);
```

**Reflection Effect (Yansıma Efekti)**

```java
import javafx.scene.effect.Reflection;

Text text = new Text("JavaFX");
text.setFont(Font.font(30));

Reflection reflection = new Reflection();
reflection.setFraction(0.7);
reflection.setTopOpacity(0.8);

text.setEffect(reflection);
```

---

## 8.3 Filtreler ve Görsel Öğelere Efekt Ekleme

### Image Uygulaması

```java
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;

// Resim yükle
Image image = new Image("file:path/to/image.png");
ImageView imageView = new ImageView(image);
imageView.setFitWidth(200);
imageView.setFitHeight(200);
imageView.setPreserveRatio(true);

// Efekt ekle
DropShadow shadow = new DropShadow();
imageView.setEffect(shadow);
```
> [!NOTE]
> `file:path/to/image.png` adresi resmin bulunduğu yeri ifade etmektedir. Resmi, proje içerisinde `Source Packages` (src) aldında açacağınız bir klasöre (örn; `assets`) koyarsanız buraya o yolu belirten adresi yazmak gerekir. Yani `file:src/assets/image.png`

### Sepia Filter (Eski Fotoğraf Efekti)

```java
import javafx.scene.effect.ColorAdjust;

ImageView imageView = new ImageView(image);

ColorAdjust colorAdjust = new ColorAdjust();
colorAdjust.setHue(-0.1);
colorAdjust.setSaturation(-0.8);
colorAdjust.setBrightness(-0.2);

imageView.setEffect(colorAdjust);
```

### Opacity (Şeffaflık)

```java
// Şeffaflık ayarlama
circle.setOpacity(0.5);  // %50 şeffaf

// Gradual şeffaflık
rectangle.setOpacity(0.3);  // %30 şeffaf
```

### Transformation Effects (Dönüşüm Efektleri)

```java
import javafx.scene.transform.Rotate;
import javafx.scene.transform.Scale;

Rectangle rect = new Rectangle(100, 100);
rect.setFill(Color.BLUE);

// Döndürme
Rotate rotate = new Rotate(45, 50, 50);
rect.getTransforms().add(rotate);

// Ölçeklendirme
Scale scale = new Scale(1.5, 1.5, 50, 50);
rect.getTransforms().add(scale);
```

---

## 8.4 Ses Dosyası Çalma

### AudioClip Kullanarak Ses Çalma

```java
import javafx.scene.media.AudioClip;
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class AudioPlayerApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        VBox root = new VBox();
        
        // Ses dosyasını yükle
        String audioFile = "file:src/assets/sound.wav";
        AudioClip audioClip = new AudioClip(audioFile);
        
        // Düğmeler
        Button playButton = new Button("Çal");
        playButton.setOnAction(e -> audioClip.play());
        
        Button stopButton = new Button("Durdur");
        stopButton.setOnAction(e -> audioClip.stop());
        
        Button volumeUpButton = new Button("Ses Aç");
        volumeUpButton.setOnAction(e -> audioClip.setVolume(1.0));
        
        Button volumeDownButton = new Button("Ses Kapat");
        volumeDownButton.setOnAction(e -> audioClip.setVolume(0.0));
        
        root.getChildren().addAll(playButton, stopButton, volumeUpButton, volumeDownButton);
        
        Scene scene = new Scene(root, 300, 200);
        primaryStage.setTitle("Ses Oynatıcı");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

### Media ve MediaPlayer ile Video Oynatma

```java
import javafx.scene.media.Media;
import javafx.scene.media.MediaPlayer;
import javafx.scene.media.MediaView;
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class VideoPlayerApp extends Application {
    private MediaPlayer mediaPlayer;
    
    @Override
    public void start(Stage primaryStage) {
        VBox root = new VBox();
        
        // Video dosyasını yükle
        String videoFile = "file:src/assets/video.mp4";
        Media media = new Media(videoFile);
        mediaPlayer = new MediaPlayer(media);
        
        // MediaView ile video göster
        MediaView mediaView = new MediaView(mediaPlayer);
        mediaView.setFitWidth(600);
        mediaView.setFitHeight(400);
        mediaView.setPreserveRatio(true);
        
        // Kontrol Düğmeleri
        Button playButton = new Button("Oynat");
        playButton.setOnAction(e -> mediaPlayer.play());
        
        Button pauseButton = new Button("Duraklat");
        pauseButton.setOnAction(e -> mediaPlayer.pause());
        
        Button stopButton = new Button("Durdur");
        stopButton.setOnAction(e -> mediaPlayer.stop());
        
        // Ses Kontrolü
        Button volumeUpButton = new Button("Ses Aç");
        volumeUpButton.setOnAction(e -> mediaPlayer.setVolume(1.0));
        
        Button volumeDownButton = new Button("Ses Kapat");
        volumeDownButton.setOnAction(e -> mediaPlayer.setVolume(0.0));
        
        root.getChildren().addAll(mediaView, playButton, pauseButton, stopButton, 
                                  volumeUpButton, volumeDownButton);
        
        Scene scene = new Scene(root, 700, 600);
        primaryStage.setTitle("Video Oynatıcı");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 8.5 Canvas ile Serbest Çizim

Canvas, pixel bazında çizim yapmak için kullanılan bileşendir. İleri seviye grafik uygulamaları, animasyonlar ve özel efektler için idealdir.

### Basit Canvas Örneği

```java
import javafx.scene.canvas.Canvas;
import javafx.scene.canvas.GraphicsContext;
import javafx.scene.paint.Color;
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.layout.Pane;
import javafx.stage.Stage;

public class CanvasDrawingApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        Pane root = new Pane();
        
        // Canvas oluştur
        Canvas canvas = new Canvas(600, 400);
        GraphicsContext gc = canvas.getGraphicsContext2D();
        
        // Arkaplan
        gc.setFill(Color.WHITE);
        gc.fillRect(0, 0, 600, 400);
        
        // Daire çiz
        gc.setFill(Color.BLUE);
        gc.fillOval(100, 100, 100, 100);
        
        // Dikdörtgen çiz
        gc.setFill(Color.RED);
        gc.fillRect(250, 100, 150, 100);
        
        // Metin yaz
        gc.setFill(Color.BLACK);
        gc.setFont(javafx.scene.text.Font.font(16));
        gc.fillText("Canvas ile Çizim", 200, 300);
        
        root.getChildren().add(canvas);
        
        Scene scene = new Scene(root, 600, 400);
        primaryStage.setTitle("Canvas Çizim Örneği");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

---

## 8.6 Timeline ve Animasyonlar

### Timeline Kullanarak Animasyon

```java
import javafx.animation.Timeline;
import javafx.animation.KeyFrame;
import javafx.util.Duration;
import javafx.scene.shape.Circle;
import javafx.scene.paint.Color;

Circle circle = new Circle(50);
circle.setFill(Color.BLUE);
circle.setCenterX(100);
circle.setCenterY(100);

// Timeline animasyonu
Timeline timeline = new Timeline(
    new KeyFrame(Duration.seconds(0), 
        new javafx.animation.KeyValue(circle.centerXProperty(), 100)),
    new KeyFrame(Duration.seconds(2), 
        new javafx.animation.KeyValue(circle.centerXProperty(), 300))
);

timeline.setCycleCount(Timeline.INDEFINITE);
timeline.setAutoReverse(true);
timeline.play();
```

### Transition Animasyonları

```java
import javafx.animation.TranslateTransition;
import javafx.util.Duration;

TranslateTransition transition = new TranslateTransition(Duration.seconds(2), circle);
transition.setByX(200);
transition.setCycleCount(TranslateTransition.INDEFINITE);
transition.setAutoReverse(true);
transition.play();
```

---

## 8.7 Veri Görselleştirme (Charts)

### Basit Bar Chart Örneği

```java
import javafx.scene.chart.BarChart;
import javafx.scene.chart.CategoryAxis;
import javafx.scene.chart.NumberAxis;
import javafx.scene.chart.XYChart;

CategoryAxis xAxis = new CategoryAxis();
NumberAxis yAxis = new NumberAxis();

BarChart<String, Number> barChart = new BarChart<>(xAxis, yAxis);
barChart.setTitle("Satış Verileri");

XYChart.Series<String, Number> series = new XYChart.Series<>();
series.setName("2024");
series.getData().add(new XYChart.Data<>("Ocak", 100));
series.getData().add(new XYChart.Data<>("Şubat", 150));
series.getData().add(new XYChart.Data<>("Mart", 200));

barChart.getData().add(series);
```

### Basit Line Chart Örneği

```java
import javafx.scene.chart.LineChart;
import javafx.scene.chart.NumberAxis;
import javafx.scene.chart.XYChart;

NumberAxis xAxis = new NumberAxis();
NumberAxis yAxis = new NumberAxis();

LineChart<Number, Number> lineChart = new LineChart<>(xAxis, yAxis);
lineChart.setTitle("Sıcaklık Değişimi");

XYChart.Series<Number, Number> series = new XYChart.Series<>();
series.setName("Sıcaklık (°C)");
series.getData().add(new XYChart.Data<>(1, 20));
series.getData().add(new XYChart.Data<>(2, 22));
series.getData().add(new XYChart.Data<>(3, 25));

lineChart.getData().add(series);
```

---

## 8.8 CSS ile Stil Uygulama

JavaFX'te CSS kullanarak bileşenlere stil uygulanabilir:

```css
/* style.css */

.root {
    -fx-font-size: 14;
    -fx-font-family: "Arial";
}

.button {
    -fx-padding: 10;
    -fx-font-size: 14;
    -fx-text-fill: white;
    -fx-background-color: #007ACC;
}

.button:hover {
    -fx-background-color: #005a9e;
}

.label {
    -fx-text-fill: #333333;
    -fx-font-size: 12;
}
```

```java
// CSS dosyasını uygulamak
scene.getStylesheets().add("style.css");
```

---

## Özet

Bu ünite, JavaFX kullanılarak grafik ve medya bileşenlerinin nasıl kullanılacağını anlatmıştır:

- **Şekiller:** Circle, Rectangle, Polygon gibi temel şekil nesneleri
- **Renkler & Degradeler:** Solid renk, linear gradient, radial gradient
- **Efektler:** DropShadow, Blur, Lighting, Reflection vs.
- **Görseller:** Image ve ImageView ile resim yükleme ve gösterme
- **Medya:** AudioClip ile ses, Media/MediaPlayer/MediaView ile video
- **Canvas:** Pixel bazında serbest çizim
- **Animasyonlar:** Timeline ve Transition sınıfları
- **Grafikler:** BarChart, LineChart, PieChart ile veri görselleştirme
- **CSS:** JavaFX uygulamalarında stil yönetimi

---

## Uygulama Alıştırmaları

1. **Şekil Çizme:** Çeşitli şekilleri birleştirerek bir ev, araba veya yıldız çiziniz.
2. **Renk Oyunu:** Farklı degradeleri deneyerek renkli bir palette oluşturunuz.
3. **Efekt Uygulaması:** Bir resme DropShadow, Blur ve Lighting efektleri uygulayınız.
4. **Medya Oynatıcı:** Bir basit ses ve video oynatıcı uygulaması geliştiriniz.
5. **Animasyon:** Timeline kullanarak bir şekili ekranda hareket ettiriniz.
6. **Veri Grafiği:** Bir veri setini bar chart veya line chart ile görselleştiriniz.
7. **Canvas Çizim:** Mouse olaylarıyla canvas üzerine çizim yapan uygulama yazınız.

---

## Kaynaklar

- Oracle JavaFX Documentation: https://docs.oracle.com/javase/8/javafx/api/
- OpenJFX Project: https://openjfx.io/
- JavaFX CSS Reference: https://openjfx.io/javadoc/21/javafx.graphics/javafx/scene/doc-files/cssref.html