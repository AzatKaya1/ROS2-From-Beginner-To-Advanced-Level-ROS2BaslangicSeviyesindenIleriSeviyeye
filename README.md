# ROS2 Başlangıç Seviyesinden İleri Seviyeye
(ROS2 From Beginner To Advanced Level)




##  **ROS2 AKIŞ ŞEMASI (ROS2 FLOW CHART)** 


**Workspace** → **Package** → **URDF (Unified Robot Description Format) (Robot Tanımı)** → Node’s → **Launch** → **Build** → **Source** → Run

----

 1. Çalışma Alanı (Workspace) Oluşturma 
 2. Paket Oluşturma 
 3. URDF (Unified Robot Description Format) Yazımı 
 4. Launch Dosyaları 
 5. Derleme Aşaması  
 6. Kaynak Gösterme

<br>

 🧱 **Çalışma Alanı (Workspace) Oluşturma** (mkdir -p ~/ros2_ws/src)

🔹​​ROS 2'de kodlar rastgele klasörlere yazılmaz. Sistemin derleme yapabilmesi için özel bir hiyerarşi gerekir.

🔹​​ ros2_ws senin fabrikan, src ise bu fabrikanın içindeki "üretim bandı"dır. Tüm ham kodlar (yazdığın Python/C++ dosyaları) sadece src içine girer.

<br>

 📦 **Paket Oluşturma** (ros2 pkg create)

🔹​​ ROS 2 devasa bir kütüphanedir. Her şeyi tek bir dosyaya yazamazsın. Yazılımı modüllere (paketlere) ayırman gerekir.

🔹​​ "Robotun Tasarımı" bir pakettir, "Kamera İşleme" başka bir pakettir.

<br>

 🤖  **URDF (Unified Robot Description Format) Yazımı**
  
🔹​​ Bilgisayar, senin robotunun bir kutu mu yoksa örümcek mi olduğunu bilmez. URDF, robotun "iskelet sistemidir".

🔹​​ **link** ile robotun parçalarını (gövde, tekerlek), **joint** ile bu parçaların birbirine nasıl bağlandığını (tekerlek gövdeye döner eklemle bağlıdır gibi) XML formatında anlatırsın.

<br>

 🚀  **Launch Dosyaları** (launch.py)
  
🔹​​ Robotu çalıştırmak için bazen 10 tane terminal açıp 10 farklı komut yazman gerekir (RViz aç, Robot State Publisher aç, Gazebo aç...).

🔹​​ Launch dosyası "Tek Tuşla Başlat" düğmesidir. İçine hangi programların (düğümlerin) hangi ayarlarla açılacağını yazarsın, tek komutla hepsini ayağa kaldırırsın.

<br>

 🔨  **Derleme Aşaması** (colcon build)
  
🔹​​ Sen kodu src içine yazdın ama bilgisayar o ham dosyaları doğrudan okumaz. Onları sistemin anlayacağı "çalıştırılabilir" dosyalara dönüştürmesi gerekir.

🔹​​ colcon build komutu src içindeki kodları alır, build ve install klasörlerini oluşturur. Artık robotun "kuruluma hazır" bir yazılım haline gelir.

<br>

 ⚡  **Kaynak Gösterme** (source install/setup.bash)
  
🔹​​ Terminaline "Hey, ben yeni bir paket yaptım, artık ros2 run dediğimde bunu bulabilirsin" demen gerekir.

🔹​​ Bu komut, o terminalin hafızasına senin yeni paketinin yolunu (path) ekler. Bunu yapmazsan "Package not found" hatası alırsın.

<br>
<br>

◼️ ROS2 projeleri workspace içinde çalışır. Bu bir nevi “proje klasörü”.

İç yapı:
```sh

ros2_ws/
 ├── src/        ← kodların burada (package'ler)
 ├── build/      ← derleme çıktıları
 ├── install/    ← ROS’un çalıştırdığı yer
 └─ log/

```
<br>

◼️ Paket (Package) Oluşturma
  
  Example: **ros2 pkg create** --build-type ament_cmake my_robot_description
  
  ROS2’de her şey package içinde olur.
  Bu paketler robotun tarifini tutar. (URDF)

  ament_cmake = ROS2’nin build sistemidir. C++ projelerde yaygındır.


  ament_python = (Python)
  
<br>

◼️ URDF (Robot Tanımı)
```sh
mkdir urdf
nano urdf/my_robot.urdf
```
ROS robotu fiziksel olarak tanımaz. Biz tanıtırız.


<br>













