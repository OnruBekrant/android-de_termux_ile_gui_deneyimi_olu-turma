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
    #!/data/data/com/termux/files/usr/bin/sh

    echo "Grafik arayüzü başlatılıyor..."

    # Termux:X11 sunucusunu arka planda (:0 numaralı ekranda) başlat
    termux-x11 :0 &
    
    # Sunucunun başlaması için 2 saniye bekle
    sleep 2

    # PulseAudio ses sunucusunu başlat
    pulseaudio --start

    # ÖNEMLİ: XFCE masaüstünü :0 numaralı sanal ekrana gönderen komut.
    DISPLAY=:0 dbus-launch --exit-with-session xfce4-session

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
