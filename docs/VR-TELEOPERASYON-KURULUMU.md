> Bu belge NVlabs/GR00T-WholeBodyControl deposundaki `docs/source/getting_started/vr_teleop_setup.md` dosyasının Türkçe çevirisidir (Apache-2.0, commit 1e1ec4f).

# VR Teleoperasyon Kurulumu (PICO)

Bu sayfa, PICO VR ile tüm gövde teleoperasyonu için tek seferlik donanım ve yazılım kurulumunu anlatır. Bu adımları tamamladıktan sonra teleoperasyonu simülasyonda ya da gerçek donanımda çalıştırmak için [ZMQ Manager eğitimine](VR-TUM-GOVDE-TELEOPERASYON.md) geçin.

> **Not:** Aşağıdaki adımlar XRoboToolkit kullanır. IsaacCapture kullanmak için 1. ve 2. adımları tamamlayın, ardından [Isaac Capture Kurulumu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/isaac_teleop_publisher_setup.md) ile devam edin.

---

## Gerekli donanım

- [PICO 4 / PICO 4 Pro başlık](https://www.picoxr.com/global/products/pico4)
- [2 adet PICO kontrolcüsü](https://www.picoxr.com/global/products/pico4)
- [2 adet PICO hareket izleyicisi (motion tracker)](https://www.picoxr.com/global/products/pico-motion-tracker) (ayak bileklerine bağlanır)
- Yüksek hızlı, düşük gecikmeli bir Wi-Fi bağlantısı. Teleoperasyon performansı büyük ölçüde ağ kalitesine bağlıdır.

---

## 1. adım: Hareket izleyici kurulumu

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/pico_setup/pico_setup_screenshot.png" width="600" alt="PICO hareket izleyici kurulum ekranı">
</p>

1. Bir PICO hareket izleyicisini sol ayak bileğinize, birini de sağ ayak bileğinize bağlayın. İzleyiciler görünsün diye bol giysileri **toplayıp** aşağı indirin. Işık göstergeli yüzün yukarı baktığından emin olun.
2. PICO ayarlarına gidin. Soldaki menüde en alttaki seçeneğe, **"Developer"** seçeneğine inin. **"Safeguard"** ayarının kapalı olduğundan emin olun.
   - Developer seçeneği etkin değilse görünene kadar "Software" seçeneğine dokunun.
3. PICO menüsündeki **Wi-Fi simgesine** tıklayın. Başlığın bir resmi görünür. Başlığın üstünde hareket izleyiciler için küçük, yuvarlak bir logo bulunur. Logo görünmüyorsa **"Motion Tracker"** uygulamasını doğrudan açın.
   - Başlık ve 2 kontrolcü listelenir. **Motion Tracker** (küçük daire) seçeneğini seçin.
4. Her izleyicinin yanında bir **"i"** simgesi bulunur. Buna tıklayın ve **tüm izleyicilerin eşleşmesini kaldırın**.
5. Tüm izleyiciler temizlenince sağ üst köşedeki **"Pair"** düğmesine tıklayın.
6. Her hareket izleyicinin üstündeki düğmeyi **6 saniye** basılı tutun. Eşleştirme kipine geçtiğinde ışıklar kırmızı ve mavi yanıp söner.

### Hareket izleyici kalibrasyonu

1. PICO başlığı gözlerinizin üstüne takın.
2. Mavi **"Calibrate"** düğmesine basın ve iki kalibrasyon dizisini izleyin:
   - **1. dizi:** El kontrolcüleri iki yanınızda aşağıda, dimdik durun.
   - **2. dizi:** Başlık kameraları ayak izleyicilerini tanıyana kadar aşağıya, ayaklarınızdaki izleyicilere bakın.
3. Kalibrasyon bitince PICO başlığı alnınızın çevresine takın (hareket izleyicileri algılamayı sürdürmesi için PICO'nun öne baktığından emin olun).

---

## 2. adım: PICO teleoperasyon ortamını kurun

**Depo kökünden:**

```bash
bash install_scripts/install_pico.sh
```

Bu komut, şunları içeren bir `.venv_teleop` sanal ortamı (Python 3.11) oluşturur:
- `teleop` eki (ZMQ, Pinocchio, PyVista)
- `sim` eki (MuJoCo, tyro)
- XRoboToolkit SDK
- CloudXR çalışma zamanıyla birlikte IsaacCapture 1.5
- Unitree SDK2 Python bağlamaları

Ortamı şu komutla etkinleştirin:

```bash
source .venv_teleop/bin/activate   # istem: (gear_sonic_teleop)
```

---

## 3. adım: XRoboToolkit'i kurun

XRoboToolkit, bir PC hizmetinden (iş istasyonunuzda çalışır) ve gövde izleme verisini akıtan bir PICO uygulamasından (başlıkta çalışır) oluşur.

### PC hizmeti

PICO'nun bağlanabilmesi için PC hizmeti iş istasyonunuza **önceden** kurulmuş ve çalışıyor olmalıdır.

**Ubuntu 22.04 (x86_64 iş istasyonu):**

```bash
wget https://github.com/XR-Robotics/XRoboToolkit-PC-Service/releases/download/v1.0.0/XRoboToolkit_PC_Service_1.0.0_ubuntu_22.04_amd64.deb
sudo dpkg -i XRoboToolkit_PC_Service_1.0.0_ubuntu_22.04_amd64.deb
```

**Ubuntu 24.04 (x86_64 iş istasyonu):**

```bash
wget https://github.com/XR-Robotics/XRoboToolkit-PC-Service/releases/download/v1.0.0/XRoboToolkit_PC_Service_1.0.0_ubuntu_24.04_amd64.deb
sudo dpkg -i XRoboToolkit_PC_Service_1.0.0_ubuntu_24.04_amd64.deb
```

**Jetson (aarch64):**

```bash
sudo dpkg -i gear_sonic_deploy/thirdparty/roboticsservice_1.0.0.0_arm64.deb
```

Öteki platformlar ya da yeni sürümler için [XRoboToolkit-PC-Service sürümlerine](https://github.com/XR-Robotics/XRoboToolkit-PC-Service/releases) bakın.

### PICO uygulaması

1. Kurulum ve yükleme sürecini başlatmak için PICO başlığı takın.
2. PICO'daki hızlı kurulumu tamamlayın.
3. PICO'nun Wi-Fi'ye bağlı olduğundan emin olun.
4. PICO'daki tarayıcı uygulamasını açın.
5. Arama çubuğuna **"xrobotoolkit"** yazın ve [https://github.com/XR-Robotics](https://github.com/XR-Robotics) GitHub sayfasını seçin.

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/pico_setup/google_search_screenshot.png" width="600" alt="PICO tarayıcısında XRoboToolkit araması">
</p>

6. **Developer Mode** ayarının açık olduğundan emin olun (Settings → Developer).
7. **[PICO İÇİNDE]** GitHub sayfasında APK indirme seçeneğini görene kadar aşağı kaydırın ve indirmek için PICO tetiğiyle tıklayın.

> **İpucu:** [XRoboToolkit-PICO-1.1.1.apk](https://github.com/XR-Robotics/XRoboToolkit-Unity-Client/releases/download/v1.1.1/XRoboToolkit-PICO-1.1.1.apk) dosyasını PICO'da tarayıcıyla indirin. ([Öteki sürümler](https://github.com/XR-Robotics/XRoboToolkit-Unity-Client/releases))

8. **[PICO İÇİNDE]** Tarayıcı sayfasının sağ üst bölümündeki indirmeleri yönetme seçeneğini açın ve `XRoboToolkit-PICO-1.1.1.apk` indirmesini açmak için tıklayın.
9. **[PICO İÇİNDE]** **Install** seçeneğini seçin. Uygulama, kitaplığınızın **Unknown** bölümünde görünür.

### XRoboToolkit'i bağlayın

1. Hem dizüstü/PC'de hem PICO'da Wi-Fi ayarlarını açın ve **aynı Wi-Fi ağında** olduklarından emin olun. Wi-Fi IPv4 adresini not edin.
   - PICO'nun Wi-Fi bilgisine ulaşmak için menünün sağ altındaki kontrol merkezini seçin.

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/pico_setup/internet.png" width="600" alt="PICO Wi-Fi ayarları">
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/pico_setup/pico_vr_screenshot.png" width="600" alt="PICO içinde XRoboToolkit ağ ekranı">
</p>

2. **XRoboToolKit** uygulamasını açın. "PC Service:" yanındaki **"Enter"** seçeneğine tıklayıp dizüstü bilgisayarın IP adresini girin. "Status:" yanında **WORKING** görünürse bağlantı doğru kurulmuştur.
   - IP adresiniz zaten girilmişse Network bölümünde "Status:" yazan yerde **"Reconnect"** seçeneğini seçin.

3. Aşağıdaki resimdeki kutuların işaretli olduğundan emin olun:
   - "Tracking" bölümünde **"Head"** ve **"Controller"**.
   - Data/Control için **"Send"** düğmesini seçin.
   - "Pico Motion Tracker" için **"Full body"** seçeneğini seçin.

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/pico_setup/xrrobot_setup.png" width="600" alt="XRoboToolkit izleme ayarları: Head, Controller, Send, Full body">
</p>

---

## Sonraki adımlar

PICO donanımınız ve yazılımınız artık hazır. Tüm gövde teleoperasyonunu simülasyonda ya da gerçek robotta çalıştırmak için [ZMQ Manager (`zmq_manager`) eğitimine](VR-TUM-GOVDE-TELEOPERASYON.md) geçin.

Unitree G1 ile XR teleoperasyonu için Türkçe kaynaklarımız: [unitree-teleoperasyon-turkce](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [Unitree G1 teleoperasyon rehberi](https://www.robotlar.org/unitree/g1-teleoperasyon)

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
