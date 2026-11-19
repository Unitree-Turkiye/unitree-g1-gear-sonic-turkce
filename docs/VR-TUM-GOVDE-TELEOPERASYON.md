> Bu belge NVlabs/GR00T-WholeBodyControl deposundaki `docs/source/tutorials/vr_wholebody_teleop.md` dosyasının Türkçe çevirisidir (Apache-2.0, commit 1e1ec4f).

# PICO VR ile Tüm Gövde Teleoperasyonu

PICO VR başlığı ve kontrolcüleriyle tam tüm gövde teleoperasyonu. Teleoperasyon için dağıtımda `--input-type zmq_manager` seçeneğini kullanın. `zmq_manager` girdi türü, bir **planlayıcı kipi** (ZMQ üzerinden yürüyüş/hareket komutları) ile bir **akışlı hareket kipi** (PICO'dan gelen tüm gövde SMPL pozları) arasında geçiş yapar.

## SONIC Low Latency

Aşağıdaki görüntü dizisi, SONIC Low Latency modelini başarılı bir yerden alma hareketi dahil tüm gövde teleoperasyon kipinde gösterir.

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/sonic_low_latency_demo.gif" width="640" alt="SONIC Low Latency ile tüm gövde teleoperasyonu ve yerden alma">
</p>

> **⚠️ Güvenlik uyarısı**
> Tüm gövde teleoperasyonu hızlı ve çevik hareketler içerir. **Her zaman** boş bir güvenlik alanı bırakın ve klavye başında acil durdurmayı tetiklemeye hazır bir güvenlik operatörü bulundurun (C++ terminalinde **`O`**, PICO kontrolcülerinde **A+B+X+Y**).
>
> Ayak izleyicilerinin görüş hattını güvenceye almak için **dar kesim pantolon ya da tayt giymeniz şarttır**. Bol ya da geniş giysiler izlemenin öngörülemez biçimde kopmasına ve tehlikeli harekete yol açabilir.

🎬 Video: [Baştan sona teleoperasyon akışı (teleop_session_overview.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop/teleop_session_overview.mp4)
*PICO kalibrasyonu, politikanın devreye alınması ve robotun kendi başına dengede durması. Ayrıntılı en iyi uygulamalar için [Teleoperasyon Rehberi](TELEOPERASYON-REHBERI.md) belgesine bakın.*

## Ön koşullar

1. **[Hızlı Başlangıç](HIZLI-BASLANGIC.md) tamamlanmış olmalı:** sim2sim döngüsünü çalıştırabiliyorsunuz ([dağıtım kurulumu](KURULUM.md) ve [model kontrol noktalarını indirme](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/getting_started/download_models.md) dahil).
2. **[VR Teleoperasyon Kurulumu](VR-TELEOPERASYON-KURULUMU.md) tamamlanmış olmalı:** `.venv_teleop` hazır, PICO donanımı eşleştirilmiş ve kalibre edilmiş.

---

## Simülasyonda adım adım teleoperasyon

Benzetilen robotu teleoperasyonla yönetmek için **üç terminal** çalıştırın.

### Terminal 1: Sanal robotu MuJoCo simülatöründe başlatın

Dağıtım ana makinesinde, **depo kökünden**:

```bash
# bash install_scripts/install_pico.sh

source .venv_teleop/bin/activate
python gear_sonic/scripts/run_sim_loop.py
```

### Terminal 2: C++ dağıtımı

[Kurulum Rehberi](KURULUM.md) içindeki, platformunuza uygun TensorRT paketiyle yerel dağıtımı çalıştırın:

```bash
cd gear_sonic_deploy
export TensorRT_ROOT=$HOME/TensorRT

source scripts/setup_env.sh
just build                                  # yalnız ilk çalıştırmada
./deploy.sh --input-type zmq_manager sim
# "Init done" mesajını görene kadar bekleyin
```

#### Ayrı ana makinelerde çalıştırma

> **Not:** Yayıncı (streamer), PICO izleme verisini alır. Dağıtım ana makinesi C++ politikasını ve Sim2Sim için MuJoCo simülatörünü çalıştırır.
>
> - Dağıtım ana makinesinde, yayıncının TCP **5556** portundan komut ve poz almak için `./deploy.sh` komutuna `--zmq-host <streamer-ip>` ekleyin.
> - Yayıncı ana makinesinde, dağıtım ana makinesinin TCP **5557** portundan robot durumu geri bildirimi almak için `pico_manager_thread_server.py` komutuna `--zmq_feedback_host <deployment-ip>` ekleyin.
>
> İki bayrak da tek ana makineli kurulum için varsayılan olarak `localhost` değerini alır. Bu ayarlar hem XRoboToolkit hem IsaacCapture için geçerlidir.
>
> Güvenlik duvarı açıksa yayıncı ana makinesinde dağıtım ana makinesinden gelen TCP **5556** bağlantısına, dağıtım ana makinesinde de yayıncı ana makinesinden gelen TCP **5557** bağlantısına izin verin.

### Terminal 3: PICO teleoperasyon yayıncısı

Yayıncı ana makinesinde, **depo kökünden**:

```bash
source .venv_teleop/bin/activate

# Tam görselleştirmeyle (ilk çalıştırma için önerilir):
python gear_sonic/scripts/pico_manager_thread_server.py --manager \
    --vis_vr3pt --vis_smpl

# Görselleştirmesiz (ekransız / robot üstü kullanım için):
# python gear_sonic/scripts/pico_manager_thread_server.py --manager
```

> **IsaacCapture (CloudXR):** IsaacCapture kullanmak için `--input-source isaac-teleop` ekleyin ve [Isaac Capture Kurulumu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/isaac_teleop_publisher_setup.md) belgesini izleyin.

Görselleştirmeyi açtığınızda, tüm eklemleri varsayılan açılarda duran bir Unitree G1 ağ modelini gösteren pencerenin açılmasını bekleyin. Varsayılan PICO yolunda pencere açılmazsa [VR Teleoperasyon Kurulumu](VR-TELEOPERASYON-KURULUMU.md) belgesindeki PICO XRoboToolKit IP yapılandırmasını yeniden denetleyin. Bunun yerine Isaac Capture kullanıyorsanız başlığın süreç içi CloudXR çalışma zamanına bağlı olduğunu doğrulayın. Bağlantı adımları için [Isaac Capture Kurulumu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/isaac_teleop_publisher_setup.md) belgesine bakın.

### İlk teleoperasyon oturumunuz

1. **Kalibrasyon pozunu alın:** dik durun, ayaklar bitişik, üst kollar iki yanınızda, ön kollar 90° öne bükülü (her dirsekte L biçimi), avuç içleri içe dönük. Ayrıntılar için [Kalibrasyon pozu](#vr_3pt-kalibrasyon-ipucu) bölümüne bakın.
2. Kontrol politikasını devreye almak ve ilk tam kalibrasyonu (`CALIB_FULL`) çalıştırmak için **A + B + X + Y** düğmelerine aynı anda basın.
3. Kollarınızı robotun geçerli pozuyla hizalayın, ardından tüm gövde SMPL teleoperasyonuna (**POSE** kipi) geçmek için **A + X** düğmelerine basın. Kollarınızı ve bacaklarınızı hareket ettirin, robot sizi izler.
4. **PLANNER** (boşta) kipine dönmek için yeniden **A + X** düğmelerine basın.
5. Robotu durdurmak için yeniden **A + B + X + Y** düğmelerine basın.

🎬 Video: [Temel tüm gövde teleoperasyon akışı (basic_flow_web.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop_calib/basic_flow_web.mp4)
*Temel tüm gövde teleoperasyon akışı: kalibrasyon pozu → devreye alma → POSE kipi → PLANNER boşta → durdurma.*

---

## PICO kontrollerinin tamamı

### Kipler ve kalibrasyon

Sistemde **4 çalışma kipi** ve **2 kalibrasyon türü** bulunur.

**Kipler:**

| Kip | Kodlayıcı | Açıklama |
|---|---|---|
| **OFF** | -- | Politika çalışmıyor. [Kalibrasyon pozunda](#vr_3pt-kalibrasyon-ipucu) durun, ardından politikayı başlatmak için **A+B+X+Y** düğmelerine basın. |
| **POSE** | SMPL | Tüm gövde teleoperasyonu: SMPL pozu PICO'dan C++ dağıtım tarafına akıtılır. Hareketiniz doğrudan robota eşlenir. |
| **PLANNER** | G1 | Yürüyüş planlayıcısı etkin, üst gövdeyi planlayıcı yönetir. Kollar (joystick), yürüme ve koşma kiplerinde yönü ve baş doğrultusunu (heading) belirler. |
| **PLANNER_FROZEN_UPPER** | G1 | Planlayıcıyla yürüyüş; üst gövde son POSE anlık görüntüsünde dondurulur. |
| **VR_3PT** | TELEOP | Planlayıcıyla yürüyüş; üst gövde VR 3 nokta izlemesini (baş + 2 el) izler. Ters kinematik (IK) tabanlı olmayan VR 3 nokta kalibrasyonuna dayanır. |

**Kalibrasyon türleri** (en düşük gecikme için IK tabanlı olmayan iş akışı):

| Tür | Neyi kalibre eder | Ne zaman tetiklenir |
|---|---|---|
| **CALIB_FULL** | Baş + iki bilek, **tümüyle sıfır** referans poza göre. | İlk **A+B+X+Y** basışında bir kez (başlangıç). |
| **CALIB** | Yalnız iki bilek, **robotun geçerli pozuna** göre. | **Sol kol tıklamasıyla (Left Stick Click)** VR_3PT kipine her geçişte. |

### Durum makinesi

4 kip ve 2 kontrol zinciri bulunur. Her zincir bir üçgen oluşturur: **A+X** (ya da **B+Y**), hem planlayıcı düğümünden hem de onun VR_3PT alt kipinden POSE kipine döndürür.

```text
  ┌──────────────────────────────────────┐
  │  A+B+X+Y (any mode) ──► OFF          │
  └──────────────────────────────────────┘

  Startup:
    OFF ──(A+B+X+Y)──► PLANNER ──(A+X)──► POSE
          CALIB_FULL

  Chain 1 - G1 encoder listens to planner-generated full-body motion: PLANNER

    PLANNER ─── L-Stick (+CALIB) ──► VR_3PT
     ▲    │ ◄─────── L-Stick ─────────  │
     │    │                             │
     │A+X │A+X                    A+X   │
     │    ▼                             ▼
     └─ POSE ◄──────────────────────────┘

  Chain 2 - G1 encoder listens to planner-generated lower-body motion: PLANNER_FROZEN_UPPER

    PLANNER_FROZEN_UPPER ── L-Stick (+CALIB) ──► VR_3PT
         ▲     │ ◄──────── L-Stick ──────────     │
         │     │                                  │
         │B+Y  │B+Y                          B+Y  │
         │     ▼                                  ▼
         └── POSE ◄───────────────────────────────┘
```

*Şemanın Türkçesi: herhangi bir kipte A+B+X+Y, OFF kipine götürür. Başlangıçta OFF, A+B+X+Y ile (CALIB_FULL) PLANNER kipine, oradan A+X ile POSE kipine geçer. 1. zincirde G1 kodlayıcısı planlayıcının ürettiği tüm gövde hareketini dinler (PLANNER). 2. zincirde G1 kodlayıcısı planlayıcının ürettiği alt gövde hareketini dinler (PLANNER_FROZEN_UPPER). Sol kol tıklaması (+CALIB) VR_3PT kipine götürür ve geri getirir.*

> **⚠️ TEHLİKE: Kip geçişi güvenliği**
> **POSE ya da VR_3PT kipine geçmeden önce** vücudunuzu her zaman önce robotun geçerli pozuyla hizalayın!!
>
> - **POSE:** Robot anında sizin fiziksel pozunuza oturur. Büyük bir uyumsuzluk ani, sert harekete yol açar.
> - **VR_3PT:** Hizasız bir kalibrasyon, kollarınızı oynattığınız anda düzensiz ve tehlikeli hareket üretir. Ayrıntı için [Her geçişte CALIB](#her-geçişte-calib-yalnız-bilekler) bölümüne bakın.

### VR_3PT kalibrasyon ipucu

`VR_3PT` kipi doğru kalibrasyona dayanır. İki kalibrasyon olayı yaşanır:

#### Tek seferlik `CALIB_FULL` (baş ve bilekler)

**A + B + X + Y** düğmelerine ilk kez basmadan önce robotun **tümüyle sıfır referans pozunda** durmanız **şarttır**. Sistem, PICO gövde izleme çerçevenizi sonraki tüm hareket eşlemeleri için sıfır referansı olarak kaydeder.

**Referans poz:**
1. **Dik durun**, ayaklar bitişik, dümdüz ileri bakın.
2. **Üst kollar** gövdenize yakın, dümdüz aşağı sarkar.
3. **Ön kollar** 90° öne bükülü (her dirsekte L biçimi), avuç içleri içe dönük.

> **İpucu:** Robotun referans pozunu bir görselleştirme penceresinde görmek için teleoperasyon betiğini `--vis_vr3pt` ile başlatın. Başlatma düğme bileşimine basmadan önce vücudunuzu bu poza uydurun.

#### Her geçişte `CALIB` (yalnız bilekler)

**Sol kol tıklamasıyla** `VR_3PT` kipine her girişinizde sistem iki bileği robotun **geçerli** pozuna göre yeniden kalibre eder. Tıklamadan önce kollarınızı her zaman robotla hizalayın.

Aşağıda **kötü kalibrasyon uygulamasına** bir örnek var: kollarınızı robotun geçerli pozuyla hizalamadan VR_3PT kipine geçmek. Robot hemen sıçramayabilir ama siz hareket ettiğiniz anda düzensiz ve tehlikeli hareket sergiler.

🎬 Video: [Kötü uygulama: hizalamadan VR_3PT kipine geçmek (BAD_Calib_VR3PT_web.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop_calib/BAD_Calib_VR3PT_web.mp4)
*Kötü uygulama: kolları hizalamadan VR_3PT kipine girmek düzensiz, güvensiz harekete yol açar.*

> **⚠️ TEHLİKE: Kip geçişi güvenliği**
> **POSE ya da VR_3PT kipine geçmeden önce** vücudunuzu her zaman önce robotun geçerli pozuyla hizalayın.
>
> **Kötü VR_3PT kalibrasyonundan kurtulma:**
> 1. Üst gövdeyi dondurun: **Sol kol tıklamasıyla** geri geçin.
> 2. Kollarınızı robotun geçerli (bozulmuş olabilecek) pozuyla yeniden hizalayın.
> 3. Sıfırlamak için **POSE** kipine (**A+X**) geçin.

Aşağıda **kurtarma yöntemi** gösterilir: yanlışlıkla kötü kalibre edilmiş bir VR_3PT durumuna girerseniz üst gövdeyi dondurun (Sol kol tıklamasıyla geri), ardından güvenle sıfırlamak için POSE kipine (A+X) geçin.

🎬 Video: [Kötü VR_3PT kalibrasyonundan kurtarma (BAD_Calib_VR3PT_recover_web.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/teleop_calib/BAD_Calib_VR3PT_recover_web.mp4)
*Kötü VR_3PT kalibrasyonundan kurtarma: üst gövdeyi dondur → yeniden hizala → POSE kipine geç.*

### Hızlı başlangıç özeti

| Eylem | Düğme | Notlar |
|---|---|---|
| **Politikayı başlat / durdur** | **A+B+X+Y** | İlk basış: devreye alma + CALIB_FULL. Yeniden basış: acil durdurma → OFF. |
| **POSE aç/kapat** | **A+X** | PLANNER ↔ POSE arasında geçiş yapar. Ya da VR_3PT kipinden (PLANNER üzerinden girildiyse) → POSE. |
| **PLANNER_FROZEN_UPPER aç/kapat** | **B+Y** | POSE ↔ PLANNER_FROZEN_UPPER arasında geçiş yapar. Ya da VR_3PT kipinden (PLANNER_FROZEN_UPPER üzerinden girildiyse) → POSE. |
| **VR_3PT aç/kapat** | **Sol kol tıklaması** | Herhangi bir Planner kipinden → VR_3PT (CALIB tetikler). Geri dönmek için yeniden tıklayın. |
| **El kavrama** | **Tetik** (her el için) | İlgili elin kavramasını yönetir. |

### Kol (joystick) kontrolleri (Planner kipleri)

**PLANNER**, **PLANNER_FROZEN_UPPER** ve **VR_3PT** kiplerinde etkindir:

| Girdi | İşlev |
|---|---|
| **Sol kol** | Hareket yönü (ileri / geri / yana kayma) |
| **Sağ kol (yatay)** | Sapma (yaw) / baş doğrultusu (sürekli birikir) |
| **A + B** | Sonraki yürüyüş kipi |
| **X + Y** | Önceki yürüyüş kipi |

**Yürüyüş kipleri** (A+B / X+Y ile sırayla geçilir):

| ID | Kip |
|---|---|
| 0 | Boşta (Idle) [VARSAYILAN] |
| 1 | Yavaş yürüme (Slow Walk) |
| 2 | Yürüme (Walk) |
| 3 | Koşma (Run) |
| 4 | Çömelme (Squat) |
| 5 | Diz çökme, iki bacak (Kneel, two legs) |
| 6 | Diz çökme (Kneel) |
| 7 | Yüzüstü yatma (Lying face-down) |
| 8 | Emekleme (Crawling) |
| 9-16 | Boks türleri (boşta, yürüme, yumruklar, kroşeler) |
| 17 | İleri zıplama (Forward Jump) |
| 18 | Gizlice yürüme (Stealth Walk) |
| 19 | Yaralı yürüyüş (Injured Walk) |

---

## Acil durdurma

| Yöntem | Eylem |
|---|---|
| **PICO kontrolcüleri** | **A+B+X+Y** düğmelerine aynı anda basın → OFF |
| **Klavye** (C++ terminali) | Anında durdurma için **`O`** tuşuna basın |

---

## Gerçek robotta adım adım teleoperasyon

> **⚠️ Güvenlik uyarısı**
> Ancak robotu simülasyonda akıcı biçimde yönetebiliyor ve acil durdurmaları rahatça kullanabiliyorsanız devam edin. **Önce çalışan her `run_sim_loop.py` sürecini sonlandırın.** Aynı anda çalışan simülasyon ve gerçek örnekler birbiriyle çakışır.

Gerçek robot iş akışı **iki terminal** kullanır (MuJoCo simülatörü yok).

### Terminal 1: C++ dağıtımı (gerçek robot)

[Kurulum Rehberi](KURULUM.md) içindeki, platformunuza uygun TensorRT paketiyle yerel dağıtımı çalıştırın:

```bash
cd gear_sonic_deploy
export TensorRT_ROOT=$HOME/TensorRT

source scripts/setup_env.sh
just build                                  # yalnız ilk çalıştırmada

# 'real' robot ağ arayüzünü (192.168.123.x) otomatik algılar.
# Otomatik algılama arayüzü bulamazsa G1'in IP adresini doğrudan verin:
#   ./deploy.sh --input-type zmq_manager <G1-IP>
./deploy.sh --input-type zmq_manager real
# "Init done" mesajını görene kadar bekleyin
```

> **Not:** Dağıtım ve yayıncı farklı makinelerde çalışıyorsa iki ZMQ bağlantısını yapılandırmak için [Ayrı ana makinelerde çalıştırma](#ayrı-ana-makinelerde-çalıştırma) bölümünü izleyin.

### Terminal 2: PICO teleoperasyon yayıncısı

Yayıncı ana makinesinde, **depo kökünden**:

```bash
# bash install_scripts/install_pico.sh

source .venv_teleop/bin/activate
python gear_sonic/scripts/pico_manager_thread_server.py --manager

# Ekranlı bir harici makinede çalışıyorsanız görselleştirme ekleyin:
#   --vis_vr3pt --vis_smpl
```

İstendiğinde XRoboToolkit başlık uygulamasında PC hizmetini seçin.

> **IsaacCapture (CloudXR):** IsaacCapture kullanmak için `--input-source isaac-teleop` ekleyin ve [Isaac Capture Kurulumu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/isaac_teleop_publisher_setup.md) belgesini izleyin.

Aynı başlatma sırasını izleyin: kalibrasyon pozu → **A+B+X+Y** → POSE kipi için **A+X**. Kullanılabilir tüm komutlar için [PICO kontrollerinin tamamı](#pico-kontrollerinin-tamamı) bölümüne bakın.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · [Unitree G1 teleoperasyon rehberi](https://www.robotlar.org/unitree/g1-teleoperasyon) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
