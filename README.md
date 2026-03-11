# Samsung Galaxy Tab S9+ İçin Termux XFCE Masaüstü Kurulum ve Yapılandırma Rehberi

> Bu rehber, güçlü bir Android tablet üzerinde Termux ve Termux:X11 kullanarak tam teşekküllü, akıcı ve yüksek çözünürlüğe (HiDPI) uyumlu bir XFCE Linux masaüstü ortamının nasıl kurulacağını ve A'dan Z'ye nasıl yapılandırılacağını anlatır.

---

## Bölüm 1: Temel Kurulum

Bu bölümde, sistemin temelini atıp masaüstü ortamını çalışır hale getireceğiz.

### **Gereksinimler**

* **Termux:** F-Droid veya GitHub üzerinden kurulmuş güncel sürüm.
* **Termux:X11:** F-Droid veya GitHub üzerinden kurulmuş güncel sürüm.
* **(Tavsiye)** Fiziksel klavye ve fare.

### **Adım 1.1: Termux'u Hazırlama**

İlk olarak Termux'u açıp temel hazırlıkları yapıyoruz.

* **Paketleri Güncelleme:**
    ```bash
    pkg update && pkg upgrade -y
    ```
* **Depolama İzni Verme:**
    ```bash
    termux-setup-storage
    ```

### **Adım 1.2: Grafik Bileşenlerini Kurma**

Masaüstü ortamı (XFCE), ses sunucusu (PulseAudio) ve grafik sunucusu (Termux:X11) için gerekli paketleri kuruyoruz.

* **X11 Deposunu Ekleme:**
    ```bash
    pkg install x11-repo -y
    ```
* **Ana Paketleri Kurma:**
    ```bash
    pkg install termux-x11-nightly xfce4 pulseaudio -y
    ```

### **Adım 1.3: Kolay Başlatma Script'i Oluşturma**

Her seferinde uzun komutlar yazmamak için masaüstünü tek komutla başlatacak bir script oluşturuyoruz.

* **Nano Editörü ile `start` Dosyasını Oluşturma:**
    ```bash
    nano start
    ```

* **Açılan Ekrana Aşağıdaki Kodları Yapıştırma:**
    ```sh
    #!/data/data/com.termux/files/usr/bin/sh

    echo "Grafik arayüzü başlatılıyor..."

    # Termux:X11 sunucusunu arka planda (:0 numaralı ekranda) başlat
    termux-x11 :0 &

    # Sunucunun başlaması için 2 saniye bekle
    sleep 2

    # DISPLAY değişkenini ayarla
    export DISPLAY=:0

    # PulseAudio ses sunucusunu başlat
    pulseaudio --start

    # ÖNEMLİ: XFCE masaüstünü :0 numaralı sanal ekrana gönderen komut.
    dbus-launch --exit-with-session xfce4-session

    echo "Oturum kapatıldı."
    ```

* **Dosyayı Kaydetme ve Çıkma:**
    `Ctrl + X` > `Y` > `Enter` tuş kombinasyonunu kullanın.

* **Script'i Çalıştırılabilir Yapma:**
    ```bash
    chmod +x start
    ```

---

## Bölüm 2: Yapılandırma ve İnce Ayarlar

Bu bölümde, ilk kurulum sonrası karşılaşılan yaygın sorunları (ölçekleme, küçük arayüz vb.) çözeceğiz.

### **Adım 2.1: Masaüstünü Başlatma**

1.  Önce **Termux:X11** uygulamasını açın ve arka plana alın.
2.  Ardından **Termux** uygulamasını açın.
3.  Oluşturduğunuz script'i çalıştırın:
    ```bash
    ./start
    ```
4.  Hemen **Termux:X11** uygulamasına geri dönün.

### **Adım 2.2: Yüksek Çözünürlük (HiDPI) İçin Ölçekleme Ayarları**

Yüksek çözünürlüklü ekranda her şeyin çok küçük görünmesi sorununu çözüyoruz.

#### **1. Genel Ölçekleme (DPI Ayarı)**
* **Yol:** `Menü > Ayarlar > Görünüm > Yazı Tipleri` sekmesi.
* **İşlem:** `Özel DPI ayarlarını kullan` kutucuğunu işaretleyin ve varsayılan `96` olan değeri `160`, `180` gibi yüksek bir değerle değiştirin. Bu, en büyük etkiyi yaratacaktır.

#### **2. Pencere Başlık Çubukları**
* **Sorun:** Pencerelerin üstündeki başlık çubuğunun (kapatma/taşıma alanı) küçük kalması.
* **Yol:** `Menü > Ayarlar > Pencere Yöneticisi > Stil` sekmesi.
* **İşlem:** `Başlık yazı tipi` ayarına tıklayın ve açılan pencereden yazı tipi boyutunu `14`, `16` gibi daha büyük bir değerle değiştirin.

#### **3. Panel Boyutu**
* **Sorun:** Ekranın altındaki veya üstündeki görev çubuğunun (panel) ince kalması.
* **Yol:** Panele sağ tıklayıp `Panel > Panel Tercihleri`.
* **İşlem:** `Satır boyutu (piksel)` değerini artırarak panelin kalınlığını ayarlayın (örn: `48` piksel).

---

## Bölüm 3: Son Dokunuşlar ve Kullanım

### **Adım 3.1: Gereksiz Paneli Kaldırma**

* **Sorun:** Ekranın altında, içindeki simgeler çalışmayan ikinci bir panelin olması.
* **Yol:** Kaldırmak istediğiniz panele sağ tıklayıp `Panel > Panel Tercihleri`.
* **İşlem:** Açılan pencerede doğru paneli (örn: `Panel 1`) seçin ve kırmızı `-` (Kaldır) butonuna basın.

### **Adım 3.2: Yeni Uygulamalar Kurma**

Masaüstü ortamınıza yeni programlar eklemek için Termux komut satırını kullanabilirsiniz.

* **Örnek Komut:**
    ```bash
    pkg install <uygulama_adı> -y
    ```

* **Tavsiye Edilen Uygulamalar:**
    * `firefox`: Tam sürüm web tarayıcısı.
    * `geany`: Hafif kod ve metin editörü.
    * `abiword`: Word belgeleri için kelime işlemci.
    * `gnumeric`: Excel tabloları için hesap tablosu programı.
    * `gimp`: Gelişmiş resim düzenleyici.

---

## Bölüm 4: VS Code Kurulumu (code-server)

Android üzerinde doğrudan VS Code çalıştırmak mümkün olmadığından, VS Code'un tarayıcı üzerinden çalışan açık kaynak portu olan **code-server** kullanılır.

### **Adım 4.1: Gerekli Paketleri Kurma**

```bash
pkg install nodejs -y
```

### **Adım 4.2: code-server'ı Kurma**

```bash
npm install -g code-server
```

> **Not:** Bu işlem ARM64 mimarisinde birkaç dakika sürebilir, sabırla bekleyin.

### **Adım 4.3: code-server'ı Başlatma**

Termux'ta aşağıdaki komutu çalıştırın:

```bash
code-server --bind-addr 127.0.0.1:8080 --auth none
```

### **Adım 4.4: VS Code'a Erişim**

#### **Seçenek A: XFCE İçinden Tarayıcı ile (Tavsiye Edilen)**
1. XFCE masaüstünü başlatın (`./start`).
2. XFCE içinde **Firefox** tarayıcısını açın.
3. Adres çubuğuna şunu yazın: `http://127.0.0.1:8080`
4. VS Code arayüzü tarayıcıda açılacaktır.

#### **Seçenek B: Android Tarayıcısı ile**
1. Termux'ta code-server'ı başlatın.
2. Android'deki herhangi bir tarayıcıda (Chrome, Firefox vb.) `http://127.0.0.1:8080` adresini açın.

### **Adım 4.5: code-server'ı Otomatik Başlatma (İsteğe Bağlı)**

`start` script'inize code-server'ı da eklemek için aşağıdaki satırı `dbus-launch` satırından önce ekleyin:

```sh
# code-server'ı arka planda başlat
code-server --bind-addr 127.0.0.1:8080 --auth none &
```

### **Adım 4.6: Türkçe Dil Desteği Ekleme**

code-server arayüzünde Türkçe kullanmak için:
1. VS Code'u açın (`http://127.0.0.1:8080`).
2. `Ctrl+Shift+X` ile Eklentiler panelini açın.
3. `Turkish Language Pack` aratın ve kurun.
4. VS Code'u yenileyin.

---

## Bölüm 5: Sorun Giderme

### **Sorun 1: Ekran Açılmıyor / "No Display" Hatası**
* **Çözüm:** Termux:X11 uygulamasının arka planda açık olduğundan emin olun. `start` scriptini çalıştırmadan önce Termux:X11'i açıp arka plana alın.

### **Sorun 2: Ses Çalışmıyor**
* **Çözüm:** PulseAudio'yu elle yeniden başlatın:
    ```bash
    pulseaudio --kill
    pulseaudio --start
    ```

### **Sorun 3: Arayüz Çok Küçük Görünüyor**
* **Çözüm:** Bölüm 2.2'deki HiDPI ayarlarını uygulayın. DPI değerini `160`–`192` arasında deneyin.

### **Sorun 4: `pkg upgrade` Sırasında Hata**
* **Çözüm:** Termux depo listesini güncelleyin:
    ```bash
    pkg update -y && pkg upgrade -y
    ```
    Eğer sorun devam ederse:
    ```bash
    pkg install termux-tools -y
    termux-fix-shebang
    ```

### **Sorun 5: code-server Çok Yavaş**
* **Çözüm:** Daha az eklenti açık tutun. Aşırı bellek kullanımını önlemek için `--max-old-space-size` sınırı ekleyin:
    ```bash
    NODE_OPTIONS="--max-old-space-size=512" code-server --bind-addr 127.0.0.1:8080 --auth none
    ```

### **Sorun 6: XFCE Oturumu Donuyor**
* **Çözüm:** Termux'a dönüp `xfce4-session`'ı yeniden başlatın:
    ```bash
    pkill xfce4-session
    export DISPLAY=:0
    dbus-launch --exit-with-session xfce4-session &
    ```

---
