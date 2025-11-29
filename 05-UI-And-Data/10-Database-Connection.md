# Bölüm 10: Veri Tabanı Bağlantısı

## Özet
Bu bölümde JavaFX uygulamalarını veritabanına bağlama (JDBC), temel CRUD işlemleri ve NetBeans/IDE entegrasyonu ele alınacaktır. Örnek olarak SQLite ve MySQL kullanımı gösterilecektir.

---

## 10.1 JDBC'ye Giriş

JDBC (Java Database Connectivity) Java'da veritabanına erişim standartıdır. Bağlantı kurmak için genellikle şu adımlar izlenir:

1. JDBC sürücüsünü sınıf yoluna ekleyin (pom.xml veya IDE proje ayarları).
2. `DriverManager.getConnection(...)` ile bağlantı oluşturun.
3. `PreparedStatement` ile sorgu çalıştırın.
4. `ResultSet` ile sonuçları okuyun.
5. Kaynakları kapatın (try-with-resources önerilir).

### Örnek: SQLite Bağlantısı

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBUtils {
    public static Connection connectSQLite(String dbFilePath) throws SQLException {
        String url = "jdbc:sqlite:" + dbFilePath;
        return DriverManager.getConnection(url);
    }
}
```

### Örnek: MySQL Bağlantısı

```java
String url = "jdbc:mysql://localhost:3306/school?useSSL=false&serverTimezone=UTC";
String user = "root";
String pass = "password";
Connection conn = DriverManager.getConnection(url, user, pass);
```

> Not: MySQL için `mysql-connector-java` sürücüsünü projenize ekleyin.

---

## 10.2 Basit CRUD Operasyonları

### Create (Ekleme)

```java
String sql = "INSERT INTO students(name, email, birthdate) VALUES(?,?,?)";
try (PreparedStatement ps = conn.prepareStatement(sql)) {
    ps.setString(1, student.getName());
    ps.setString(2, student.getEmail());
    ps.setDate(3, java.sql.Date.valueOf(student.getBirthDate()));
    ps.executeUpdate();
}
```

### Read (Okuma)

```java
String sql = "SELECT id, name, email FROM students";
try (Statement st = conn.createStatement(); ResultSet rs = st.executeQuery(sql)) {
    while (rs.next()) {
        int id = rs.getInt("id");
        String name = rs.getString("name");
        // listeye ekle
    }
}
```

### Update (Güncelleme)

```java
String sql = "UPDATE students SET email=? WHERE id=?";
try (PreparedStatement ps = conn.prepareStatement(sql)) {
    ps.setString(1, newEmail);
    ps.setInt(2, id);
    ps.executeUpdate();
}
```

### Delete (Silme)

```java
String sql = "DELETE FROM students WHERE id=?";
try (PreparedStatement ps = conn.prepareStatement(sql)) {
    ps.setInt(1, id);
    ps.executeUpdate();
}
```

---

## 10.3 NetBeans Üzerinden Veritabanı Bağlantısı (Kısa Notlar)

- `Services -> Databases` panelinden veritabanı ekleyebilirsiniz.
- JDBC sürücüsünü projeye ekleyip, bağlantı URL'sini kullanarak test edebilirsiniz.
- NetBeans, SQL editörü ve veritabanı tarayıcı ile kolayca test yapmanızı sağlar.

---

## 10.4 JavaFX ile Veritabanı Entegrasyonu

- UI thread (JavaFX Application Thread) içinde uzun süreli DB işlemlerini çalıştırmayın. Bunun yerine `Task` veya `Service` kullanın.

### Örnek: Arka plan DB Görevi

```java
Task<Void> dbTask = new Task<>() {
    @Override
    protected Void call() throws Exception {
        try (Connection conn = DBUtils.connectSQLite("data.db")) {
            // sorgular
        }
        return null;
    }
};
new Thread(dbTask).start();
```

---

## 10.5 Güvenlik ve İyi Uygulamalar

- SQL enjeksiyonundan korunmak için `PreparedStatement` kullanın.
- Parolaları saklamayın; konfigürasyon dosyalarını güvenli tutun.
- Kaynakları kapatmak için try-with-resources kullanın.
- Büyük sorguları sayfalandırın (pagination) ve UI'de yavaş yüklemeyi önleyin.

---

## 10.6 Proje Örneği: Basit Öğrenci Bilgi Sistemi

- `students` tablosu: `id INTEGER PRIMARY KEY, name TEXT, email TEXT, birthdate DATE`
- UI: Form (ekle), TableView (listele), düzenle, sil

### Hızlı Akış

1. Kullanıcı formu doldurur -> doğrulanır
2. Arka planda DB kaydı oluşturulur
3. Başarılıysa TableView güncellenir

---

## 10.7 Bağımlılıklar ve Maven Örnek (pom.xml)

SQLite için (pom snippet):

```xml
<dependency>
    <groupId>org.xerial</groupId>
    <artifactId>sqlite-jdbc</artifactId>
    <version>3.40.0.0</version>
</dependency>
```

MySQL için:

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
```

---

## Alıştırmalar

- SQLite ile bir veritabanı dosyası oluşturun, tabloyu oluşturun ve JavaFX ile CRUD işlemleri gerçekleştirin.
- `Service` ve `Task` kullanarak DB işlemlerini arka planda çalıştırın ve UI'yi freeze olmaktan koruyun.
- Hazır test verileriyle uygulamayı test edin ve performans sorunlarını ölçün.

---

## Kaynaklar

- JDBC Tutorial: https://docs.oracle.com/javase/tutorial/jdbc/
- JavaFX Concurrency: https://openjfx.io/javadoc/
- NetBeans Database Tools
