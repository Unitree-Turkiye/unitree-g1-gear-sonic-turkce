> Bu belge NVlabs/GR00T-WholeBodyControl deposundaki `docs/source/getting_started/quickstart.md` dosyasının Türkçe çevirisidir (Apache-2.0, commit 1e1ec4f).

# Hızlı Başlangıç

SONIC ile dakikalar içinde başlayın!

> **Ön koşullar**
> 1. **[Kurulum Rehberi](KURULUM.md) tamamlanmış olmalı:** TensorRT kurulu, depo klonlanmış ve C++ dağıtımı derlenmiş.
> 2. **Model kontrol noktaları indirilmiş olmalı:** depo kökünde `python download_from_hf.py` komutunu çalıştırın. Ayrıntılar için [Model Kontrol Noktalarını İndirme](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/getting_started/download_models.md) belgesine bakın.

> **⚠️ Güvenlik uyarısı**
> Robotlar tehlikeli olabilir. Boş bir güvenlik alanı sağlayın, klavyenin başında acil durdurmayı tetiklemeye hazır bir güvenlik operatörü bulundurun ve bu yazılımı riski size ait olmak üzere kullanın. Yazarlar ve katkı verenler, bu projenin kullanımı ya da kötüye kullanımından doğan hiçbir hasar, yaralanma ya da kayıptan sorumlu değildir.

## Isaac Lab değerlendirmesi

Yayımlanmış PyTorch kontrol noktasını simülasyonda hızlıca sınamak için Isaac Lab'i kullanın. Bu komutları depo kökünden, Isaac Lab Python ortamınızın içinde çalıştırın.

Yalnız dağıtım için ONNX dosyalarını indirdiyseniz önce değerlendirme kontrol noktasını ve küçük örnek hareket kümesini çekin:

```sh
python download_from_hf.py --training --no-smpl
python download_from_hf.py --sample
```

Isaac Sim görüntüleyicisini açıp politikayı izlemek için:

```sh
python gear_sonic/eval_agent_trl.py \
    +checkpoint=sonic_release/last.pt \
    +headless=False \
    ++num_envs=1 \
    ++manager_env.observations.policy.enable_corruption=False \
    ++manager_env.observations.tokenizer.enable_corruption=False \
    "++manager_env.commands.motion.motion_lib_cfg.motion_file=sample_data/robot_filtered" \
    "++manager_env.commands.motion.motion_lib_cfg.smpl_motion_file=sample_data/smpl_filtered"
```

Görüntüleyiciyi incelerken bu komutu çalışır durumda bırakın, sonra `Ctrl+C` ile durdurun.

Hızlı bir ölçüm (metrik) çalıştırması için:

```sh
python gear_sonic/eval_agent_trl.py \
    +checkpoint=sonic_release/last.pt \
    +headless=True \
    ++eval_callbacks=im_eval \
    ++run_eval_loop=False \
    ++num_envs=128 \
    ++manager_env.observations.policy.enable_corruption=False \
    ++manager_env.observations.tokenizer.enable_corruption=False \
    "+manager_env/terminations=tracking/eval" \
    "++manager_env.commands.motion.motion_lib_cfg.max_unique_motions=512" \
    "++manager_env.commands.motion.motion_lib_cfg.motion_file=sample_data/robot_filtered" \
    "++manager_env.commands.motion.motion_lib_cfg.smpl_motion_file=sample_data/smpl_filtered"
```

Bunun yerine video kaydetmek için:

```sh
python gear_sonic/eval_agent_trl.py \
    +checkpoint=sonic_release/last.pt \
    +headless=True \
    ++eval_callbacks=im_eval \
    ++run_eval_loop=False \
    ++num_envs=8 \
    ++manager_env.config.render_results=True \
    "++manager_env.config.save_rendering_dir=/tmp/sonic_renders" \
    ++manager_env.config.env_spacing=10.0 \
    "~manager_env/recorders=empty" "+manager_env/recorders=render" \
    ++manager_env.observations.policy.enable_corruption=False \
    ++manager_env.observations.tokenizer.enable_corruption=False \
    "++manager_env.commands.motion.motion_lib_cfg.motion_file=sample_data/robot_filtered" \
    "++manager_env.commands.motion.motion_lib_cfg.smpl_motion_file=sample_data/smpl_filtered"
```

Videolar `/tmp/sonic_renders` klasörüne yazılır. Tüm veri setiyle değerlendirme ve beklenen ölçümler için [Eğitim Rehberi](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/user_guide/training.md#evaluation) belgesine bakın.

## MuJoCo'da Sim2Sim

🎬 Video: [MuJoCo'da sim2sim döngüsü (sim2sim.mp4)](https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/sim2sim.mp4)

MuJoCo simülatöründe test için simülasyon döngüsünü ve dağıtım betiğini ayrı terminallerde çalıştırın.

> **Not:** MuJoCo simülatörü (Terminal 1) **ana makinede**, bir Python sanal ortamında çalışır. Docker konteynerinin **içinde değildir**. Dağıtım ikili dosyası (Terminal 2) ana makinede yerel olarak ya da Docker konteynerinin içinde çalışabilir. Docker kullanıyorsanız Terminal 1'i ana makinede, Terminal 2'yi konteynerin içinde çalıştırın.

### Tek seferlik kurulum: MuJoCo simülasyon ortamını kurun

**Ana makinede** (Docker dışında), **depo kökünden** (`GR00T-WholeBodyControl/`) şunu çalıştırın:

```sh
bash install_scripts/install_mujoco_sim.sh
```

Bu komut, yalnız simülatörün ihtiyaç duyduğu paketleri (MuJoCo, Pinocchio, Unitree SDK2 vb.) içeren hafif bir `.venv_sim` sanal ortamı oluşturur.

### Sim2sim döngüsünü çalıştırma

Gerçek donanımda dağıtmadan önce bu süreci baştan sona uygulamanızı ve simülasyonda kontrollere alışmanızı önemle öneririz.

**Terminal 1: MuJoCo simülatörü** (ana makine, depo kökünden):

```sh
source .venv_sim/bin/activate
python gear_sonic/scripts/run_sim_loop.py
```

**Terminal 2: Dağıtım** (ana makine ya da Docker, `gear_sonic_deploy/` klasöründen):

```sh
bash deploy.sh sim
```

**Kontrolü başlatma:**

1. Terminal 2'de (deploy.sh) politikayı başlatmak için **`]`** tuşuna basın.
2. MuJoCo görüntüleyici penceresine tıklayın ve robotu yere indirmek için **`9`** tuşuna basın.
3. Terminal 2'ye dönün. Geçerli referans hareketi oynatmak için **`T`** tuşuna basın. Robot hareketi sonuna kadar uygular.
4. Sonraki ya da önceki hareket dizisine geçmek için **`N`** ya da **`P`** tuşuna basın.
5. Yeni hareketi oynatmak için yeniden **`T`** tuşuna basın.
6. Hareket bittiğinde aynı hareketi yeniden oynatmak için **`T`** tuşuna yine basabilirsiniz. Durup geçerli hareketin ilk karesine dönmek isterseniz hareketi baştan başlatmak için **`R`** tuşuna basın. Bu yolla politikayı sonlandırmadan hareketi durdurabilirsiniz.
7. İşiniz bittiğinde ya da **acil durdurma** gerektiğinde kontrolü durdurup çıkmak için **`O`** tuşuna basın.

Daha fazla kontrol için [Klavye](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/keyboard.md), [Oyun kumandası](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/gamepad.md), [ZMQ akışı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/zmq.md) ve [Arayüz yöneticisi](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/manager.md) eğitimlerine bakın.

## Gerçek robot

Gerçek G1 robotunda dağıtmak için şunu çalıştırın:

```sh
./deploy.sh real
```

## Çevrim içi görselleştirme

Görselleştiriciyi başlatın ve çalışan bir `g1_deploy` yürütülebilir dosyasına bağlayın:

```sh
python visualize_motion.py --realtime_debug_url tcp://localhost:5557
```

Notlar:
- Varsayılan port: 5557 (`--zmq-out-port <port>` ile değiştirin)
- Varsayılan konu (topic): `g1_debug` (yürütülebilir dosyada `--zmq-out-topic <topic>`, görselleştiricide `--realtime_debug_topic <topic>` ile değiştirin)
- Fiziksel robotlar için `localhost` yerine robotun IP adresini yazın

Çevrim dışı hareket CSV görselleştirmesi ve kayıt (log) ayrıntıları için [Dağıtım Kodu ve Program Akışı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/references/deployment_code.md) belgesine bakın.

İleri kullanım için [Klavye](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/keyboard.md), [Oyun kumandası](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/gamepad.md), [ZMQ akışı](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/zmq.md), [VR ile tüm gövde teleoperasyonu](VR-TUM-GOVDE-TELEOPERASYON.md) ve [Arayüz yöneticisi](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/tutorials/manager.md) eğitimlerine bakın.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
