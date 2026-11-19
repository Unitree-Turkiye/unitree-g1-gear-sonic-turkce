<div align="center">

# NVIDIA GEAR SONIC Türkçe: Unitree G1 İnsansı Robot için Tüm Gövde Kontrolü ve VR Teleoperasyon

**GR00T Whole-Body Control (WBC): GEAR-SONIC davranış temel modeli · Decoupled WBC · MotionBricks önizlemesi**

**SONIC: Supersizing Motion Tracking for Natural Humanoid Whole-Body Control**

**Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org · [www.robotlar.org](https://www.robotlar.org)**

👉 **Unitree G1 ürün sayfası: [robotlar.org/tr/insansi-robotlar/unitree-g1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · G1 EDU için teklif: [robotlar.org/tr/fiyat/unitree-g1-edu-fiyat](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat)**

📖 **Türkçe rehber: [GEAR SONIC ile Unitree G1 tüm gövde kontrol rehberi](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi)** · 🥽 **[Unitree G1 teleoperasyon rehberi](https://www.robotlar.org/unitree/g1-teleoperasyon)**

[Model Kartı](#-model-kartı) · [VR Teleoperasyon](#-vr-ile-tüm-gövde-teleoperasyonu) · [Kurulum](#kurulum) · [Belgeler](#-belgeler) · [G1 ile Başlangıç](#-unitree-g1-ile-gear-sonic-türkiyede-nereden-başlanır) · [Unitree Ürünleri](#-unitree-ürün-ailesi-türkiye)

</div>

<div align="center">
  <img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/groot_wbc.png" width="800" alt="GEAR SONIC ve GR00T Whole-Body Control başlık görseli">
</div>

<div align="center">

[![License](https://img.shields.io/badge/License-Apache%202.0-76B900.svg)](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/LICENSE)
[![IsaacLab](https://img.shields.io/badge/IsaacLab-2.3.2-orange.svg)](https://github.com/isaac-sim/IsaacLab/releases/tag/v2.3.2)
[![Documentation](https://img.shields.io/badge/docs-GitHub%20Pages-76B900.svg)](https://nvlabs.github.io/GR00T-WholeBodyControl/)
[![Demo](https://img.shields.io/badge/Live%20Demo-GEAR--SONIC-blue.svg)](https://nvlabs.github.io/GEAR-SONIC/demo.html)

</div>

> Bu depo, NVIDIA'nın (NVlabs) resmi [NVlabs/GR00T-WholeBodyControl](https://github.com/NVlabs/GR00T-WholeBodyControl) projesinin README'sinin ve seçili belgelerinin Türkçe çevirisidir. Çeviri, `main` dalının `1e1ec4f` commit'ine (2 Kasım 2026) dayanır. Kaynak kod, model ağırlıkları, ağ (mesh) dosyaları ve görseller orijinal depoda kalır. Burada yalnız Türkçe anlatım bulunur ve görseller orijinal depodan gösterilir.
>
> **Lisans dağılımı:** kaynak kod, betikler ve yazılım bileşenleri Apache-2.0, eğitilmiş model kontrol noktaları ve ağırlıkları NVIDIA Open Model License kapsamındadır. Bu çeviri Apache-2.0 ile, atıf yapılarak yayımlanır. Ayrıntı [Lisans](#-lisans) bölümünde.
>
> **Kodu çalıştırmak için orijinal depoyu klonlayın:** [github.com/NVlabs/GR00T-WholeBodyControl](https://github.com/NVlabs/GR00T-WholeBodyControl)
>
> GEAR SONIC, NVIDIA'nın projesidir. Çeviri ve Türkçe teknik destek: Unitree Türkiye resmi distribütörü [Robotlar.org](https://www.robotlar.org).
>
> **Bu depo ile Isaac GR00T Türkçe deposunun farkı:** [unitree-g1-isaac-groot-turkce](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) deposu ve [Unitree G1 Isaac GR00T rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-isaac-gr00t-rehberi), G1 üzerinde Isaac GR00T N1.7 ile veri toplamadan gerçek robota dağıtıma uzanan VLA akışını anlatır. Bu depo o akışın altındaki tüm gövde kontrol katmanına odaklanır: SONIC politikası, C++ dağıtım yığını, PICO VR ile tüm gövde teleoperasyonu ve G1 üzerinde sim2sim ile gerçek robot adımları.

## GR00T-WholeBodyControl

Bu, **GR00T Whole-Body Control (WBC)** projelerinin kod tabanıdır. İnsansı robotlar için gelişmiş tüm gövde kontrolcülerini eğitmek, değerlendirmek ve dağıtmak için model kontrol noktalarını (checkpoints) ve betikleri barındırır. Şu an desteklenenler:

- **Decoupled WBC:** NVIDIA GR00T [N1.5](https://research.nvidia.com/labs/gear/gr00t-n1_5/) ve [N1.6](https://research.nvidia.com/labs/gear/gr00t-n1_6/) modellerinde kullanılan ayrık kontrolcü (alt gövde için pekiştirmeli öğrenme, üst gövde için ters kinematik).
- **GEAR-SONIC serisi:** genel amaçlı insansı tüm gövde kontrolcülerimizin en yeni sürümü ([teknik makaleye](https://nvlabs.github.io/GEAR-SONIC/) bakın).
- **MotionBricks:** animasyon ve robotikte etkileşimli hareket kontrolü için gerçek zamanlı bir gizil üretken model ([proje sayfasına](https://nvlabs.github.io/motionbricks/) bakın).

## 📰 Haberler

- **[2026-09-14]** **IsaacTeleop 1.4 ve AGX Orin desteği:** PICO teleoperasyonu için IsaacTeleop 1.4 ve Jetson AGX Orin desteği eklendi.
- **[2026-08-31]** **SONIC dağıtım güncellemesi:** tökezlemeyi azaltmak için motor başına Kp/Kd ölçekleme eklendi.
- **[2026-07-23]** **SONIC v1.1 kontrol noktası:** tüm gövde teleoperasyonu ve SONIC destekli VLA yürütmesi için bilek pozu çoğaltmasıyla (wrist-pose augmentation) eğitilmiş, robot yönelimine göre normalleştirilmiş bir SONIC kontrolcüsü yayımlandı. [Model Kartı](#-model-kartı) ve [Model İndirme](https://nvlabs.github.io/GR00T-WholeBodyControl/getting_started/download_models.html#sonic-v11-checkpoint) sayfasına bakın.
- **[2026-06-16]** **Isaac Teleop Kurulumu (CloudXR / DeviceIO, süreç içi):** ayrı bir yayıncı (publisher) konteyneri gerektirmeyen, `isaacteleop[cloudxr]` üzerinden süreç içi CloudXR yolu için devreye alma belgeleri eklendi. [Isaac Teleop Kurulumu](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/isaac_teleop_publisher_setup.html) sayfasına bakın.
- **[2026-06-16]** **Düşük gecikmeli teleoperasyon kontrol noktası:** daha çabuk tepki veren tüm gövde teleoperasyonu için 4 karelik SMPL referans önden bakışı (lookahead) olan bir SONIC kontrol noktası yayımlandı. [Model Kartı](#-model-kartı), [Model İndirme](https://nvlabs.github.io/GR00T-WholeBodyControl/getting_started/download_models.html#low-latency-teleoperation-checkpoint) ve [VLA Çıkarımı](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/vla_inference.html#low-latency-teleoperation-checkpoint) sayfalarına bakın.
- **[2026-05-07]** 🤖 **G1 üzerinde baştan sona VLA iş akışı:** teleoperasyon verisi toplayın, Isaac-GR00T N1.7'ye ince ayar yapın ve SONIC tüm gövde kontrolüyle dağıtın. [Veri Toplama](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/data_collection.html), [VLA İş Akışı](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/vla_workflow.html) ve [VLA Çıkarımı](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/vla_inference.html) sayfalarına bakın.
- **[2026-04-27]** 🧩 **MotionBricks önizlemesi:** etkileşimli G1 demosu, önceden eğitilmiş kontrol noktaları (VQVAE · poz · kök), sentetik eğitim kodu ve hareket temsili belgeleri. [`motionbricks/`](https://github.com/NVlabs/GR00T-WholeBodyControl/tree/main/motionbricks) klasörüne ve [proje sayfasına](https://nvlabs.github.io/motionbricks/) bakın.
- **[2026-04-14]** 🌐 **[Canlı web demosu](https://nvlabs.github.io/GEAR-SONIC/demo.html):** SONIC'i tarayıcınızda etkileşimli olarak deneyin. Demoda [Kimodo](https://github.com/nv-tlabs/kimodo) ile metinden harekete üretim de bulunur.
- **[2026-04-10]** 🚀 **SONIC eğitim kodu ve kontrol noktası** [Hugging Face](https://huggingface.co/nvidia/GEAR-SONIC) üzerinde yayımlandı. Sıfırdan eğitin ya da ince ayar yapın. **Ek gövde (embodiment) desteği** ve **VLA veri toplama hattı** da geldi. [Eğitim Rehberi](https://nvlabs.github.io/GR00T-WholeBodyControl/user_guide/training.html) sayfasına bakın.
- **[2026-03-24]** 🔧 C++ çıkarım yığını güncellemesi: motor hata izleme, TTS uyarıları, ZMQ protokol v4, boşta kip yeniden uyarlaması. **ZMQ başlık boyutu 1280 bayta çıktı.**
- **[2026-03-16]** 📦 [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) açık kaynak oldu: G1 MuJoCo yörüngeleriyle birlikte 142 binden fazla insan hareketi (yaklaşık 288 saat).
- **[2026-02-19]** 🎉 GEAR-SONIC yayımlandı: önceden eğitilmiş kontrol noktaları, C++ çıkarımı, VR teleoperasyonu ve belgeler.
- **[2025-11-12]** 🏁 GR00T N1.5 ve N1.6 için Decoupled WBC ile ilk sürüm.

## 📑 İçindekiler

- [Haberler](#-haberler)
- [GEAR-SONIC](#-gear-sonic)
- [Model Kartı](#-model-kartı)
- [VR ile Tüm Gövde Teleoperasyonu](#-vr-ile-tüm-gövde-teleoperasyonu)
- [Kinematik Planlayıcı](#-kinematik-planlayıcı)
- [SONIC Eğitimi](#-sonic-eğitimi)
- [Yapılacaklar](#-yapılacaklar)
- [Depoda Neler Var](#-depoda-neler-var)
  - [Kurulum](#kurulum)
- [Belgeler](#-belgeler)
- [Atıf](#-atıf)
- [Lisans](#-lisans)
- [Destek](#-destek)
- [MotionBricks](#-motionbricks)
- [Decoupled WBC](#-decoupled-wbc)

## 🤖 GEAR-SONIC

<p style="font-size: 1.2em;">
    <a href="https://nvlabs.github.io/GEAR-SONIC/"><strong>Web sitesi</strong></a> |
    <a href="https://huggingface.co/nvidia/GEAR-SONIC"><strong>Model</strong></a> |
    <a href="https://arxiv.org/abs/2511.07820"><strong>Makale</strong></a> |
    <a href="https://nvlabs.github.io/GR00T-WholeBodyControl/"><strong>Belgeler</strong></a>
</p>

<div align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/sonic-preview-gif-480P.gif" width="800" alt="GEAR SONIC ile Unitree G1 insansı robotun tüm gövde hareketleri">
</div>

SONIC, robotlara büyük ölçekli insan hareketi verisinden öğrenilmiş temel bir motor beceri seti kazandıran insansı davranış temel modelidir (humanoid behavior foundation model). Önceden tanımlı hareketler için ayrı kontrolcüler kurmak yerine hareket takibini (motion tracking) ölçeklenebilir bir eğitim görevi olarak kullanır. Böylece tek, birleşik bir politika doğal tüm gövde hareketi üretir ve yürümeden emeklemeye, teleoperasyondan çok kipli (multi-modal) kontrole kadar geniş bir davranış yelpazesini destekler. Eğitimde gördüğü hareketlerin ötesine genellemek ve üst düzey planlama ile etkileşime temel olmak üzere tasarlandı.

Bu depoda SONIC'in eğitim kodunu, dağıtım çatısını, model kontrol noktalarını ve veri toplama için teleoperasyon yığınını yayımlıyoruz.

## 📦 Model Kartı

SONIC, yayımlanmış üç Unitree G1 kontrol noktası sunar. Modeli referans temsiline ve hedeflediğiniz dağıtıma göre seçin. Belgenin tam çevirisi: [docs/MODEL-KARTI.md](docs/MODEL-KARTI.md).

### Mevcut modeller

| Model | Hugging Face konumu | SMPL referans girdisi | Kullanım amacı ve açıklamalar |
|---|---|---|---|
| **Varsayılan SONIC (ilk sürüm)** | Üst düzeydeki `model_encoder.onnx`, `model_decoder.onnx` ve `observation_config.yaml`; eğitim kontrol noktası `sonic_release/last.pt` | 20 ms aralıklı 10 gelecek kare, yaklaşık 200 ms referans önden bakışı | Hareket takibi, planlama, teleoperasyon ve mevcut dağıtımlarla uyumluluk için varsayılan genel amaçlı SONIC kontrolcüsü. G1 ve teleoperasyon gelecek referans gözlemleri `step5` kullanır. |
| **Düşük gecikmeli teleoperasyon** | [`low_latency/`](https://huggingface.co/nvidia/GEAR-SONIC/tree/main/low_latency) | 20 ms aralıklı 4 gelecek kare, yaklaşık 80 ms referans önden bakışı | Daha çabuk tepki veren tüm gövde teleoperasyonu ve VLA yürütmesi için. G1 ve teleoperasyon gelecek referans gözlemleri `step1` kullanır. Kodlayıcısını, kod çözücüsünü ve gözlem yapılandırmasını birlikte kullanın. |
| **SONIC v1.1** | [`sonic_v1_1/`](https://huggingface.co/nvidia/GEAR-SONIC/tree/main/sonic_v1_1) | 20 ms aralıklı 10 gelecek kare, yaklaşık 200 ms referans önden bakışı | Robot yönelimine göre normalleştirilmiş hedef yönelimi kullanır ve bilek pozu çoğaltmasıyla eğitildi. Yönelimi kararlı tüm gövde teleoperasyonu ve bu kontrolcüyü kullanan SONIC destekli VLA politikaları için. G1 ve teleoperasyon gelecek referans gözlemleri `step5` kullanır; bu model düşük gecikmeli model ile aynı model değildir. |

Üç modelin hepsi SONIC evrensel belirteç (universal-token) kontrolcüsünü kullanır, 64 boyutlu gizil hareket belirteçleri üretir, kontrolcüyü 50 Hz'de çalıştırır ve SMPL pozu, G1 hareket referansı ve teleoperasyon girdilerini destekler. Dağıtım C++ ve TensorRT kullanır. PyTorch kontrol noktaları Isaac Lab değerlendirmesini ve eğitime devam etmeyi destekler.

Önden bakış değerleri, kontrolcüye sunulan referans ufkunu tanımlar. Bunlar algılama, ağ, ön işleme ve çıkarımı da içeren toplam teleoperasyon gecikmesinin ölçümü **değildir**. Model ağırlıkları [NVIDIA Open Model License](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/LICENSE) kapsamındadır.

### Yayımlanan dosyalar

| Model | Dağıtım dosyaları | PyTorch ve yapılandırma dosyaları |
|---|---|---|
| Varsayılan SONIC | `model_encoder.onnx`, `model_decoder.onnx`, `observation_config.yaml` | `sonic_release/last.pt`, `sonic_release/config.yaml` |
| Düşük gecikmeli teleoperasyon | `low_latency/model_encoder.onnx`, `low_latency/model_decoder.onnx`, `low_latency/observation_config.yaml` | `low_latency/last.pt`, `low_latency/config.yaml`, `low_latency/model_config.yaml` |
| SONIC v1.1 | `sonic_v1_1/model_encoder.onnx`, `sonic_v1_1/model_decoder.onnx`, `sonic_v1_1/observation_config.yaml` | `sonic_v1_1/last.pt`, `sonic_v1_1/config.yaml`, `sonic_v1_1/model_config.yaml` |

Kökteki [`config.json`](https://huggingface.co/nvidia/GEAR-SONIC/blob/main/config.json), bu sürümler ve ortak dosyalar için asıl sürüm bildirimidir (release manifest). Resmi indirici, model dosyalarını indirmeden önce bu dosyayı çeker ve doğrular.

### Kullanım

Varsayılan modeli ve planlayıcıyı indirin:

```bash
python download_from_hf.py
```

Düşük gecikmeli teleoperasyon modelini ve planlayıcıyı indirin:

```bash
python download_from_hf.py --low-latency
```

SONIC v1.1'i ve planlayıcıyı indirin:

```bash
python download_from_hf.py --sonic-v1-1
```

Varsayılan C++ dağıtım yığınını çalıştırın:

```bash
cd gear_sonic_deploy
./deploy.sh --input-type zmq_manager real
```

Düşük gecikmeli C++ dağıtım yığınını çalıştırın:

```bash
cd gear_sonic_deploy
./deploy.sh \
    --cp policy/low_latency/model \
    --obs-config policy/low_latency/observation_config.yaml \
    --input-type zmq_manager \
    real
```

SONIC v1.1 C++ dağıtım yığınını çalıştırın:

```bash
cd gear_sonic_deploy
./deploy.sh \
    --cp policy/sonic_v1_1/model \
    --obs-config policy/sonic_v1_1/observation_config.yaml \
    --input-type zmq_manager \
    --motor-kp-scale 4,10=1.5 \
    --motor-kd-scale 4,10=1.5 \
    real
```

Bu, SONIC v1.1 için test edilmiş dağıtım ayarıdır. `4` ve `10` donanım indisleri sol ve sağ ayak bileği yunuslama (ankle-pitch) motorlarıdır. Bu motorların kontrol kazançlarını artırmak tüm gövde kararlılığını ve uygulamada bilek takibini iyileştirir. Bayraklar isteğe bağlıdır ve öteki kontrol noktalarını değiştirmez.

Varsayılan Python VLA başlatıcısını çalıştırın. Başlatıcı, C++ kontrolcüyü ve Python çıkarım istemcisini birlikte yönetir:

```bash
python gear_sonic/scripts/launch_inference.py \
    --camera-host 192.168.123.164 \
    --prompt "pick up the cup"
```

Düşük gecikmeli model için eşleşen dağıtım dosyalarını ekleyin:

```bash
python gear_sonic/scripts/launch_inference.py \
    --deploy-checkpoint policy/low_latency/model \
    --deploy-obs-config policy/low_latency/observation_config.yaml \
    --camera-host 192.168.123.164 \
    --prompt "pick up the cup"
```

SONIC v1.1 için eşleşen model/yapılandırmayı ve kazanç ayarını kullanın:

```bash
python gear_sonic/scripts/launch_inference.py \
    --deploy-checkpoint policy/sonic_v1_1/model \
    --deploy-obs-config policy/sonic_v1_1/observation_config.yaml \
    --deploy-motor-kp-scale 4,10=1.5 \
    --deploy-motor-kd-scale 4,10=1.5 \
    --camera-host 192.168.123.164 \
    --prompt "pick up the cup"
```

Python kontrol noktası değerlendirmesi ve ek dağıtım seçenekleri için [Model Kontrol Noktalarını İndirme](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/getting_started/download_models.md#sonic-v11-checkpoint) belgesine bakın. Kontrol noktasını fiziksel bir robotta kullanmadan önce simülasyonda test edin.

## 🥽 VR ile Tüm Gövde Teleoperasyonu

SONIC, PICO VR başlığıyla gerçek zamanlı tüm gövde teleoperasyonunu destekler. Böylece veri toplama ve etkileşimli kontrol için insandan robota doğal hareket aktarımı sağlar.

<div align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/sonic_v1_1_demo.gif" width="800" alt="SONIC v1.1 ile Unitree G1 tüm gövde teleoperasyon demosu">
</div>

<p align="center"><em><strong>SONIC v1.1:</strong> anlatımlı bilek hareketi ve dinamik hareketle tüm gövde teleoperasyon kipi.</em></p>

<div align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/sonic_low_latency_demo.gif" width="640" alt="SONIC Low Latency ile tüm gövde teleoperasyonu ve yerden nesne alma">
</div>

<p align="center"><em><strong>SONIC Low Latency:</strong> başarılı bir yerden alma hareketiyle tüm gövde teleoperasyon kipi.</em></p>

PICO teleoperasyonu için [VR Teleoperasyon Kurulumu](docs/VR-TELEOPERASYON-KURULUMU.md) belgesini izleyin. IsaacCapture 1.5 kullanmak için [Isaac Capture Kurulumu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/isaac_teleop_publisher_setup.md) belgesini izleyin ve yayıncıyı `--input-source isaac-teleop` ile başlatın.

<div align="center">
<table>
<tr>
<td align="center"><b>Yürüme</b></td>
<td align="center"><b>Koşma</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_walking.gif" width="400" alt="Teleoperasyonla yürüme"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_running.gif" width="400" alt="Teleoperasyonla koşma"></td>
</tr>
<tr>
<td align="center"><b>Yana hareket</b></td>
<td align="center"><b>Diz çökme</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_sideways.gif" width="400" alt="Teleoperasyonla yana hareket"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_kneeling.gif" width="400" alt="Teleoperasyonla diz çökme"></td>
</tr>
<tr>
<td align="center"><b>Ayağa kalkma</b></td>
<td align="center"><b>Zıplama</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_getup.gif" width="400" alt="Teleoperasyonla ayağa kalkma"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_jumping.gif" width="400" alt="Teleoperasyonla zıplama"></td>
</tr>
<tr>
<td align="center"><b>İki elle manipülasyon</b></td>
<td align="center"><b>Nesneyi elden ele verme</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_bimanual.gif" width="400" alt="Teleoperasyonla iki elle manipülasyon"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/teleop_switch_hands.gif" width="400" alt="Teleoperasyonla nesneyi elden ele verme"></td>
</tr>
</table>
</div>

## 🧭 Kinematik Planlayıcı

SONIC, gerçek zamanlı yürüyüş/hareket (lokomosyon) üretimi için bir kinematik planlayıcı içerir. Bir hareket stili seçin, klavye ya da oyun kumandasıyla yön verin, hızı ve yüksekliği anında ayarlayın.

<div align="center">
<table>
<tr>
<td align="center" colspan="2"><b>Açık alanda gezinme</b></td>
</tr>
<tr>
<td align="center" colspan="2"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_in_the_wild_navigation.gif" width="800" alt="Planlayıcıyla açık alanda gezinme"></td>
</tr>
<tr>
<td align="center"><b>Koşma</b></td>
<td align="center"><b>Neşeli</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_run.gif" width="400" alt="Planlayıcı koşma stili"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_happy.gif" width="400" alt="Planlayıcı neşeli stil"></td>
</tr>
<tr>
<td align="center"><b>Gizlice yürüme</b></td>
<td align="center"><b>Yaralı</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_stealth.gif" width="400" alt="Planlayıcı gizlice yürüme stili"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_injured.gif" width="400" alt="Planlayıcı yaralı yürüyüş stili"></td>
</tr>
<tr>
<td align="center"><b>Diz çökme</b></td>
<td align="center"><b>Elle emekleme</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_kneeling.gif" width="400" alt="Planlayıcı diz çökme"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_hand_crawling.gif" width="400" alt="Planlayıcı elle emekleme"></td>
</tr>
<tr>
<td align="center"><b>Dirsekle emekleme</b></td>
<td align="center"><b>Boks</b></td>
</tr>
<tr>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_elbow_crawling.gif" width="400" alt="Planlayıcı dirsekle emekleme"></td>
<td align="center"><img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/media/planner/planner_boxing.gif" width="400" alt="Planlayıcı boks stili"></td>
</tr>
</table>
</div>

## 🎓 SONIC Eğitimi

SONIC, [Bones-SEED](https://huggingface.co/datasets/bones-studio/seed) hareket yakalama veri setiyle (142 binden fazla hareket, yaklaşık 288 saat, Unitree G1'e yeniden hedeflenmiş) sıfırdan eğitilebilir ya da [Hugging Face](https://huggingface.co/nvidia/GEAR-SONIC) üzerindeki yayımlanmış kontrol noktasından ince ayarla geliştirilebilir.

### Hızlı başlangıç

```bash
# Eğitim bağımlılıklarını kurun (Isaac Lab ayrıca kurulmalıdır, belgelere bakın)
pip install -e "gear_sonic/[training]"

# Kontrol noktasını ve SMPL verisini Hugging Face'ten indirin
pip install huggingface_hub
python download_from_hf.py --training

# Bones-SEED G1 CSV dosyalarını huggingface.co/datasets/bones-studio/seed adresinden indirin, sonra dönüştürüp süzün
python gear_sonic/data_process/convert_soma_csv_to_motion_lib.py \
    --input /path/to/bones_seed/g1/csv/ \
    --output data/motion_lib_bones_seed/robot --fps 30 --fps_source 120 --individual --num_workers 16
python gear_sonic/data_process/filter_and_copy_bones_data.py \
    --source data/motion_lib_bones_seed/robot --dest data/motion_lib_bones_seed/robot_filtered

# Yayımlanmış kontrol noktasından ince ayar (64+ GPU önerilir)
accelerate launch --num_processes=8 gear_sonic/train_agent_trl.py \
    +exp=manager/universal_token/all_modes/sonic_release \
    +checkpoint=sonic_release/last.pt \
    num_envs=4096 headless=True \
    ++manager_env.commands.motion.motion_lib_cfg.motion_file=data/motion_lib_bones_seed/robot_filtered \
    ++manager_env.commands.motion.motion_lib_cfg.smpl_motion_file=data/smpl_filtered
```

Uyarlanabilir örnekleme (adaptive sampling) varsayılan olarak açıktır ve hataları, ortam sıfırlanmadan önce izlenen hareket imlecine bağlar.

Çok düğümlü eğitim, değerlendirme, ONNX dışa aktarma ve SOMA kodlayıcı kurulumunu içeren tam rehber için:
📖 [Kurulum (Eğitim)](https://nvlabs.github.io/GR00T-WholeBodyControl/getting_started/installation_training.html) |
[Eğitim Rehberi](https://nvlabs.github.io/GR00T-WholeBodyControl/user_guide/training.html)

## ✅ Yapılacaklar

- [x] Önceden eğitilmiş SONIC politika kontrol noktalarını yayımlamak
- [x] C++ çıkarım yığınını açık kaynak yapmak
- [x] Kurulum belgeleri
- [x] Teleoperasyon yığınını ve gösterim betiklerini açık kaynak yapmak
- [x] Hareket taklidi ve ince ayar için eğitim betiklerini ve tariflerini yayımlamak
- [x] Büyük ölçekli veri toplama iş akışlarını ve VLA ince ayar betiklerini açık kaynak yapmak
- [x] Ön işlenmiş ek büyük ölçekli insan hareketi veri setleri yayımlamak

## 📁 Depoda Neler Var

Bu sürüm şunları içerir:

- **`gear_sonic_deploy`**: SONIC politikalarını gerçek donanımda dağıtmak için C++ çıkarım yığını
- **`gear_sonic`**: Tam SONIC eğitim yığını. PPO eğitimi, veri işleme hattı ve Bones-SEED ile özel hareket veri setlerinde eğitim için yapılandırma sistemi
- **`motionbricks`**: MotionBricks gerçek zamanlı gizil üretken yığınının önizleme sürümü. Etkileşimli G1 demosu, önceden eğitilmiş kontrol noktaları, sentetik eğitim kodu ve hareket temsili belgeleri

### Kurulum

> **Git LFS gerekir.** Bu depo büyük ikili dosyalar (ağ dosyaları, ONNX modelleri) içerir. Git LFS olmadan gerçek veri yerine küçük işaretçi (pointer) dosyalar iner ve süreç hata mesajı vermeden yanlış ilerler. Git LFS kurulu değilse önce kurun:
> `sudo apt install git-lfs && git lfs install`
>
> MotionBricks'in önceden eğitilmiş kontrol noktaları, olağan monorepo kurulumunda yaklaşık 2,2 GiB'lık ek indirmeyi önlemek için varsayılan olarak atlanır. MotionBricks GIF'leri ve ağ dosyaları yine normal şekilde iner. MotionBricks demosunu çalıştıracaksanız kontrol noktalarını ayrıca çekin.

```bash
git clone https://github.com/NVlabs/GR00T-WholeBodyControl.git
cd GR00T-WholeBodyControl
git lfs pull

# İsteğe bağlı: MotionBricks'in önceden eğitilmiş kontrol noktalarını çekin.
git lfs pull --include="motionbricks/out/**" --exclude=""

# Ortamınızı doğrulayın
python check_environment.py
```

### Hangi ortama ihtiyacım var?

| Yapmak istediğim... | Ortam | Nasıl kurulur |
|---|---|---|
| **SONIC eğitmek / ince ayar yapmak** | Isaac Lab'in Python ortamı | [Isaac Lab'i kurun](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/index.html), ardından `pip install -e "gear_sonic/[training]"` |
| **MuJoCo simülasyonu çalıştırmak** | `.venv_sim` (otomatik oluşur) | `bash install_scripts/install_mujoco_sim.sh` |
| **VR teleoperasyonu** | `.venv_teleop` (otomatik oluşur) | `bash install_scripts/install_pico.sh` |
| **Veri toplamak** | `.venv_data_collection` (otomatik oluşur) | `bash install_scripts/install_data_collection.sh` |
| **Gerçek robotta dağıtmak** | C++ derlemesi | [Dağıtım belgelerine](docs/KURULUM.md) bakın |

Her kullanım durumunun kendi hafif ortamı vardır. Kurulum betikleri `uv` kullanır ve yalıtılmış sanal ortamları (venv) otomatik oluşturur, bunları elle yönetmeniz gerekmez. Isaac Lab gerektiren (ayrıca kurulan) tek iş eğitimdir.

## 📚 Belgeler

📚 **[Tam belgeler (orijinal, İngilizce)](https://nvlabs.github.io/GR00T-WholeBodyControl/)**

### Başlarken
- [Kurulum Rehberi](docs/KURULUM.md)
- [Hızlı Başlangıç](docs/HIZLI-BASLANGIC.md)
- [VR Teleoperasyon Kurulumu](docs/VR-TELEOPERASYON-KURULUMU.md)

### Eğitimler
- [Klavye ile Kontrol](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/keyboard.html)
- [Oyun Kumandası ile Kontrol](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/gamepad.html)
- [ZMQ Haberleşmesi](https://nvlabs.github.io/GR00T-WholeBodyControl/tutorials/zmq.html)
- [ZMQ Manager / PICO VR](docs/VR-TUM-GOVDE-TELEOPERASYON.md)

### Eğitim (model)
- [Kurulum (Eğitim)](https://nvlabs.github.io/GR00T-WholeBodyControl/getting_started/installation_training.html)
- [Eğitim Rehberi](https://nvlabs.github.io/GR00T-WholeBodyControl/user_guide/training.html)
- [Eğitim Verisi](https://nvlabs.github.io/GR00T-WholeBodyControl/user_guide/training_data.html)

### En iyi uygulamalar
- [Teleoperasyon](docs/TELEOPERASYON-REHBERI.md)

### Bu depodaki Türkçe belgeler

SONIC kurulumu, Unitree G1 üzerinde sim2sim ve gerçek robot dağıtımı ile teleoperasyon belgeleri önceliklidir. Aşağıdaki yedi belge tam çevrilmiştir.

| Türkçe belge | Kaynak dosya | Konu |
| --- | --- | --- |
| [docs/KURULUM.md](docs/KURULUM.md) | `docs/source/getting_started/installation_deploy.md` | TensorRT sürümleri, yerel ve Docker kurulumu, C++ derleme |
| [docs/HIZLI-BASLANGIC.md](docs/HIZLI-BASLANGIC.md) | `docs/source/getting_started/quickstart.md` | Isaac Lab değerlendirmesi, MuJoCo sim2sim, gerçek G1 |
| [docs/MODEL-KARTI.md](docs/MODEL-KARTI.md) | `docs/source/model_card.md` | Üç G1 kontrol noktası, dosyalar, sınırlamalar ve güvenlik |
| [docs/JETPACK6-YUKLEME.md](docs/JETPACK6-YUKLEME.md) | `docs/source/references/jetpack6.md` | G1 üstündeki Orin NX için JetPack 6.2 yükleme |
| [docs/VR-TELEOPERASYON-KURULUMU.md](docs/VR-TELEOPERASYON-KURULUMU.md) | `docs/source/getting_started/vr_teleop_setup.md` | PICO donanımı, hareket izleyiciler, XRoboToolkit |
| [docs/VR-TUM-GOVDE-TELEOPERASYON.md](docs/VR-TUM-GOVDE-TELEOPERASYON.md) | `docs/source/tutorials/vr_wholebody_teleop.md` | Simülasyonda ve gerçek robotta PICO ile tüm gövde teleoperasyonu |
| [docs/TELEOPERASYON-REHBERI.md](docs/TELEOPERASYON-REHBERI.md) | `docs/source/user_guide/teleoperation.md` | Kıyafet, Wi-Fi gecikmesi, kalibrasyon, kip geçiş güvenliği |

### Orijinal belgeler (İngilizce)

Şu belgeleri orijinal depodan okuyabilirsiniz:

- Başlarken: [Model kontrol noktalarını indirme](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/getting_started/download_models.md) · [Kurulum (Eğitim)](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/getting_started/installation_training.md)
- Eğitimler: [Klavye](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/keyboard.md) · [Oyun kumandası](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/gamepad.md) · [ZMQ](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/zmq.md) · [Arayüz yöneticisi (Manager)](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/manager.md) · [Isaac Teleop / Isaac Capture kurulumu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/isaac_teleop_publisher_setup.md) · [Canlı kamera teleoperasyonu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/live_camera_teleop.md) · [Veri toplama](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/data_collection.md) · [VLA iş akışı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/vla_workflow.md) · [VLA çıkarımı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/vla_inference.md)
- Kullanım rehberi: [Eğitim](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/user_guide/training.md) · [Eğitim verisi](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/user_guide/training_data.md) · [Yapılandırma](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/user_guide/configuration.md) · [Yeni gövdeler (embodiments)](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/user_guide/new_embodiments.md) · [Sorun giderme](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/user_guide/troubleshooting.md)
- Başvuru: [Decoupled WBC](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/decoupled_wbc.md) · [Dağıtım kodu ve program akışı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/deployment_code.md) · [Eğitim kodu](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/training_code.md) · [Gözlem yapılandırması](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/observation_config.md) · [Hareket referansı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/motion_reference.md) · [Planlayıcı ONNX](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/planner_onnx.md) · [Kurallar (conventions)](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/conventions.md)
- MotionBricks: [Alt proje README](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/motionbricks/README.md) · [Hareket temsili](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/motionbricks/docs/motion_representation.md) · [Kendi veri setinizi ekleme](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/motionbricks/docs/adding_your_own_dataset.md)

MotionBricks'in kendi deposundaki belgelerin Türkçe çevirisi [unitree-g1-motionbricks-turkce](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce) deposundadır.

---

## 📝 Atıf

GEAR-SONIC'i araştırmanızda kullanırsanız lütfen şu atfı verin:

```bibtex
@article{luo2025sonic,
    title={SONIC: Supersizing Motion Tracking for Natural Humanoid Whole-Body Control},
    author={Luo, Zhengyi and Yuan, Ye and Wang, Tingwu and Li, Chenran and Chen, Sirui and Casta\~neda, Fernando and Cao, Zi-Ang and Li, Jiefeng and Minor, David and Ben, Qingwei and Da, Xingye and Ding, Runyu and Hogg, Cyrus and Song, Lina and Lim, Edy and Jeong, Eugene and He, Tairan and Xue, Haoru and Xiao, Wenli and Wang, Zi and Yuen, Simon and Kautz, Jan and Chang, Yan and Iqbal, Umar and Fan, Linxi and Zhu, Yuke},
    journal={arXiv preprint arXiv:2511.07820},
    year={2025}
}
```

---

## 📄 Lisans

Bu proje çift lisans kullanır:

- **Kaynak kod:** Apache License 2.0 ile lisanslanır. Bu depodaki tüm kod, betik ve yazılım bileşenlerine uygulanır.
- **Model ağırlıkları:** NVIDIA Open Model License ile lisanslanır. Eğitilmiş tüm model kontrol noktalarına ve ağırlıklarına uygulanır.

Çift lisansın tam metni için [LICENSE](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/LICENSE) dosyasına bakın.

Bu projeyi kullanmadan önce iki lisansı da inceleyin. NVIDIA Open Model License, atıf yapılması koşuluyla ticari kullanıma izin verir ve NVIDIA'nın Trustworthy AI koşullarına uyulmasını şart koşar.

Apache 2.0 lisansı, üçüncü taraf atıfları ve DCO metni dahil gerekli tüm hukuki belgeler bu deponun [/legal](https://github.com/NVlabs/GR00T-WholeBodyControl/tree/main/legal) klasöründe toplanmıştır.

**Lisans doğrulama notu (çeviri ekibi):** Kökteki [LICENSE](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/LICENSE) dosyası ikinci bölümde NVIDIA Open Model License Agreement metnini (son değişiklik 24 Ekim 2025) içerir. Model ya da türev model dağıtan taraf bu sözleşmenin bir kopyasını ekler ve "Licensed by NVIDIA Corporation under the NVIDIA Open Model License." atıf bildirimini kullanır. `legal/` klasöründeki üçüncü taraf listesi şu bileşenleri sayar: unitree_sdk2_python (Unitree Robotics, BSD 3-Clause), XRoboToolkit-PC-Service-Pybind (XR-Robotics, MIT), robosuite (ARISE Initiative, Apache 2.0), tmux-plugins/tpm (MIT) ve BeyondMimic (HybridRobotics, MIT). `gear_sonic_deploy/thirdparty/unitree_sdk2` klasörü kendi lisans dosyalarıyla gelir.

**Bu çevirinin lisansı:** Bu depodaki metinler, NVIDIA belgelerinin Türkçe çevirisidir ve [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0) ile yayımlanır. Özgün eser: NVIDIA, [NVlabs/GR00T-WholeBodyControl](https://github.com/NVlabs/GR00T-WholeBodyControl) README ve belgeleri. Değişiklik: Türkçeye çeviri ve Türkiye bağlantıları. Bu depo kaynak kod, model ağırlığı, hareket verisi ya da medya dosyası barındırmaz. Görseller orijinal depodan gösterilir.

---

## 💬 Destek

Sorular ve sorunlar için geri bildiriminizi GEAR WBC ekibine [gear-wbc@nvidia.com](mailto:gear-wbc@nvidia.com) adresinden iletin. Bu Türkçe çeviriyle ilgili düzeltme önerilerinizi bu depoda issue olarak açabilirsiniz.

## 🧱 MotionBricks

<p style="font-size: 1.2em;">
  <a href="https://nvlabs.github.io/motionbricks/"><strong>Proje sayfası</strong></a> |
  <a href="https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/motionbricks/README.md"><strong>Alt proje README</strong></a> |
  <a href="https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce"><strong>Türkçe çeviri</strong></a>
</p>

<div align="center">
  <img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/motionbricks/assets/gifs/teaser_animation.gif" width="400" alt="MotionBricks animasyon karakteri">
  <img src="https://media.githubusercontent.com/media/NVlabs/GR00T-WholeBodyControl/main/motionbricks/assets/gifs/teaser_robotics.gif" width="400" alt="MotionBricks ile Unitree G1">
</div>

MotionBricks, animasyon ve robotikte etkileşimli hareket kontrolünü dönüştüren gerçek zamanlı bir üretken çatıdır. Büyük ölçekli bir gizil omurgayı sezgisel "smart primitive" (akıllı ilkel) yapı taşlarıyla birleştirir ve 15.000 FPS hızda yüksek kaliteli, sıfır atışlı (zero-shot) hareket sentezi sunar. Bu depodaki takip tabanlı GEAR-SONIC kontrolcülerini tamamlar.

Bu önizleme sürümü etkileşimli bir G1 demosu (klavyeyle sürülen, MuJoCo görüntüleyicili), önceden eğitilmiş kontrol noktaları (VQVAE · poz · kök), sentetik bir eğitim hattı ve hareket temsili belgeleriyle gelir. Önceden eğitilmiş kontrol noktaları monorepo klonlarında isteğe bağlıdır. Demoyu kullanmadan önce depo kökünde `git lfs pull --include="motionbricks/out/**" --exclude=""` komutunu çalıştırın. GEAR-SONIC hattına tümüyle gömülü tam sürüm için hedeflenen süre yaklaşık bir aydır. Kurulum, demo ve eğitim yönergeleri için [`motionbricks/README.md`](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/motionbricks/README.md) dosyasına bakın.

MotionBricks artık kendi deposunda ([NVlabs/motionbricks](https://github.com/NVlabs/motionbricks)) yayımlanıyor. Türkçe anlatım için [Unitree G1 MotionBricks rehberine](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi) bakın.

## 🔀 Decoupled WBC

GR00T N1.5 ve N1.6 modellerinde kullanılan Decoupled WBC için [Decoupled WBC belgelerine](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/decoupled_wbc.md) bakın. Belgeye göre bu yazılım yığını, birden çok insansı platformda yürüme ve manipülasyon (loco-manipulation) deneyleri içindir ve birincil desteği Unitree G1'e verir.

## 🙏 Teşekkür

Bu depodaki kodun bazı parçalarının türetildiği şu projelere teşekkür ederiz (orijinal depodan):

- [Beyond Mimic](https://github.com/HybridRobotics/whole_body_tracking)
- [Isaac Lab](https://github.com/isaac-sim/IsaacLab)

---

## 🎯 Gerçek robotta sonuçlar (NVIDIA ve topluluk)

Bu bölüm, SONIC'in gerçek Unitree G1 üzerindeki sonuçlarını NVIDIA'nın yayınlarından ve topluluk yazılarından aktarır.

- **SONIC makalesi:** NVIDIA'nın [arXiv 2511.07820](https://arxiv.org/abs/2511.07820) numaralı makalesine göre deneyler 29 eklemli Unitree G1 üzerinde yürütüldü. Makale yöntemi, gerçek G1'e dağıtılan 124 farklı hareket sekansıyla değerlendirir. Gerçek robot videoları [GEAR-SONIC proje sayfasında](https://nvlabs.github.io/GEAR-SONIC/) duruyor.
- **Model kartı:** Hugging Face'teki [nvidia/GEAR-SONIC](https://huggingface.co/nvidia/GEAR-SONIC) model kartı G1 için denetleyici varyantları listeler. Teleoperasyon için düşük gecikmeli varyant yaklaşık 80 ms, varsayılan varyant yaklaşık 200 ms önden bakışla çalışır.
- **README haberi:** GR00T-WholeBodyControl README'sindeki 14 Eylül 2026 tarihli habere göre PICO teleoperasyonu için IsaacTeleop 1.4 ve Jetson AGX Orin desteği eklendi.
- **Topluluk, Classmethod:** Classmethod'un 2026 ağustosunda yayımlanan [Japonca yazısına](https://dev.classmethod.jp/articles/nvidia-gear-sonic-g1-teleope/) göre gerçek bir Unitree G1'de PICO 4 ve gövde izleyicileriyle tüm gövde teleoperasyonu yapıldı. Robot ayakta kaldı ve operatörün tüm gövde hareketini izledi, Inspire el kavrayışı da bu akışla birlikte çalıştı. Çıkarım bilgisayarı bir DGX Spark'tı ve G1'e kablolu LAN ile bağlandı. Ekip donanıma geçmeden önce akışı MuJoCo'da simülasyonda çalıştırdı.

## 🚀 Unitree G1 ile GEAR SONIC: Türkiye'de nereden başlanır

Model kartına göre yayımlanan üç SONIC kontrol noktasının hedef gövdesi Unitree G1'dir. Kontrolcü 50 Hz'de çalışır, dağıtım C++ ve TensorRT ile yapılır. NVIDIA'nın belgeleri G1 için şu yolu izler:

1. **Kurulum:** İş istasyonunda TensorRT 10.13, G1 üstündeki Orin'de JetPack 6 ile TensorRT 10.7, G1 üstündeki Thor'da TensorRT 10.13. Belge, listelenen sürümlerin aynen kullanılmasını şart koşar ([KURULUM.md](docs/KURULUM.md), [JETPACK6-YUKLEME.md](docs/JETPACK6-YUKLEME.md)).
2. **Simülasyon:** Önce Isaac Lab değerlendirmesi, sonra MuJoCo'da sim2sim döngüsü. NVIDIA, gerçek donanıma geçmeden önce kontrolleri simülasyonda öğrenmeyi güçlü biçimde önerir ([HIZLI-BASLANGIC.md](docs/HIZLI-BASLANGIC.md)).
3. **Gerçek robot:** `./deploy.sh real` komutu robot ağ arayüzünü (192.168.123.x) otomatik algılar. Güvenlik alanı ve acil durdurma için hazır bekleyen bir güvenlik operatörü şarttır.
4. **Teleoperasyon:** PICO 4 / PICO 4 Pro başlık, iki kontrolcü ve ayak bileklerine takılan iki hareket izleyiciyle tüm gövde teleoperasyonu ([VR-TELEOPERASYON-KURULUMU.md](docs/VR-TELEOPERASYON-KURULUMU.md), [VR-TUM-GOVDE-TELEOPERASYON.md](docs/VR-TUM-GOVDE-TELEOPERASYON.md), [TELEOPERASYON-REHBERI.md](docs/TELEOPERASYON-REHBERI.md)).
5. **VLA:** NVIDIA'nın haberine göre G1 üzerinde teleoperasyon verisi toplanır, Isaac-GR00T N1.7'ye ince ayar yapılır ve politika SONIC tüm gövde kontrolüyle dağıtılır. Bu akışın Türkçe özeti [Isaac GR00T Türkçe deposunda](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) bulunur.

**Platform seçimi**

| Ürün | GEAR SONIC iş akışındaki yeri |
| --- | --- |
| [Unitree G1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) | SONIC kontrol noktalarının hedeflediği insansı robot, 132 cm, 35 kg, 23-43 DoF |
| [Unitree G1 EDU teklif](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) | Geliştirici ve araştırma sürümü için teklif formu |
| [Unitree G1 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-plus) | 2 DoF boyun, yüksek tork, dokunsal kafa ve 6 mikrofonlu G1 |
| [Unitree G1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-d) | İnsansı robot veri toplama ve eğitim platformu, Jetson Orin |
| [G1 Ar-Ge platformu](https://www.robotlar.org/tr/g1-arge-platformu) | Üniversite ve Ar-Ge laboratuvarları için G1 çözüm sayfası |
| [Unitree Dex3-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex3-1) | G1 için 7 serbestlik dereceli el, opsiyonel 33 dokunsal sensör |

**Türkçe rehberler (iş akışına göre)**

- Bu deponun özeti ve G1 dağıtım adımları: [GEAR SONIC ile Unitree G1 tüm gövde kontrol rehberi](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi)
- VLA akışı ve Isaac GR00T N1.7: [Unitree G1 Isaac GR00T rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-isaac-gr00t-rehberi)
- XR ile teleoperasyon ve veri toplama: [Unitree G1 teleoperasyon](https://www.robotlar.org/unitree/g1-teleoperasyon)
- Fizik tabanlı insansı eğitimi: [ProtoMotions rehberi](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi)
- Gerçek zamanlı hareket üretimi: [Unitree G1 MotionBricks rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi)
- SONIC eğitim verisi: [BONES-SEED hareket veri seti rehberi](https://www.robotlar.org/tr/rehber/bones-seed-hareket-veri-seti-rehberi)
- Pekiştirmeli öğrenme: [Unitree RL Gym](https://www.robotlar.org/unitree/rl-gym)
- Veri seti ve taklit öğrenme: [Unitree LeRobot rehberi](https://www.robotlar.org/tr/rehber/unitree-lerobot-rehberi)
- Robotla haberleşme ve düşük seviye kontrol: [Unitree SDK rehberi](https://www.robotlar.org/unitree/sdk-rehberi)
- Robot üstü hesaplama: [Jetson Orin rehberi](https://www.robotlar.org/tr/rehber/saha-robotu-jetson-orin-dayanikli-bilgisayar)

**Türkçe GitHub depoları:** [G1](https://github.com/Unitree-Turkiye/unitree-g1-turkce) · [G1 + Isaac GR00T](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) · [XR Teleoperasyon](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [SDK2](https://github.com/Unitree-Turkiye/unitree-sdk2-turkce) · [ProtoMotions](https://github.com/Unitree-Turkiye/nvidia-protomotions-turkce) · [G1 MotionBricks](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce)

Üniversite ve kurumlar için platform, eğitim ve Türkçe geliştirici desteği konusunda [kurumsal teklif formundan](https://www.robotlar.org/tr/fiyat/unitree-kurumsal-teklif-al) bize ulaşabilirsiniz.

## 🧬 NVIDIA İnsansı Hareket Ailesi (Türkçe)

NVIDIA'nın animasyon, robotik ve fiziksel yapay zeka için yayımladığı insansı hareket projeleri birbirini tamamlar: SOMA ortak gövde modelini, BONES-SEED veriyi, GEM videodan hareketi, Kimodo ve ARDY hareket üretimini, MotionBricks gerçek zamanlı hareket planlamayı, SOMA Retargeter insan hareketini Unitree G1'e aktarmayı, ProtoMotions fizik tabanlı eğitimi, GEAR SONIC de gerçek robotta tüm gövde kontrolünü üstlenir. Akışın bütününü [merkez rehberde](https://www.robotlar.org/tr/rehber/nvidia-insansi-hareket-ekosistemi-unitree-g1-rehberi) anlatıyoruz.

| Proje | Ne yapar | Türkçe repo | Türkçe rehber | Resmi depo |
|---|---|---|---|---|
| MotionBricks | Bağlam ve hedef pozlar arasında gerçek zamanlı hareket üretir, Unitree G1 modeliyle gelir | [unitree-g1-motionbricks-turkce](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi) | [NVlabs/motionbricks](https://github.com/NVlabs/motionbricks) |
| Kimodo | Denetlenebilir çevrim dışı insan hareketi üretimi | [nvidia-kimodo-turkce](https://github.com/Unitree-Turkiye/nvidia-kimodo-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-kimodo-hareket-uretimi-rehberi) | [nv-tlabs/kimodo](https://github.com/nv-tlabs/kimodo) |
| ARDY | Kimodo üzerine gerçek zamanlı, etkileşimli hareket üretimi | [nvidia-ardy-turkce](https://github.com/Unitree-Turkiye/nvidia-ardy-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-ardy-gercek-zamanli-hareket-rehberi) | [nv-tlabs/ardy](https://github.com/nv-tlabs/ardy) |
| SOMA Body Model | Birleşik, parametrik insan gövde modeli | [nvidia-soma-govde-modeli-turkce](https://github.com/Unitree-Turkiye/nvidia-soma-govde-modeli-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-soma-govde-modeli-rehberi) | [NVlabs/SOMA-X](https://github.com/NVlabs/SOMA-X) |
| BONES-SEED | SOMA ve G1 biçiminde büyük hareket yakalama veri seti | [bones-seed-hareket-veri-seti-turkce](https://github.com/Unitree-Turkiye/bones-seed-hareket-veri-seti-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/bones-seed-hareket-veri-seti-rehberi) | [bones-studio/seed](https://huggingface.co/datasets/bones-studio/seed) |
| ProtoMotions | Benzetilen insan ve insansıları eğitmek için GPU hızlandırmalı simülasyon çatısı | [nvidia-protomotions-turkce](https://github.com/Unitree-Turkiye/nvidia-protomotions-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi) | [NVlabs/ProtoMotions](https://github.com/NVlabs/ProtoMotions) |
| SOMA Retargeter | SOMA hareketini Unitree G1 gibi insansı robotlara aktarır | [unitree-g1-soma-retargeter-turkce](https://github.com/Unitree-Turkiye/unitree-g1-soma-retargeter-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/soma-retargeter-unitree-g1-hareket-aktarimi-rehberi) | [NVIDIA/soma-retargeter](https://github.com/NVIDIA/soma-retargeter) |
| GEM | Görüntü ve videodan tüm gövde ve el hareketini kestirir | [nvidia-gem-x-turkce](https://github.com/Unitree-Turkiye/nvidia-gem-x-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-gem-x-videodan-hareket-rehberi) | [NVlabs/GEM-X](https://github.com/NVlabs/GEM-X) |
| **GEAR SONIC** | **Fiziksel insansı robotlar için davranış temel modeli ve tüm gövde kontrolü** | **[unitree-g1-gear-sonic-turkce](https://github.com/Unitree-Turkiye/unitree-g1-gear-sonic-turkce)** | **[Rehber](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi)** | **[NVlabs/GR00T-WholeBodyControl](https://github.com/NVlabs/GR00T-WholeBodyControl)** |

İlgili Türkçe kaynaklar: [Unitree G1 + Isaac GR00T](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) · [XR teleoperasyon](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [LeRobot](https://github.com/Unitree-Turkiye/unitree-lerobot-turkce) · [RL Gym](https://github.com/Unitree-Turkiye/unitree-rl-gym-turkce) · [MuJoCo](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce)

## 🤖 Unitree Ürün Ailesi (Türkiye)

Robotlar.org, Unitree Robotics'in Türkiye resmi distribütörüdür. Aşağıdaki ürünlerin Türkçe tanıtımı, teknik özellikleri ve kurumsal teklif formu sitede yer alır. Fiyatlar konfigürasyona göre değiştiği için [teklif üzerinden](https://www.robotlar.org/tr/fiyat/unitree-kurumsal-teklif-al) paylaşılır.

### İnsansı robotlar

| Ürün | Ne işe yarar |
|---|---|
| [Unitree G1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) | 132 cm, 35 kg, 23-43 DoF insansı robot |
| [Unitree G1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-d) | İnsansı robot veri toplama ve eğitim platformu, Jetson Orin |
| [Unitree G1 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-plus) | 2 DoF boyun, yüksek tork, dokunsal kafa ve 6 mikrofonlu G1 |
| [Unitree H2](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2) | 182 cm, 70 kg, 31 DoF tam boy insansı robot |
| [Unitree H2-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-d) | 7 DoF kollu, tekerlekli çift kollu insansı robot |
| [Unitree H2 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-plus) | NVIDIA Isaac GR00T referans insansı robotu, 75 DoF, Jetson Thor |
| [Unitree H2-X](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-x) | Hizmet ve koruma görevleri, 4 m platformdan iniş |
| [Unitree R1](https://www.robotlar.org/tr/insansi-robotlar/unitree-r1) | 123 cm, 29 kg, 26 DoF insansı robot (AIR, R1, EDU sürümleri) |
| [Unitree R1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-r1-d) | R1-A5 ya da R1-A7 (7-DOF) kollu çift kollu insansı platform |
| [Tüm insansı robotlar →](https://www.robotlar.org/tr/insansi-robotlar) | |

### Robot elleri

| Ürün | Ne işe yarar |
|---|---|
| [Unitree Dex3-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex3-1) | G1 için 7 serbestlik dereceli el, opsiyonel 33 dokunsal sensör |
| [Unitree Dex5-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex5-1) | 20 serbestlik dereceli (16 aktif) el, Dex5-1P sürümünde 94 dokunsal sensör |
| [Unitree Dex5-S](https://www.robotlar.org/tr/robot-elleri/unitree-dex5-s) | 22 aktif serbestlik, 620 g, çift T1s Ethernet, 1000 Hz |
| [Unitree Dex1-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex1-1) | İki çeneli tutucu: 90 mm strok, 5-120 N ayarlı kavrama, RS-485 |
| [Tüm robot elleri →](https://www.robotlar.org/tr/robot-elleri) | |

### Dört ayaklı robotlar ve bileşenler

[Go2](https://www.robotlar.org/tr/kopek-robotlar/unitree-go2) · [Go2-W](https://www.robotlar.org/tr/kopek-robotlar/unitree-go2-w) · [B2](https://www.robotlar.org/tr/kopek-robotlar/unitree-b2) · [B2-W](https://www.robotlar.org/tr/kopek-robotlar/unitree-b2-w) · [A2](https://www.robotlar.org/tr/kopek-robotlar/unitree-a2) · [AS2](https://www.robotlar.org/tr/kopek-robotlar/unitree-as2) · [Tüm dört ayaklı robotlar](https://www.robotlar.org/tr/kopek-robotlar) · [4D LiDAR L2](https://www.robotlar.org/tr/unitree-bilesenler/4d-lidar-l2) · [Tüm Unitree bileşenleri](https://www.robotlar.org/tr/unitree-bilesenler)

## 📖 Türkçe Rehberler

- [NVIDIA insansı hareket ailesi ve Unitree G1 (merkez rehber)](https://www.robotlar.org/tr/rehber/nvidia-insansi-hareket-ekosistemi-unitree-g1-rehberi)
- [Unitree G1'de hareket taklidi: BeyondMimic'ten SONIC'e hareket izleme yöntemleri](https://www.robotlar.org/tr/rehber/unitree-g1-hareket-taklidi-motion-tracking-rehberi) · [Türkçe araştırma deposu: unitree-g1-hareket-taklidi-arastirma](https://github.com/Unitree-Turkiye/unitree-g1-hareket-taklidi-arastirma)
- [GEAR SONIC ile Unitree G1 tüm gövde kontrol rehberi](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi)
- [Unitree G1 Isaac GR00T rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-isaac-gr00t-rehberi)
- [Unitree G1 XR teleoperasyon](https://www.robotlar.org/unitree/g1-teleoperasyon)
- [NVIDIA ProtoMotions insansı simülasyon rehberi](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi)
- [Unitree G1 MotionBricks rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi)
- [Unitree LeRobot: veri toplama ve taklit öğrenme](https://www.robotlar.org/tr/rehber/unitree-lerobot-rehberi)
- [Unitree RL Gym](https://www.robotlar.org/unitree/rl-gym)
- [Unitree SDK rehberi](https://www.robotlar.org/unitree/sdk-rehberi)
- [Unitree eğitim çözümleri (üniversite ve laboratuvar)](https://www.robotlar.org/tr/unitree-egitim-cozumleri)
- [Tüm açık kaynak Unitree projeleri (Türkçe)](https://www.robotlar.org/tr/acik-kaynak)

## 🔗 Unitree Türkiye GitHub Ağı

**Modeller:** [Go2](https://github.com/Unitree-Turkiye/unitree-go2-turkce) · [Go2-W](https://github.com/Unitree-Turkiye/unitree-go2-w-turkce) · [B2](https://github.com/Unitree-Turkiye/unitree-b2-turkce) · [A2](https://github.com/Unitree-Turkiye/unitree-a2-turkce) · [G1](https://github.com/Unitree-Turkiye/unitree-g1-turkce) · [H2](https://github.com/Unitree-Turkiye/unitree-h2)
**Geliştirici:** [SDK2](https://github.com/Unitree-Turkiye/unitree-sdk2-turkce) · [XR Teleoperasyon](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [LeRobot](https://github.com/Unitree-Turkiye/unitree-lerobot-turkce) · [Becerikli El](https://github.com/Unitree-Turkiye/unitree-hunerli-el-turkce) · [MuJoCo](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce) · [ROS2](https://github.com/Unitree-Turkiye/unitree-ros2-turkce) · [VLA](https://github.com/Unitree-Turkiye/unitree-vla-turkce)
**G1 araştırma:** [G1 + Isaac GR00T](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) · [G1 GEAR SONIC](https://github.com/Unitree-Turkiye/unitree-g1-gear-sonic-turkce) · [G1 MotionBricks](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce) · [G1 dokunsal algılama giysisi](https://github.com/Unitree-Turkiye/unitree-g1-dokunsal-giysi-turkce) · [🏠 Unitree Türkiye](https://github.com/Unitree-Turkiye)

---

> ℹ️ **Unitree Türkiye:** Unitree Robotics'in Türkiye resmi satış ve teknik distribütörü. Unitree G1 ailesi için teklif, kurulum, eğitim ve Türkçe geliştirici desteği: **[robotlar.org](https://www.robotlar.org/tr/rehber/unitree-turkiye-distributoru)** · 📧 info@robotlar.org

© 2026 **[www.robotlar.org](https://www.robotlar.org)**
