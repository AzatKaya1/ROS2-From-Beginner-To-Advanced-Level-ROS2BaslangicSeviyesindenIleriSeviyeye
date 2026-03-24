![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420?logo=ubuntu&logoColor=white)
![ROS2](https://img.shields.io/badge/ROS2-Humble-blue?logo=ros)
# ROS2 Başlangıç Seviyesi
(ROS2 Beginner Level)

Bu kılavuz ROS2 hakkında adım adım bilinmesi gereken temel kavramları, kullanım süreçlerini ve sık yapılan hataları kapsar. Öğrenirken başvurabileceğin bir referans olarak tasarlandı.

**İçindekiler**

- [Ön Koşullar](#ön-koşullar-prerequisites)
- [ROS2 Akış Şeması](#ros2-akış-şeması-ros2-flow-chart)
- [ROS2 Genel Mantık](#ros2-genel-mantık)
- [Kavram Tanımları](#kavram-tanımları)
- [Adım Adım Kurulum](#adım-adım-kurulum)
  - [1. Workspace Oluşturma](#1-workspace-çalışma-alanı)
  - [2. Paket Oluşturma](#2-paket-package-oluşturma)
  - [3. URDF Yazımı](#3-urdf-robot-tanımı)
  - [4. Launch Dosyası](#4-launch-dosyası)
  - [5. Build (Derleme)](#5-build-derleme)
  - [6. Source](#6-source-kaynak-gösterme)
  - [7. Çalıştırma](#7-çalıştırma-run)
- [Sık Yapılan Bazı Hatalar](#sık-yapılan-bazı-hatalar)

## Ön Koşullar (Prerequisites)

OS: Tercihen **Ubuntu 22.04** 

ROS2 Versiyonu: Tercihen **ROS2 Humble**


##  **ROS2 Akış Şeması (ROS2 Flow Chart)** 

```plaintext
Workspace → Package → URDF (Unified Robot Description Format) (Robot Tanımı) → Nodes → Topics → Launch → Build → Source → Run
```
Bir ROS2 projesinde her zaman bu sıra geçerlidir. **Adımları atlama!**

## ROS2 Genel Mantık

**Node (Düğüm) Nedir?**

ROS2'de çalışan her bir program **Node**'dur. Kamera, lidar, robot kontrolü, motor kontrolü... Bunların her biri ayrı Node'dur. Burada ROS2, Node'ların birbirleriyle konuştuğu sistemdir. İyi bir Node **tek bir şey** gerçekleştirir!

Node listeleme:
```bash
ros2 node list
```

Node'dan info alma:
```bash
ros2 node info /node_name
```

**Topic(Konu) Nedir?**

Node'lar birbirine **doğrudan bağlanamaz.** Aralarında **Topic** adı verilen kanal olmalıdır.

Publisher → Topic → Subscriber

Publisher veri gönderir. Topic verinin gittiği kanaldır. Subscriber ise veriyi alır.

Örnek:

```plaintext
Lidar Node ──► /scan ──► RViz 
Lidar Node ──► /scan ──► Navigation Node
Camera Node ──► /image ──► AI Node

```
• Lidar Node = Publisher

• /scan = Topic

• RViz = Subscriber

Topic promptları:

```bash
ros2 topic list
ros2 topic echo /scan
ros2 topic info /scan
```

[Ubuntu22.04 ROS2Humble Kurulum](https://github.com/AzatKaya1/Ubuntu22.04-ROS2Humble-ArduPilotwSITL-Gazebo-SetupGuide)

Yukarıdaki kurulumu yaptıktan sonra Node - Topic mantığını daha rahat kavraman için aşağıdaki örneği yapabilirsin.

Önce yeni bir terminal açıp ortamı source ediyoruz. Daha sonra terminale C++ dilinde konuşma emri veriyoruz.

**Önemli Not: Terminale komutları her zaman satır satır yazın!** Aksi halde ileriki zamanlarda sıkıntı yaşarsınız.

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

Yukarıdaki promptu **satır satır** terminale yazdıktan sonra, yeni bir terminal açıp python dilinde dinleyici promptu yazıyoruz.
```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```

ve **BOOOM!** C++ dilinde konuşturduğumuz terminalde, Python kodlama diliyle başka bir terminaldeki mesajı dinleyebiliyoruz. :) Buradan çıkarılması gereken ders; ROS bize dil fark etmeksizin, haberleşme imkanı sunar.

<br>


## Kavram Tanımları


 🧱 **Çalışma Alanı (Workspace) Oluşturma** (mkdir -p ~/ros2_ws/src)

 • ​​ROS 2'de kodlar rastgele klasörlere yazılmaz. Sistemin derleme yapabilmesi için özel bir hiyerarşi gerekir.

 • ros2_ws senin fabrikan, src ise bu fabrikanın içindeki "üretim bandı"dır. Tüm ham kodlar (yazdığın Python/C++ dosyaları) sadece src içine girer.

<br>

 📦 **Paket Oluşturma** (ros2 pkg create)

 • ROS 2 devasa bir kütüphanedir. Her şeyi tek bir dosyaya yazamazsın. Yazılımı modüllere (paketlere) ayırman gerekir.

 • **"Robotun Tasarımı"** bir pakettir, **"Kamera İşleme"** başka bir pakettir.

<br>

 🤖  **URDF (Unified Robot Description Format) Yazımı**
  
 • Bilgisayar, senin robotunun bir kutu mu yoksa örümcek mi olduğunu bilmez. URDF, robotun **"iskelet sistemidir".**

 • **link** ile robotun parçalarını (gövde, tekerlek), **joint** ile bu parçaların birbirine nasıl bağlandığını (tekerlek gövdeye döner eklemle bağlıdır gibi) **XML** formatında anlatırsın.

<br>

 🚀  **Launch Dosyaları** (launch.py)
  
 • Robotu çalıştırmak için bazen 10 tane terminal açıp 10 farklı komut yazman gerekir (RViz aç, Robot State Publisher aç, Gazebo aç...).

 • Launch dosyası **"Tek Tuşla Başlat"** düğmesidir. İçine hangi programların (düğümlerin) hangi ayarlarla açılacağını yazarsın, **tek komutla** hepsini ayağa kaldırırsın.

<br>

 🔨  **Derleme Aşaması** (colcon build)
  
 • Sen kodu src içine yazdın ama bilgisayar o ham dosyaları doğrudan okumaz. Onları sistemin anlayacağı **"çalıştırılabilir"** dosyalara dönüştürmesi gerekir.

 • **colcon build** komutu src içindeki kodları alır, build ve install klasörlerini oluşturur. Artık robotun "kuruluma hazır" bir yazılım haline gelir.

<br>

 ⚡  **Kaynak Gösterme** (source install/setup.bash)
  
 • Terminaline **"Hey, ben yeni bir paket yaptım, artık ros2 run dediğimde bunu bulabilirsin"** demen gerekir.

 • Bu komut, o terminalin hafızasına senin yeni paketinin yolunu (path) ekler. Bunu yapmazsan **"Package not found"** hatası alırsın.

<br>


# Adım Adım Kurulum

 1. Çalışma Alanı (Workspace) Oluşturma 
 2. Paket Oluşturma 
 3. URDF (Unified Robot Description Format) Yazımı 
 4. Launch Dosyaları 
 5. Derleme Aşaması  
 6. Kaynak Gösterme
 7. Çalıştırma (Run)

## 1. Workspace (Çalışma Alanı)

ROS2 projeleri workspace içinde çalışır. Bu bir nevi proje klasörüdür. Her projede sadece **bir kez** oluşturulur.

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src

```

Derleme aracı olan **colcon** bu hiyerarşiyi arar:
```plaintext

ros2_ws/
 ├── src/        ← kodların burada (package'ler)
 ├── build/      ← derleme çıktıları (otomatik oluşur)
 ├── install/    ← ROS2'nin çalıştırdığı yer (otomatik oluşur)
 └── log/        ← derleme kayıtları (otomatik oluşur)
```
Unutma! **colcon build** komutu src içindeki kodları alır, build ve install klasörlerini oluşturur. 

## 2. Paket (Package) Oluşturma
  
  Her yeni modül (kamera işleme, robot tasarımı, navigasyon) için ayrı bir paket oluşturulur. Bu paketler robotun tarifini    tutar. (URDF)
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_cmake my_robot_description
```
ROS2'de her şey paket içinde yaşar. Büyük projelerde onlarca paket olabilir. Her paket birbirinden bağımsızdır.

  - ``` ament_cmake ``` = ROS2’nin build sistemidir. C++ projelerde yaygındır.


  - ``` ament_python ``` = (Python)
  
Paket oluşturulduğunda şu dosyalar gelir:

```plaintext

my_robot_description/
 ├── CMakeLists.txt        ← derleme tarifi (neyi derle, nereye koy)
 ├── package.xml           ← paketin kimlik kartı (ad, bağımlılıklar, yazar)
 └── include/ & src/       ← C++ için kaynak klasörleri

```
``` package.xml```  çok önemlidir! Paketin hangi diğer paketlere bağımlı olduğunu buraya yazarsın. ```colcon``` bağımlılık sırasını buradan çözer.


## 3. URDF (Robot Tanımı)

URDF'i bir kez yazarız. Daha sonra üstüne ekleyerek büyütürüz.

```bash
cd ~/ros2_ws/src/my_robot_description
mkdir urdf
nano urdf/my_robot.urdf
```

ROS robotu fiziksel olarak tanımaz. Biz tanıtırız. Unutma! URDF robotun iskelet sistemidir.

<br>

URDF Nedir?

Robotun:
- Şekli
- Boyutu
- Parçaları (kemik) (link)
- Bağlantıları (eklem) (joint)

**link** = fiziksel parça

**joint** = parçaları bağlayan şey

Basit bir URDF örneği: (**XML Format**)
```xml
<?xml version="1.0"?>
<robot name="simple_robot">
  <link name="base_link">
    <visual>
      <geometry>
        <box size="0.5 0.3 0.1"/>
      </geometry>
      <material name="blue">
        <color rgba="0 0 1 1"/>
      </material>
    </visual>
  </link>
</robot>
```

**"Yazdığınız URDF'i kontrol etmek için** ``` check_urdf urdf/dosya_adı ``` **komutunu kullanabilirsiniz."**

**Not:** Büyük projelerde URDF **xacro** ile yazılır. Xacro değişken ve makro desteği sunar. Aynı kodu 4 tekerlek için 4 kez yazmak zorunda kalmayız.

## 4. Launch Dosyası

Her proje için bir launch dosyası yazılır. Zamanla birden fazla launch dosyan olabilir (simülasyon için, gerçek robot için, test için ayrı ayrı).

```bash
mkdir launch
nano launch/display.launch.py
```

ROS'ta tek tek komut yazmak yerine tek tuşla her şeyi başlatma amacı güdüyoruz. Bu yüzden Launch dosyaları kullanılır.

Örnek launch dosyası:

```python
from launch import LaunchDescription
from launch_ros.actions import Node
import os
from ament_index_python.packages import get_package_share_directory

def generate_launch_description():
    pkg_path = get_package_share_directory('my_robot_description')
    urdf_file = os.path.join(pkg_path, 'urdf', 'my_robot.urdf')

    with open(urdf_file, 'r') as f:
        robot_desc = f.read()

    return LaunchDescription([
        Node(
            package='robot_state_publisher',
            executable='robot_state_publisher',
            parameters=[{'robot_description': robot_desc}]
        ),
        Node(
            package='rviz2',
            executable='rviz2',
            name='rviz2'
        )
    ])
```

CMakeLists.txt'e eklenmesi zorunlu satırlar:

```cmake
install(DIRECTORY urdf launch
  DESTINATION share/${PROJECT_NAME}
)
```

**Bu satırlar olmadan** ``` colcon build ``` **dosyaları kopyalamaz ve** ``` get_package_share_directory() ``` **çalışmaz!**



## 5. Build (Derleme)
ROS2 yazdığın dosyaları doğrudan çalıştıramaz. ``` colcon build  ``` onları ``` install/  ```  klasörüne işlenmiş hâlde koyar. Ayrıca build işlemini her zaman **workspace kökünden** çalıştır.

```bash
cd ~/ros2_ws
colcon build --symlink-install
```

``` colcon ``` = ROS2'nin build tool'u.

``` --symlink-install ``` = Python ve URDF dosyaları için sembolik link oluşturur. Dosyayı değiştirince tekrar build gerekmez. **Geliştirme sürecinde hep kullanılır.**

``` --packages-select paket_adi ``` = Sadece **belirtilen paketi** derler. Büyük projelerde zaman kazandırır!


## 6. Source (Kaynak Gösterme)
Bu adımı her yeni terminal açtığında tekrarlamamız gerekir:
```bash
source install/setup.bash
```
Çünkü ``` colcon build ``` paketi sisteme **kaydetmez.** Sadece ``` install/ ``` klasörüne koyar. ``` source ``` komutu ise o klasörü terminale tanıtır. Yapmazsan **ROS2 paketini görmez!**

ROS'ta oluşturduğumuz workspace'i kendi sistemine kaynak etmesi gerektiğini belirtiyoruz(Benim workspace'im de artık sistemin bir parçası).

Eğer ki bu işlemi her terminal açtığımızda yazmak istemiyorsak çok pratik bir yöntem olan ``` .bashrc ``` 'e eklememiz lazım.

```bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Artık her yeni terminalde ortam otomatik olarak **source** olacaktır!

## 7. Çalıştırma (Run)

Örnek komut:
```bash
ros2 launch my_robot_description display.launch.py
```

## Sık Yapılan Bazı Hatalar


| Hata | Neden Olur | Çözüm |
| :--- | :--- | :--- |
| `Package 'xyz' not found` | `source` yapılmadı | `source install/setup.bash` çalıştır |
| `colcon` komutu bulunamadı | Workspace kökünde değilsin | `cd ~/ros2_ws` yap ve terminali yenile |
| RViz'de hiçbir şey görünmüyor | Fixed Frame yanlış seçili | Fixed Frame'i `base_link` veya `map` yap |
| `Could not find transform` | TF (Transform) ağacı kopuk | `ros2 run tf2_tools view_frames` ile kontrol et |
| URDF dosyası bulunamıyor | CMakeLists.txt'e `install()` eksik | `install(DIRECTORY urdf launch ...)` satırını ekle |
| C++ değişikliği yansımıyor | `--symlink-install` C++'ı kapsamaz | Her değişiklikte `colcon build` yap |
| `Permission denied` (USB/Serial) | Port erişim yetkisi yok | `sudo chmod 666 /dev/ttyUSB0` (veya ilgili port) |
| Düğümler (Nodes) birbirini görmüyor | `ROS_DOMAIN_ID` uyumsuzluğu | `export ROS_DOMAIN_ID=0` (Her terminalde aynı olmalı) |
| Python dosyası çalışmıyor | Çalıştırma yetkisi (executable) yok | `chmod +x dosya_adi.py` yap ve `setup.py` kontrol et |
| `Missing dependencies` | Paket bağımlılıkları eksik | `rosdep install -i --from-path src --rosdistro $ROS_DISTRO -y` |





**UNUTMA!**

ROS Ezberlenmez → **kurulur + bozulur + düzeltilir**



Soru ve geri bildirimler için:

<div align="center">
  <a href="https://linkedin.com/in/azatkaya1">
    <img src="https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn">
  </a>

