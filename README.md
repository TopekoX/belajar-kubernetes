# Belajar Kubernetes

## 1️⃣ Apa itu Kubernetes

**Kubernetes** (sering disingkat **K8s**) adalah platform open-source yang digunakan untuk orkestrasi kontainer. 

Bayangkan aplikasi Anda adalah kumpulan kotak-kotak (kontainer) yang berisi kode. Jika Anda hanya punya satu kotak, mengaturnya mudah. Namun, jika Anda punya ribuan kotak di berbagai server, Anda butuh "kapten kapal" untuk mengaturnya—itulah Kubernetes (kata Yunani untuk "nakhoda" atau "kapten"). 

### Apa Fungsinya?
Kubernetes bekerja seperti konduktor orkestra yang memastikan setiap instrumen (kontainer) bermain pada waktu yang tepat dan dengan volume yang pas. Fungsi utamanya meliputi: 

* **Otomatisasi Deployment**: Memasang dan memperbarui aplikasi secara otomatis tanpa menghentikan layanan (zero downtime).
* **Self-Healing**: Jika ada kontainer yang rusak atau mati, Kubernetes akan mendeteksi dan menggantinya secara otomatis agar aplikasi tetap berjalan.
* **Auto-Scaling**: Menambah jumlah kontainer saat pengunjung aplikasi melonjak dan menguranginya saat sepi agar hemat biaya.
* **Load Balancing**: Membagi trafik masuk secara merata ke berbagai kontainer supaya tidak ada server yang keberatan beban. 

### Mengapa Kubernetes Populer?

Platform ini awalnya dikembangkan oleh Google dan kini dikelola oleh Cloud Native Computing Foundation (CNCF). Kubernetes menjadi standar industri karena kemampuannya menjaga aplikasi tetap stabil meski dijalankan di infrastruktur yang sangat besar, baik di cloud (seperti AWS, Google Cloud, Azure) maupun di server lokal. 

Untuk mulai mencoba, Anda bisa mengikuti Panduan Dasar di Situs Resmi Kubernetes yang tersedia dalam Bahasa Indonesia. 

## 2️⃣ Arsitektur Kubernetes

Arsitektur Kubernetes (K8s) dirancang sebagai sistem terdistribusi yang terdiri dari dua bagian utama: **Control Plane** (Master Node) dan **Worker Node**. Keduanya bekerja sama untuk memastikan aplikasi berjalan sesuai konfigurasi yang diinginkan. 

![Kubernetes](https://kubernetes.io/images/docs/kubernetes-cluster-architecture.svg)

## 1. Control Plane (Otak Klaster)
Control Plane bertanggung jawab untuk membuat keputusan global tentang klaster (seperti penjadwalan) serta mendeteksi dan menanggapi kejadian di klaster. Komponen utamanya meliputi: 

* __kube-apiserver__: Pintu masuk utama untuk semua perintah. Semua komponen berkomunikasi melalui API ini.
* __etcd__: Penyimpanan data _key-value_ yang konsisten dan memiliki ketersediaan tinggi, berfungsi sebagai basis data utama untuk semua data klaster.
* __kube-scheduler__: Bertugas memilih node mana yang paling cocok untuk menjalankan Pod baru berdasarkan ketersediaan sumber daya.
* __kube-controller-manager__: Menjalankan proses kontroler yang menjaga agar status klaster tetap sesuai dengan yang diinginkan (misalnya, mengganti Pod yang mati). 

### 2. Worker Node (Pekerja Klaster)

Node ini adalah mesin (VM atau fisik) tempat aplikasi Anda benar-benar berjalan. Komponen di dalamnya meliputi: 

* __kubelet__: Agen yang berjalan di setiap node untuk memastikan kontainer di dalam Pod berjalan dengan sehat.
* __kube-proxy__: Komponen jaringan yang mengatur aturan trafik sehingga komunikasi antar Pod atau dari luar klaster bisa terjadi.
* __Container Runtime__: Perangkat lunak yang menjalankan kontainer (seperti Docker, containerd, atau CRI-O).
* __Pods__: Unit terkecil di Kubernetes yang berisi satu atau lebih kontainer. 

### Cara Kerja Sederhana

* __User__ mengirim perintah melalui `kubectl` ke __API Server__.
* __API Server__ menyimpan status baru ke __etcd__.
* __Scheduler__ melihat ada Pod baru dan menentukan __Worker Node__ yang kosong.
* __Kubelet__ di node terpilih menerima instruksi dan memerintahkan __Container Runtime__ untuk menyalakan kontainer.

## 3️⃣ Cara Instal Kubernetes

Cara menginstal Kubernetes sangat bergantung pada kebutuhan Anda, apakah untuk belajar (lokal) atau untuk produksi (server). Berikut adalah tiga metode yang paling umum digunakan: 

### 1. Untuk Pemula & Belajar (Lokal)

Jika Anda ingin mencoba Kubernetes di laptop sendiri, gunakan tool yang ringan dan cepat.

* __Minikube__: Tool paling populer untuk membuat klaster satu node di mesin lokal.
    1. Unduh Minikube sesuai OS Anda (Windows/macOS/Linux).
    2. Pastikan Anda memiliki driver seperti Docker, VirtualBox, atau Hyper-V.
    3. Jalankan perintah: `minikube start`.
* __Docker Desktop__: Jika Anda sudah menggunakan Docker di Windows atau Mac, Anda cukup mengaktifkan fitur Kubernetes di menu __Settings > Kubernetes > Enable Kubernetes__.
* __MicroK8s__: Sangat ringan dan cocok untuk Ubuntu/Linux, diinstal cukup dengan perintah `snap install microk8s --classic`. 

### 2. Untuk Lingkungan Produksi (Manual/Bare Metal)

Jika Anda ingin membangun klaster di server asli atau VPS, gunakan alat standar komunitas:
* __Kubeadm__: Tool resmi untuk membangun klaster yang sesuai dengan standar industri.
    * **Persiapan**: Siapkan minimal 2 server (Master & Worker) dengan OS Linux (seperti Ubuntu 22.04), minimal 2 CPU, dan 2 GB RAM.
    * **Langkah Utama**:
        * Matikan Swap di semua node (`sudo swapoff -a`).
        * Instal __Container Runtime__ (seperti `containerd` atau Docker).
        * Instal alat utama: `kubeadm`, `kubelet`, dan `kubectl`.
        * Inisialisasi Master Node: `kubeadm init`.
        * Hubungkan Worker Node dengan perintah `kubeadm join` yang dihasilkan oleh Master. 

### 3. Menggunakan Layanan Cloud (Managed Kubernetes)

Cara termudah untuk skala besar tanpa pusing mengatur server Master adalah menggunakan layanan dari penyedia cloud: 

* **Google Cloud (GKE)**: Instal Google Cloud SDK dan jalankan `gcloud container clusters create [NAMA_KLASTER]`.
* **AWS (EKS)**: Menggunakan `eksctl` untuk membuat klaster secara otomatis.
* **Azure (AKS)**: Menggunakan Azure CLI atau portal untuk deployment cepat.

> Pada materi ini menggunakan lokal komputer dengan menggunakan __minikube__.

Setelah instalasi selesai, Anda wajib menginstal [kubectl](https://kubernetes.io/id/docs/tasks/tools/install-kubectl/) (Kubernetes CLI) untuk berinteraksi dengan klaster Anda. 

Cek version `kubectl version`:

```bash
ucup@Timposu:~$ kubectl version

Client Version: v1.35.2
Kustomize Version: v5.7.1
Server Version: v1.35.1
```

## 4️⃣ Node

Dalam ekosistem Kubernetes, Node adalah sebuah mesin (baik itu server fisik maupun mesin virtual/VM) tempat aplikasi Anda benar-benar dijalankan.
Jika Kubernetes adalah sebuah kapal besar, maka Node adalah ruang mesin atau dek tempat kargo (kontainer) diletakkan. Dokumentasi Resmi Kubernetes menjelaskan bahwa Node dikelola oleh Control Plane untuk memastikan aplikasi tetap berjalan.

Secara umum seperti yang sudah dibahas sebelumnya, ada dua jenis **Node Master Node (Control Plane)** dan **Worker Node**.

### Komponen di Dalam Sebuah Node
Setiap Worker Node memiliki tiga komponen utama agar bisa berkomunikasi dengan "otak" pusat:
* __kubelet__: Agen kecil yang memastikan kontainer berjalan sesuai perintah Master.
* __kube-proxy__: Pengatur lalu lintas jaringan agar aplikasi bisa diakses.
* __Container Runtime__: Mesin yang menjalankan kontainer (biasanya Docker atau containerd).

> Karena kita menggunakan Minikube, laptop kita bertindak sebagai satu-satunya node yang merangkap fungsi Master sekaligus Worker (klaster satu-node).

### Perintah Dasar Node

#### Melihat Daftar Node

Perintah ini digunakan untuk mengecek apakah Node Anda sudah aktif dan siap (Ready).

```bash
kubectl get nodes
```

> Tips: Tambahkan `-o wide` untuk melihat informasi lebih detail seperti alamat IP dan versi kernel: `kubectl get nodes -o wide`.

Karena menggunakan Minikube, Anda bisa melihat Node tunggal Anda dengan mengetik `kubectl get nodes`.

#### Melihat Detail Informasi Node

Jika terjadi error pada Node, gunakan perintah ini untuk melihat log kejadian, kapasitas CPU/RAM, dan status kesehatannya.

```bash
kubectl describe node <nama-node>
```

(Contoh: `kubectl describe node minikube`)

#### Melihat Penggunaan Sumber Daya (Resource)

Untuk melihat berapa banyak CPU dan Memori yang sedang digunakan oleh Node secara real-time:

```bash
kubectl top node
```

Catatan: Perintah ini memerlukan Metrics Server aktif. Di Minikube, aktifkan dengan: `minikube addons enable metrics-server`.

#### Mengelola Status Node (Maintenance)

Dalam kondisi tertentu, Anda mungkin perlu menghentikan sementara Node agar tidak menerima beban kerja baru:

* Cordon (Tandai Tidak Bisa Digunakan): Menghindari Pod baru masuk ke Node ini.

```bash
kubectl cordon <nama-node>
```

* Uncordon (Aktifkan Kembali): Mengizinkan kembali Pod baru masuk.

```bash
kubectl uncordon <nama-node>
```

* Drain (Kosongkan Node): Memindahkan semua Pod yang berjalan di Node tersebut ke Node lain (biasanya dilakukan sebelum mematikan server).

```bash
kubectl drain <nama-node> --ignore-daemonsets
```

#### 5. Memberi Label pada Node

Label berguna untuk mengelompokkan Node (misalnya: membedakan Node dengan SSD atau GPU).

```bash
kubectl label nodes <nama-node> disktype=ssd
```

## 5️⃣ Pod

### Apa itu Pod?

Pod adalah unit terkecil yang dapat dibuat dan dikelola di Kubernetes.

Jika di Docker Anda terbiasa mengelola Kontainer secara langsung, di Kubernetes Anda tidak menjalankan kontainer sendirian. Kontainer tersebut selalu dibungkus di dalam sebuah Pod.

Analogi: Jika kontainer adalah seekor ikan, maka Pod adalah akuariumnya. Satu akuarium (Pod) bisa berisi satu ikan atau beberapa ikan yang saling bekerja sama.

### Karakteristik Utama Pod

* **Satu IP per Pod**: Setiap Pod mendapatkan alamat IP unik. Semua kontainer di dalam satu Pod berbagi alamat IP yang sama.
* **Berbagi Penyimpanan (Volume)**: Kontainer dalam satu Pod dapat berbagi file melalui volume yang sama.
* ***Ephemeral (Sementara)***: Pod bersifat tidak kekal. Jika sebuah Pod mati, Kubernetes tidak akan "menghidupkannya lagi", melainkan membuat Pod baru sebagai penggantinya.
* **localhost**: Kontainer di dalam satu Pod yang sama bisa berkomunikasi satu sama lain menggunakan `localhost`.

### Kapan Menggunakan Multi-Kontainer dalam Satu Pod?

Sebagian besar Pod hanya berisi satu kontainer (pola paling umum). Namun, Anda bisa menggunakan lebih dari satu kontainer jika ada hubungan erat, misalnya:

* Main Container: Menjalankan aplikasi web.
* Sidecar Container: Menjalankan tugas pembantu seperti mengambil log atau memperbarui sertifikat SSL secara otomatis.

### Cara Membuat Pod

Ada dua cara untuk membuat Pod:

#### A. Cara Imperatif (Cepat untuk Tes)

Gunakan perintah `kubectl run` untuk membuat Pod secara instan:

```bash
kubectl run pod-nginx --image=nginx
```

#### B. Cara Deklaratif (Standar Industri)

Menggunakan file konfigurasi __YAML__. Buat file bernama `pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: web
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

Lalu jalankan perintah:

```bash
kubectl apply -f pod.yaml
```

atau

```bash
kubectl create -f pod.yaml
```

Perbedaan utamanya terletak pada metode pengelolaan status objek di Kubernetes. Secara singkat: `create` bersifat imperatif (sekali jalan / membuat objek baru dari nol), sedangkan `apply` bersifat deklaratif (berbasis perubahan status / membuat objek baru atau memperbarui yang sudah ada).

### Perintah Dasar Mengelola Pod

Berikut adalah perintah yang wajib Anda tau:

* Melihat daftar Pod:

`kubectl get pods`

* Melihat detail Pod (untuk debugging):

`kubectl describe pod <nama-pod>`

* Melihat log dari aplikasi di dalam Pod:

`kubectl logs <nama-pod>`

* Masuk ke terminal di dalam Pod:

`kubectl exec -it <nama-pod> -- bin/bash`

* Menghapus Pod:

`kubectl delete pod <nama-pod>`

#### Latihan membuat Pod Nginx

* Buat file `pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: web
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

* Deploy:

```bash
kubectl apply -f pod.yaml

# Cek pod
kubectl get pods
```

* Melihat Pod:

```bash
# Melihat daftar pod
kubectl get pods

# Melihat daftar pod lebih detail
kubectl get pods -o wide

# Melihat pod sangat detail
kubectl describe pod nginx-pod
```

* Akses Nginx dari Browser (Khusus Minikube):

Karena Pod berjalan di dalam klaster isolasi, Anda perlu melakukan _port-forwarding_ agar bisa membukanya di browser Windows dengan perintah `kubectl port-forward nginx-pod <portAkses>:<portPod>`:

```bash
kubectl port-forward nginx-pod 8080:80
```

Sekarang, buka browser di Windows dan akses `http://localhost:8080`. Anda akan melihat halaman selamat datang Nginx.

* Melihat Log Aplikasi:

Jika terjadi kendala, cek log dengan:

```bash
kubectl logs nginx-pod
```

## 6️⃣ Label

Label adalah pasangan kunci-nilai (_key-value pairs_) yang ditempelkan pada objek Kubernetes (seperti Pod, Service, atau Node) untuk mengidentifikasi dan mengelompokkan mereka secara logis. Label tidak memberikan fungsi langsung ke sistem, melainkan digunakan untuk organisasi.

### Mengapa Label Sangat Penting?
Tanpa label, Anda akan kesulitan mengelola ribuan Pod di dalam klaster. Label memungkinkan Anda untuk:

* Pengelompokan: Membedakan mana Pod untuk environment: production dan mana yang environment: staging.
* Versi: Menandai aplikasi dengan version: v1 atau version: v2.
* Selector (Penghubung): Digunakan oleh **Service** atau __Deployment__ untuk menemukan Pod mana yang harus mereka kelola.

### Contoh Penulisan Label (YAML)

Dalam file YAML, label diletakkan di bawah bagian metadata:

```yaml
metadata:
  name: nginx-pod
  labels:
    app: webserver
    tier: frontend
    env: production
```

* Deploy Pod:

```bash
kubectl create -f <nama-file-yaml>
```

### Cara Menggunakan Label dengan kubectl

* Melihat Label Pod:

```bash
kubectl get pods --show-labels
```
* Filter Berdasarkan Label (Selector):

Menampilkan hanya Pod yang memiliki label `env=production`:

```bash
kubectl get pods -l env=production
```

* Menambah Label secara Langsung:

```bash
kubectl label pods my-nginx-pod status=unstable
```

* Mengubah (Overwrite) Label yang Sudah Ada
Secara default, Kubernetes akan mencegah Anda mengubah label yang sudah ada untuk menghindari kesalahan. Anda harus menambahkan flag `--overwrite`:

```bash
kubectl label pods <nama-pod> <kunci>=<nilai> --overwrite
```

Contoh: Mengubah label "env" dari "dev" menjadi "prod":
`kubectl label pods my-nginx-pod env=prod --overwrite`

* Menghapus Label
Untuk menghapus label, gunakan kunci label diikuti dengan tanda minus (`-`):

```bash
kubectl label pods <nama-pod> <kunci>-
```

### Mencari Pod dengan Label

* Mencari Berdasarkan Satu Label

   Gunakan flag `-l` (singkatan dari `--selector`):

```bash
kubectl get pods -l app=webserver
```

* Mencari Berdasarkan Banyak Label (Logika AND)

  Jika Anda ingin mencari Pod yang memenuhi beberapa kriteria sekaligus, pisahkan dengan koma:

```bash
kubectl get pods -l app=webserver,env=prod
```

* Mencari Berdasarkan "Keberadaan" Kunci
  Menampilkan semua Pod yang memiliki label `env`, tidak peduli apa nilainya:

```bash
kubectl get pods -l env
```

* Menggunakan Operasi Set (In / NotIn)
  - `In`: Mencari Pod dengan nilai label yang ada dalam daftar.

```bash
kubectl get pods -l "env in (staging, prod)"
```
  - `Not In`: Mencari Pod yang nilainya bukan dari daftar tersebut.

```bash
kubectl get pods -l "env notin (dev)"
```

* Mencari yang "TIDAK" Memiliki Label Tertentu

  Gunakan tanda seru (`!`) untuk pengecualian:

```bash
kubectl get pods -l "!env"
```

* Tips: Melihat Semua Label Sambil Memfilter

Agar Anda bisa memverifikasi hasilnya dengan mudah, tambahkan flag `--show-labels`:

```bash
kubectl get pods -l app=webserver --show-labels
```

## 7️⃣ Annotation

Kubernetes Annotations sering kali membingungkan karena terlihat mirip dengan Labels. Namun, fungsinya sangat berbeda. Jika Labels digunakan untuk mengelompokkan objek, Annotations digunakan untuk memberikan instruksi atau metadata tambahan kepada sistem eksternal atau kontroler.

Annotations adalah metadata non-identitas yang ditempelkan pada objek Kubernetes (seperti Pod, Deployment, atau Service). Metadata ini tidak digunakan oleh Kubernetes untuk memilih atau mengelompokkan objek, tetapi digunakan oleh tool pihak ketiga, perpustakaan, atau kontroler untuk mengubah perilaku aplikasi.

> Gunakan Labels jika Anda ingin "memanggil" atau "mengelompokkan" objek. Gunakan Annotations jika Anda ingin "memberi instruksi" atau "catatan" tambahan bagi sistem lain yang mengelola objek tersebut.

### Kapan Harus Menggunakan Annotations?

Anda sebaiknya menggunakan Annotations dalam skenario berikut:

* Konfigurasi Ingress: Memberi tahu Nginx Ingress Controller untuk mengaktifkan SSL atau membatasi ukuran upload.
* Informasi Build/Release: Mencatat nomor build CI/CD, ID commit Git, atau tanggal rilis.
* Informasi Kontak: Mencatat siapa penanggung jawab (email/nomor telepon) jika terjadi incident.
*  Integrasi Tool Eksternal: Misalnya, memberi tahu Prometheus untuk melakukan scraping metrik pada port tertentu.
* Sidecar Injection: Memberi tahu Service Mesh (seperti Istio) untuk memasukkan kontainer tambahan secara otomatis.

### Struktur Penulisan (Syntax)

Annotations ditulis dalam format key-value. Key biasanya menggunakan format DNS untuk menghindari konflik antar tool.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-with-annotation
  labels:
    team: product
    version: "1.0"
    environment: development
  annotations:
    description: "Ini adalah aplikasi yang dibuat oleh Tim Product"
    other: "Some text..."
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

* Deploy Pod:

```bash
kubectl create -f pod-with-annotation.yaml
```

* Melakukan pengecekan:

```bash
kubectl describe pod nginx-with-annotation
```