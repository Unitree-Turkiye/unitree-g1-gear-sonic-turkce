> Bu belge NVlabs/GR00T-WholeBodyControl deposundaki `docs/source/model_card.md` dosyasının Türkçe çevirisidir (Apache-2.0, commit 1e1ec4f). Model ağırlıkları NVIDIA Open Model License kapsamındadır.

# Model Kartı

SONIC, Unitree G1 için yayımlanmış üç tüm gövde kontrolcüsü kontrol noktası sunar. Modeli referans temsiline ve hedeflediğiniz dağıtıma göre seçin.

## Mevcut modeller

| Model | Hugging Face konumu | SMPL referans girdisi | Kullanım amacı ve açıklamalar |
|---|---|---|---|
| **Varsayılan SONIC (ilk sürüm)** | Üst düzeydeki `model_encoder.onnx`, `model_decoder.onnx` ve `observation_config.yaml`; eğitim kontrol noktası `sonic_release/last.pt` | 20 ms aralıklı 10 gelecek kare, yaklaşık 200 ms referans önden bakışı (lookahead) | Hareket takibi, planlama, teleoperasyon ve mevcut dağıtımlarla uyumluluk için varsayılan genel amaçlı SONIC kontrolcüsü. G1 ve teleoperasyon gelecek referans gözlemleri `step5` kullanır. |
| **Düşük gecikmeli teleoperasyon** | [`low_latency/`](https://huggingface.co/nvidia/GEAR-SONIC/tree/main/low_latency) | 20 ms aralıklı 4 gelecek kare, yaklaşık 80 ms referans önden bakışı | Daha çabuk tepki veren tüm gövde teleoperasyonu ve VLA yürütmesi için. G1 ve teleoperasyon gelecek referans gözlemleri `step1` kullanır. Kodlayıcısını, kod çözücüsünü ve gözlem yapılandırmasını birlikte kullanın. |
| **SONIC v1.1** | [`sonic_v1_1/`](https://huggingface.co/nvidia/GEAR-SONIC/tree/main/sonic_v1_1) | 20 ms aralıklı 10 gelecek kare, yaklaşık 200 ms referans önden bakışı | Robot yönelimine göre normalleştirilmiş hedef yönelimi kullanır ve bilek pozu çoğaltmasıyla (wrist-pose augmentation) eğitildi. Yönelimi kararlı tüm gövde teleoperasyonu ve bu kontrolcüyü kullanan SONIC destekli VLA politikaları için. G1 ve teleoperasyon gelecek referans gözlemleri `step5` kullanır; bu model düşük gecikmeli model ile aynı model değildir. |

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/sonic_v1_1_demo.gif" width="100%" alt="SONIC v1.1 tüm gövde teleoperasyon demosu">
</p>

*SONIC v1.1: anlatımlı bilek hareketi ve dinamik hareketle tüm gövde teleoperasyon kipi.*

Üç modelin hepsi SONIC evrensel belirteç (universal-token) kontrolcüsünü kullanır, 64 boyutlu gizil hareket belirteçleri üretir, kontrolcüyü 50 Hz'de çalıştırır ve SMPL pozu, G1 hareket referansı ve teleoperasyon girdilerini destekler. Dağıtım C++ ve TensorRT kullanır. PyTorch kontrol noktaları Isaac Lab değerlendirmesini ve eğitime devam etmeyi destekler.

> **Not:** Önden bakış değerleri, kontrolcüye sunulan referans ufkunu tanımlar. Bunlar algılama, ağ, ön işleme ve çıkarımı da içeren toplam teleoperasyon gecikmesinin ölçümü değildir.

## Yayımlanan dosyalar

| Model | Dağıtım dosyaları | PyTorch ve yapılandırma dosyaları |
|---|---|---|
| Varsayılan SONIC | `model_encoder.onnx`, `model_decoder.onnx`, `observation_config.yaml` | `sonic_release/last.pt`, `sonic_release/config.yaml` |
| Düşük gecikmeli teleoperasyon | `low_latency/model_encoder.onnx`, `low_latency/model_decoder.onnx`, `low_latency/observation_config.yaml` | `low_latency/last.pt`, `low_latency/config.yaml`, `low_latency/model_config.yaml` |
| SONIC v1.1 | `sonic_v1_1/model_encoder.onnx`, `sonic_v1_1/model_decoder.onnx`, `sonic_v1_1/observation_config.yaml` | `sonic_v1_1/last.pt`, `sonic_v1_1/config.yaml`, `sonic_v1_1/model_config.yaml` |

Kökteki [`config.json`](https://huggingface.co/nvidia/GEAR-SONIC/blob/main/config.json), sürümler ve ortak dosyalar için asıl sürüm bildirimidir (release manifest). Resmi indirici, model dosyalarını indirmeden önce bu dosyayı çeker ve doğrular.

Tüm dosyalar [`nvidia/GEAR-SONIC`](https://huggingface.co/nvidia/GEAR-SONIC) üzerinde barındırılır. Model ağırlıkları [NVIDIA Open Model License](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/resources/license.md) kapsamındadır.

## Model seçimi

İlk sürümü, mevcut dağıtım kurulumlarıyla en geniş uyumluluğu ya da standart hareket takibi ve planlama kontrolcüsünü istiyorsanız **Varsayılan SONIC** kullanın.

Akışla gelen SMPL, VR ya da VLA komutlarına tepki hızı önceliğinizse **Düşük gecikmeli teleoperasyon** modelini kullanın. Kısa referans ufku, komut verilen hareketin önden bakışını azaltır. Sistemin başka yerlerindeki gecikme ise olduğu gibi kalır.

Robot yönelimine göre normalleştirilmiş tüm gövde teleoperasyonu ya da bu kontrolcüye karşı eğitilmiş SONIC destekli bir VLA politikası için **SONIC v1.1** kullanın. Bu model 10 karelik SMPL ufkunu korur ve bilek pozu çoğaltmasıyla eğitildi.

## Kullanım

Hugging Face bağımlılığını depo kökünden kurun:

```bash
pip install huggingface_hub
```

### Varsayılan SONIC

```bash
python download_from_hf.py

cd gear_sonic_deploy
./deploy.sh --input-type zmq_manager real
```

### Düşük gecikmeli teleoperasyon

```bash
python download_from_hf.py --low-latency

cd gear_sonic_deploy
./deploy.sh \
    --cp policy/low_latency/model \
    --obs-config policy/low_latency/observation_config.yaml \
    --input-type zmq_manager \
    real
```

### SONIC v1.1

```bash
python download_from_hf.py --sonic-v1-1

cd gear_sonic_deploy
./deploy.sh \
    --cp policy/sonic_v1_1/model \
    --obs-config policy/sonic_v1_1/observation_config.yaml \
    --input-type zmq_manager \
    --motor-kp-scale 4,10=1.5 \
    --motor-kd-scale 4,10=1.5 \
    real
```

Kazanç bayrakları, v1.1 için test edilmiş donanım ayarıdır. `4` ve `10` motor indisleri sol ve sağ ayak bileği yunuslama (ankle-pitch) motorlarıdır. Ek sertlik, tüm gövde kararlılığını ve gözlenen bilek takibini iyileştirir. Ölçekleme isteğe bağlıdır.

### Python VLA başlatıcısı

Varsayılan model için:

```bash
python gear_sonic/scripts/launch_inference.py \
    --camera-host 192.168.123.164 \
    --prompt "pick up the cup"
```

Düşük gecikmeli model için:

```bash
python gear_sonic/scripts/launch_inference.py \
    --deploy-checkpoint policy/low_latency/model \
    --deploy-obs-config policy/low_latency/observation_config.yaml \
    --camera-host 192.168.123.164 \
    --prompt "pick up the cup"
```

SONIC v1.1 için yukarıdaki iki `policy/low_latency/` yolunu `policy/sonic_v1_1/` ile değiştirin ve şunları ekleyin:

```text
--deploy-motor-kp-scale 4,10=1.5
--deploy-motor-kd-scale 4,10=1.5
```

PyTorch kontrol noktası değerlendirmesi ve ek indirme seçenekleri için [Model Kontrol Noktalarını İndirme](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/getting_started/download_models.md) belgesine bakın.

## Sınırlamalar ve güvenlik

- "Düşük gecikmeli" adı, kontrolcünün referans önden bakışının azaltılmasını anlatır. Toplam sistem gecikmesi için bir kıyaslama ölçümü değildir.
- SONIC v1.1 düşük gecikmeli bir kontrol noktası değildir. 10 karelik SMPL referans ufkunu kullanır.
- Her ONNX kodlayıcısı ve kod çözücüsü, kendisiyle eşleşen gözlem yapılandırmasıyla kullanılmalıdır.
- Bu kontrol noktaları Unitree G1 gövdesini hedefler.
- Dağıtımdan önce simülasyonda test edin ve fiziksel robotu durdurmaya hazır bir güvenlik operatörü bulundurun.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
