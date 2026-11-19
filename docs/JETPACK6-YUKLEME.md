> Bu belge NVlabs/GR00T-WholeBodyControl deposundaki `docs/source/references/jetpack6.md` dosyasının Türkçe çevirisidir (Apache-2.0, commit 1e1ec4f).

# G1 JetPack 6 Yükleme (Flashing) Rehberi

*Çeviri notu: [Kurulum](KURULUM.md) belgesine göre G1 üstündeki Orin için TensorRT 10.7 ve JetPack 6 zorunludur. Bu rehber Orin NX modülünü JetPack 6.2'ye yükseltir.*

## 1. İmajı indirin

1. **Gerekli dosyaları indirin:** hem `.tar` dosyasını hem de imaj dosyasını [Jetpack 6.2](https://drive.google.com/drive/folders/1ho17ectOxi7FbaRFdpAbP4tet8BJWjbm) klasöründen alın.

## 2. Orin NX'in NVMe diskini çıkarın

### NVMe SSD'yi çıkarma adımları

1. **Arka tutamak vidalarını sökün**

   Robotun arkasında, tutamağın yakınındaki iki vidayı 5 mm T saplı alyan anahtarıyla sökün.

2. **Köpük ve plastik arka kapağı çıkarın**

   - Köpük ve plastik arka kapağı tutan dört vidayı Fanttik alet setindeki 2 mm alyan uçla sökün.
   - İç bileşenler görünsün diye kapağı kaldırın.

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/screws.png" width="600" alt="Unitree G1 arka kapak vidaları">
</p>

3. **Orin NX modülündeki NVMe vidasını sökün**

   Orin NX'in NVMe SSD'sini sabitleyen tek vidayı yıldız tornavidayla (Fanttik alet setinde de bulunur) sökün.

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/ssd.png" width="600" alt="Unitree G1 içindeki Orin NX NVMe SSD">
</p>

4. **SSD kartını çıkarın**

   NVMe SSD'yi yuvasından dikkatle kaydırarak çıkarın.

## 3. NVMe SSD'ye imajı yazın

**Orin NX'ten çıkan NVMe SSD'yi NVMe SSD kutu adaptörüne takın.**
(İmajı dizüstü bilgisayardan yazarken adaptör gerekir)

1. Robotun SSD'sinin bağlı (mount) olmadığını denetleyin. İmajı yazacağınız harici SSD'nin bağlı olmadığından emin olmak için şu komutu çalıştırın:

```bash
sudo umount /dev/sda*
```

2. SSD bağlıysa bu komut onu güvenle ayırır ve imaj yazmaya hazırlar.

3. İmajın bulunduğu klasöre geçin (`cd robot_NXUpgrade/`) ve şu komutu çalıştırın:

```bash
bzip2 -dc g1-nx-j6.2.img.bz2 | sudo dd of=/dev/sda bs=4M status=progress conv=fsync
```

4. İşlem bitince kartı güvenle çıkarmak için şu komutları kullanın:

```bash
sudo sync
sudo udisksctl power-off -b /dev/sda
```

5. **SSD kartını bir kenara koyun ve yükleme sürecinin ikinci bölümüne geçin!**

## 4. Robotu yükleme kipine alın

1. **G1'i açın** ve üç güç gösterge ışığının hepsi sabit yanana kadar bekleyin.

2. **Robotu dizüstü ya da masaüstü bilgisayarınıza** USB-C kabloyla bağlayın.

3. Robottaki **iki beyaz düğmeye aynı anda basıp** iki saniye basılı tutun.

4. Basılı tutmaya devam ederken **üstteki beyaz düğmeyi bırakın** ve **üç yeşil ışık iki yeşil ışığa dönene** kadar **alttaki düğmeyi** 2 saniye basılı tutmayı sürdürün.

<p align="center">
  <img src="https://raw.githubusercontent.com/NVlabs/GR00T-WholeBodyControl/main/docs/source/_static/flashing.png" width="600" alt="Unitree G1 yükleme kipi düğmeleri ve gösterge ışıkları">
</p>

5. Yalnız iki ışık yandığında robot **artık yükleme kipindedir**. Bilgisayarınızda yeni bir terminal açın ve `lsusb` yazın. Çıktıda `NVIDIA Corp. APX` içeren bir satır görmelisiniz.

6. Artık şu komutları çalıştırabilirsiniz:

```bash
sudo tar -xjvf Jetpack_6.2_nx.tar.bz2
cd Jetpack_6.2_nx/Linux_for_Tegra
sudo ./flash_nx_module.sh
```

Başarı mesajı görünene kadar yaklaşık 8 dakika sabırla bekleyin.

## 5. Robotu yeniden toplayın

1. Yükleme bitince **robotu kapatın**.

2. **Orin NX'in NVMe SSD'sini** G1 robotundaki yuvasına yeniden takın ve vidasıyla sabitleyin.

3. **Köpük ve plastik arka kapağı** söktüğünüz aletlerle yeniden takın.

4. Arka kapağın ve tutamağın sağlam oturması için **tüm vidaları sıkın**.

5. Jetson Orin üzerinde `maxn` kipini şu komutla açın:

```
sudo nvpmodel -m 0
```

ve Maxn kipinde olup olmadığını denetlemek için şunları kullanın:

```
sudo jetson_clocks
sudo jetson_clocks --show
```

## 6. Gerekli JetPack paketlerini kurun

Dağıtım için gereken paketleri kurun:

```
sudo apt-get install -y nvidia-l4t-dla-compiler libcudla-dev-12-6
```

---

Sonraki adım: [Kurulum](KURULUM.md) · [Hızlı Başlangıç](HIZLI-BASLANGIC.md) · Robot üstü hesaplama için [Jetson Orin rehberi](https://www.robotlar.org/tr/rehber/saha-robotu-jetson-orin-dayanikli-bilgisayar)

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
