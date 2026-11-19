> Bu belge NVlabs/GR00T-WholeBodyControl deposundaki `docs/source/getting_started/installation_deploy.md` dosyasının Türkçe çevirisidir (Apache-2.0, commit 1e1ec4f).

# Kurulum (Dağıtım)

## Ön koşullar

**Tüm kurulumlar için gerekli:**
- **Ubuntu 20.04/22.04/24.04** ya da Debian tabanlı başka bir Linux dağıtımı
- **CUDA Toolkit** (GPU hızlandırması için)
- **TensorRT** (çıkarım iyileştirmesi için). **Önce bunu kurun!**
- **Jetson AGX Orin üzerinde JetPack 6** (robot üstü dağıtım için)
- **Jetson AGX Thor üzerinde JetPack 7.1** (robot üstü dağıtım için)
- Python 3.8+
- LFS destekli Git

**TensorRT'yi** [NVIDIA Developer](https://developer.nvidia.com/tensorrt/download/10x) sayfasından **indirin**:

| Platform | TensorRT sürümü |
|---|---|
| x86_64 (masaüstü) | **10.13** (zorunlu) |
| Jetson / G1 üstündeki Orin | **10.7** (zorunlu; JetPack 6 gerektirir, [yükleme rehberi](JETPACK6-YUKLEME.md)) |
| Jetson / G1 üstündeki Thor | **10.13** (zorunlu) |

> **İpucu:** TensorRT'yi istediğiniz konuma açabilmek için **TAR** paketini (DEB paketini değil) indirin. Arşiv yaklaşık 10 GB'tır. İlerlemeyi izlemek için `pv` kullanabilirsiniz:

> **⚠️ Tehlike:** Yukarıda listelenen TensorRT sürümlerini **aynen** kullanmanız **şarttır**. Farklı bir sürüm kullanmanın yanlış çıkarım sonuçları ürettiği bilinir. Planlayıcı yanlış hareket çıkarır ve bu da tehlikeli robot davranışına yol açabilir.

```sh
sudo apt-get install -y pv
pv TensorRT-*.tar.gz | tar -xz -f -
```

Açılan TensorRT klasörünü `~/TensorRT` (ya da benzeri bir) konuma taşıyın ve `~/.bashrc` dosyanıza ekleyin:

```sh
export TensorRT_ROOT=$HOME/TensorRT
```

## Depoyu klonlama

```bash
git clone https://github.com/NVlabs/GR00T-WholeBodyControl.git
cd GR00T-WholeBodyControl
git lfs pull          # tüm büyük dosyaların çekildiğinden emin olun
```

## Kurulum adımları

### Yerel geliştirme (önerilen)

**Avantajları:** Doğrudan sisteme kurulum, daha hızlı derleme, üretime hazır.

> **Uyarı:** G1 üzerinde robot üstü dağıtım için, TensorRT'yi desteklemesi amacıyla robot üstündeki Orin'in JetPack 6'ya yükseltilmesi gerekir. Yükseltme için lütfen [yükleme rehberini](JETPACK6-YUKLEME.md) izleyin!
> AGX Thor, JetPack 7.1 gerektirir.

**Ön koşullar:**
- Temel geliştirme araçları (cmake, git vb.)
- (İsteğe bağlı) ROS2 tabanlı girdi/çıktı kullanmayı planlıyorsanız ROS2

**Kurulum adımları:**

1. **Sistem bağımlılıklarını kurun:**

```sh
cd gear_sonic_deploy
chmod +x scripts/install_deps.sh
./scripts/install_deps.sh
```

2. **Ortamı hazırlayın:**

```sh
source scripts/setup_env.sh
```

Hazırlık betiği otomatik olarak:
- TensorRT ortamını yapılandırır
- Gerekli tüm yolları ayarlar

Kolaylık için ortam hazırlığını kabuk profilinize ekleyebilirsiniz:

```sh
echo "source $(pwd)/scripts/setup_env.sh" >> ~/.bashrc
```

3. **Projeyi derleyin:**

```sh
just build
```

### Docker (ROS2 geliştirme ortamı)

x86_64 ve Jetson platformlarını destekleyen, ROS2 Humble içeren birleşik bir Docker ortamı sunuyoruz.

**Ön koşullar:**
- Docker kurulu ve kullanıcı docker grubuna eklenmiş
- x86_64 ana makinelerde `TensorRT_ROOT` ortam değişkeni ayarlı
- Jetson için: AGX Orin üzerinde JetPack 6.1+ ya da AGX Thor üzerinde JetPack 7.1

**Hızlı kurulum:**

```sh
# 1. Kullanıcıyı docker grubuna ekleyin (tek seferlik kurulum)
sudo usermod -aG docker $USER
newgrp docker

# 2. x86_64 üzerinde TensorRT yolunu ayarlayın (kalıcı olması için ~/.bashrc dosyasına ekleyin)
export TensorRT_ROOT=/path/to/TensorRT

# 3. Konteyneri başlatın
cd gear_sonic_deploy
./docker/run-ros2-dev.sh
```

**Seçenekler:**

```sh
./docker/run-ros2-dev.sh               # Standart derleme (hızlı)
./docker/run-ros2-dev.sh --rebuild     # Yeniden derlemeyi zorla
./docker/run-ros2-dev.sh --with-opengl # Görselleştirme için OpenGL ekle (RViz, Gazebo)
```

**Mimari desteği:**
- **x86_64**: CUDA 12.4.1 (NVIDIA sürücüsü 550+ gerektirir)
- **Jetson**: AGX Orin ve AGX Thor

**Konteynerin içinde:**

```sh
source scripts/setup_env.sh # bağımlılıkları hazırla
just build                  # Derle
just --list                 # Tüm komutları göster
```

**Sorun giderme:**
- "permission denied" hatası alırsanız docker grubunda olduğunuzdan emin olun
- x86_64 üzerinde `TensorRT_ROOT`, konteyner başlamadan önce **ana makinede** ayarlanmış olmalıdır
- Jetson için: önce ana makinede `source scripts/setup_env.sh` çalıştırın (jetson_clocks ayarını yapar)

---

Sonraki adım: [Hızlı Başlangıç](HIZLI-BASLANGIC.md) · Model indirme: [download_models.md (orijinal)](https://github.com/NVlabs/GR00T-WholeBodyControl/blob/main/docs/source/getting_started/download_models.md)

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
