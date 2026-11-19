> Bu belge NVlabs/GR00T-WholeBodyControl deposundaki `docs/source/user_guide/teleoperation.md` dosyasının Türkçe çevirisidir (Apache-2.0, commit 1e1ec4f).

# Tüm Gövde Teleoperasyon Rehberi

Bu rehber, tüm gövde teleoperasyonu sırasındaki en iyi uygulamaları anlatır.

> **Ön koşullar**
> [Hızlı Başlangıç](HIZLI-BASLANGIC.md), [PICO Kurulumu](VR-TELEOPERASYON-KURULUMU.md) ve [Teleoperasyon Kurulumu](VR-TUM-GOVDE-TELEOPERASYON.md) adımlarını tamamlayın.

## Genel bakış

Tüm gövde teleoperasyonunu doğru kurmak çok zordur ve işin birçok hareketli parçası vardır. Bu rehber, tüm gövde teleoperasyonu sırasında gereken ayrıntıları adım adım anlatır. Tüm gövde teleoperasyonunda GEAR-SONIC politikası, ayak hareketleriniz dahil hareketinizi **olabildiğince** kopyalamaya çalışır! Bu yüzden gerekli önlemler alınmazsa iş oldukça **zorlayıcı ve tehlikeli** olabilir.

> **⚠️ Güvenlik uyarısı**
> Robot, tüm gövde hareketlerinizi gerçek zamanlı izler. Robotun çevresinde her zaman 3 metrelik boş bir güvenlik alanı bırakın, acil durdurma için **`O`** tuşuna basmaya hazır bir güvenlik operatörünü klavye başında tutun ve PICO kontrolcüsünden kendiniz de acil durdurma yapmaya her an hazır olun. Gerçek donanımda denemeden önce simülasyonda bolca alıştırma yapın!

## Örnek teleoperasyon oturumu

Tipik bir tüm gövde teleoperasyon oturumu şu iş akışını izler:

🎬 Video: [Tam başlatma sırası (teleop_session_overview.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop/teleop_session_overview.mp4)
*Video: Tam başlatma sırası. PICO başlığının kalibrasyonu, politikanın devreye alınması ve robotun askı iskelesinde (gantry) kendi başına dengede durmaya başlaması.*

**Terminal 1: MuJoCo simülatörü** (gerçek robot için atlayın):
```bash
source .venv_teleop/bin/activate
python gear_sonic/scripts/run_sim_loop.py
```

**Terminal 2: C++ dağıtımı**:
```bash
cd gear_sonic_deploy
# Simülasyon için:
bash deploy.sh --input-type zmq_manager sim
# Gerçek robot için:
# bash deploy.sh --input-type zmq_manager real
```

**Terminal 3: PICO teleoperasyon yayıncısı**:
```bash
source .venv_teleop/bin/activate
python gear_sonic/scripts/pico_manager_thread_server.py --manager
```

Yukarıdaki komut XRoboToolkit kullanır. IsaacCapture kullanmak için `--input-source isaac-teleop` ekleyin ve [Isaac Capture kurulum yönergelerini](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/isaac_teleop_publisher_setup.md) izleyin.

**Operatör adımları**:
1. **PICO başlığı ve kontrolcüleri takın:** ayak izleyicilerinin sıkıca bağlı olduğundan emin olun.
2. **Kalibrasyon pozunda durun:** dik, ayaklar bitişik, kollar aşağıda. Sık sık yeniden kalibre edin!!!
3. **Robotu gevşek ama ayakta durur halde tutun:** G1'i askı iskelesinde biraz gevşek bırakın (politika başlar ve robot kendi başına dengede durmaya başlar).
4. Kontrolcülerde **A+B+X+Y** düğmelerine basın: politikayı başlatır ve kalibre eder (Planner kipine girer).
5. **A+X** düğmelerine basın: Pose kipine geçer (tüm gövde teleoperasyonu etkin).
6. **Teleoperasyon yapın:** hareketleriniz artık robota yansır.
7. İşiniz bitince **A+B+X+Y** düğmelerine basın: acil durdurma ve çıkış. Politika durur!!!

## Kıyafet gereksinimleri

**Çok önemli:** Teleoperasyon sırasında **dar kesim pantolon ya da tayt** giymeniz **şarttır**.

**Neden önemli:**
- PICO ayak izleyicileri görsel izleme kullanır ve bacak/ayak hareketlerinizi açıkça görmeleri gerekir
- Bol, geniş ya da dalgalanan giysiler (eşofman altı, geniş paçalı pantolon, uzun etek) izleyicilerin önünü kapatır
- Kısa bir izleme kaybı bile robota yanlış ayak konumları gönderir. Bu da tökezlemeye ya da tehlikeli hareketlere yol açar

**Önerilenler:**
- ✅ Spor tayt ya da sıkı tayt (kompresyon)
- ✅ Dar kesim kot ya da slim fit pantolon

**Önerilmeyenler:**
- ❌ Bol eşofman altı ya da kargo pantolon
- ❌ Geniş ya da İspanyol paça pantolon
- ❌ Uzun elbise ya da etek
- ❌ Bacak çevresinde bol ya da dalgalanan her tür kumaş

**Üst gövde:** Olağan, vücuda oturan giysiler uygundur. Kontrolcü izlemesine engel olabilecek çok bol kollardan kaçının.

**❗️❗️❗️Sık sık yeniden kalibre edin.** PICO'nun izleme kalitesi zamanla düşebilir. Performans düştüğünde her zaman yeniden kalibre edin! Ayrıca PICO kontrolcüsü izlemeyi kaybettiğinde oturur ya da tuhaf bir pozda takılı kalabilir. Teleoperasyon politikasını başlatmadan önce izlenen hareketinizin doğru olduğundan (PICO avatarına bakarak) her zaman emin olun!

## Wi-Fi gecikmeleri

🎬 Video: [Doğal ve tereddütlü yürümenin dengeye etkisi (teleop_natural_movement.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop/teleop_natural_movement.mp4)
*Video: Doğal ve tereddütlü yürümenin robot kararlılığını nasıl etkilediği. Wi-Fi gecikmeleri ya da doğal olmayan hareketin yol açtığı tökezleme.*

**Ağ gecikmesi olabildiğince düşük tutulmalıdır.** Gecikmeleri tespit etmek için araçlar sunuyoruz. Ağ gecikmeleri GEAR-SONIC politikasının performansını ciddi biçimde etkileyebilir. Çünkü robotun hareket akışı hareketin ortasında (örneğin yürürken adımın ortasında) kesilir ve robot tökezleyebilir ya da dengesini kaybedebilir.

**En iyi uygulamalar:**
- **Özel Wi-Fi yönlendiricileri kullanın.** Herkese açık ve okul Wi-Fi ağlarında kolayca büyük gecikmeler oluşabilir.
- **Wi-Fi atlamalarını en aza indirin:** PICO ve dağıtım makinesi ideal olarak aynı yerel ağda olmalıdır.
- **Gecikmeyi denetleyin:** terminal çıktısındaki ZMQ mesaj gecikmelerini izleyin.

**Beklenen gecikme:**
- **İyi:** < 10ms (kablolu ya da güçlü yerel Wi-Fi)
- **Kabul edilebilir:** 10-30ms (hafif bir gecikme fark edilebilir)
- **Zayıf:** > 30ms (robot akıcı hareketleri izlemekte zorlanır, tökezleme riski artar)

**Ağ performansını denetleme:**
Mesaj gecikmeleri eşikleri aşarsa dağıtım terminali uyarı gösterir. Şuna benzer mesajları izleyin:
```
WARNING: High ZMQ latency detected: 45ms
```

Kalıcı yüksek gecikme uyarıları görürseniz devam etmeden önce ağ kurulumunuzu iyileştirin.

## Hareket biçimleri

🎬 Video: [Yürüme demosu (teleop_walking.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop/teleop_walking.mp4)
*Video: Yürüme demosu. Doğal yürüyüşle ileri, geri, koşarak ve yana hareket.*

**Olabildiğince doğal olmaya çalışın.** GEAR-SONIC politikası doğal insan hareketiyle eğitildi, bu yüzden doğal hareket etmek en iyi sonucu verir. Hareket ederken tereddüt etmek aslında daha çok tökezlemeye yol açar!

**İyi hareket uygulamaları:**

1. **Doğal yürüyün:** kolların doğal salınımıyla olağan yürüyüşünüzü kullanın. Abartmayın ve robota "yardım etmeye" çalışmayın.

2. **Ağırlığınızı yumuşakça aktarın:** adım atarken ağırlığınızı bir ayaktan ötekine her zamanki gibi aktarın. Kendinizden emin olun!

3. **Orta hızlar kullanın:** robot hızlı hareketleri izleyebilir, ama rahat bir tempoda yürüyerek başlayın. Alıştıktan sonra hızlanabilir ya da koşmaya başlayabilirsiniz.

**Sık yapılan hatalar:**

- ❌ **Aşırı yavaş hareketler:** fazla yavaşlamayın!
- ❌ **Robotun hareketine uymaya çalışmak:** robotun hareketine uymaya çalışmayın. Yoksa robot da sizin hareketinize uymaya çalışır ve tökezlemeye başlar. Doğal olun!

**İleri düzey hareketler:**

🎬 Video: [İleri düzey hareketler (teleop_advanced.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop/teleop_advanced.mp4)
*Video: İleri düzey hareketler. Diz çökme, dinamik bacak hareketleri ve zorlu pozlar.*

Temel yürümeye alıştıktan sonra:
- **Dönme:** gövdenizi döndürerek ve yeni yöne adım atarak doğal biçimde dönün
- **Uzanma:** nesneleri tutmak için kollarınızı yumuşakça uzatın
- **Çömelme:** dizlerinizi bükün ve vücudunuzu doğal biçimde indirin
- **Yana adım:** doğal ağırlık aktarımıyla yana adım atın

## Kalibrasyonda en iyi uygulamalar

İlk kalibrasyon, başarılı teleoperasyon için **çok önemlidir**.

**Kalibrasyon pozu:**
1. **Dik durun:** dümdüz ileri bakın (aşağıdaki kontrolcülere değil)
2. **Ayaklar bitişik:** ayaklar paralel, arada boşluk yok
3. **Üst kollar aşağıda:** gövdenizin iki yanında dümdüz sarkar

**İpuçları:**
- A+B+X+Y düğmelerine bastıktan sonra pozu 1-2 saniye sabit tutun
- Robot oturum boyunca kaymış görünüyorsa yeniden kalibre edin (A+B+X+Y ile durdurun, sonra yeniden başlatın)

## Kip geçişi güvenliği

Kipler arasında geçerken **her zaman önce robotun geçerli pozuna uyun**.

**Tehlikeli senaryo:**
1. Robot Planner kipinde dik duruyor
2. Siz çömelmiş ya da başka bir pozda uzanmış haldesiniz
3. Pose kipine geçmek için **A+X** düğmelerine basıyorsunuz
4. **Robot sizin çömelmiş pozunuza sertçe uymaya çalışır** ⚠️

**Güvenli yöntem:**
1. **A+X** düğmelerine basmadan önce robota (ya da görselleştirmeye) bakın
2. Vücudunuzu robotun geçerli pozuna kabaca uyacak biçimde hareket ettirin
3. Sonra **A+X** düğmelerine basın, geçiş yumuşak olur

**Duraklatma özelliği (Menu düğmesi):**

🎬 Video: [Menu düğmesiyle duraklatma (teleop_pause.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop/teleop_pause.mp4)
*Video: Teleoperasyon sırasında poz akışını duraklatmak ve sürdürmek için Menu düğmesinin kullanımı.*

- **Menu** düğmesini basılı tutmak poz akışını duraklatır
- **Menu düğmesini bırakmadan önce** vücudunuzu robotun geçerli pozuna uyacak biçimde geri getirin
- Çok farklı bir pozdayken Menu düğmesini bırakmak ani, tehlikeli hareketlere yol açar

## Sorun giderme

### Robot hareketlerimi izlemiyor

**Olası nedenler:**
- Ayak izleyicileri sıkıca bağlı değil ya da pilleri zayıf
- Bol giysiler izleyicilerin önünü kapatıyor
- Işık koşulları zayıf
- XRoboToolKit PICO'da açık değil ya da yanlış yapılandırılmış

**Çözümler:**
1. Ayak izleyicilerinin yerleşimini ve pil seviyesini denetleyin
2. Dar kesim pantolon giydiğinizi doğrulayın
3. Işığı iyileştirin (çok parlak ya da çok karanlık alanlardan kaçının)
4. PICO başlığında XRoboToolKit uygulamasını yeniden başlatın
5. Yeniden kalibre edin

IsaacCapture için Web istemcisinin yayıncı IP adresine bağlı olduğunu doğrulayın.

### Robot ani, sert hareketler yapıyor

**Olası nedenler:**
- Pozlar uyuşmazken kip değiştirildi
- İzleme aksaklığı ya da ayak izleyicisinin önünün kapanması
- Ağ paket kaybı yüzünden karelerin gecikmesi

**Çözümler:**
1. Sürdürmeden önce dikkatle yeniden kalibre edin
2. Kip değiştirmeden önce her zaman robotun pozuna uyun
3. Ağ gecikmesini denetleyin ve Wi-Fi/kablolu bağlantıyı iyileştirin

### İzleme titrek ya da robot tökezliyor

**Olası nedenler:**
- Kablosuz gecikmeler
- IMU kayması
- Eklem kodlayıcı (encoder) kayması

**Çözümler:**
1. Wi-Fi girişimini azaltın (öteki kablosuz cihazlardan uzaklaşın)
2. Robotu yeniden kalibre edin

## Acil durum yöntemleri

### Acil durdurma yöntemleri

**Klavye (dağıtım terminali):**
- Anında durdurma için **`O`** tuşuna basın

**PICO kontrolcüleri:**
- **A + B + X + Y** düğmelerine aynı anda basın

İki yöntem de politikayı hemen durdurur ve kontrol kipinden çıkar.

## Sonraki adımlar

- **Girdi arayüzlerini öğrenin:** [Klavye](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/keyboard.md), [Oyun kumandası](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/gamepad.md), [ZMQ](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/zmq.md), [Manager](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/manager.md) eğitimlerine bakın
- **Dağıtımı öğrenin:** [Dağıtım Kodu ve Program Akışı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/deployment_code.md) belgesine bakın
- **Genel sorun giderme:** [Sorun Giderme Rehberi](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/user_guide/troubleshooting.md) belgesine bakın

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · [Unitree G1 teleoperasyon rehberi](https://www.robotlar.org/unitree/g1-teleoperasyon) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
