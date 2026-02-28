# Belajar Kubernetes

**Kubernetes** (sering disingkat **K8s**) adalah platform open-source yang digunakan untuk orkestrasi kontainer. 

Bayangkan aplikasi Anda adalah kumpulan kotak-kotak (kontainer) yang berisi kode. Jika Anda hanya punya satu kotak, mengaturnya mudah. Namun, jika Anda punya ribuan kotak di berbagai server, Anda butuh "kapten kapal" untuk mengaturnya—itulah Kubernetes (kata Yunani untuk "nakhoda" atau "kapten"). 

## Apa Fungsinya?
Kubernetes bekerja seperti konduktor orkestra yang memastikan setiap instrumen (kontainer) bermain pada waktu yang tepat dan dengan volume yang pas. Fungsi utamanya meliputi: 

* **Otomatisasi Deployment**: Memasang dan memperbarui aplikasi secara otomatis tanpa menghentikan layanan (zero downtime).
* **Self-Healing**: Jika ada kontainer yang rusak atau mati, Kubernetes akan mendeteksi dan menggantinya secara otomatis agar aplikasi tetap berjalan.
* **Auto-Scaling**: Menambah jumlah kontainer saat pengunjung aplikasi melonjak dan menguranginya saat sepi agar hemat biaya.
* **Load Balancing**: Membagi trafik masuk secara merata ke berbagai kontainer supaya tidak ada server yang keberatan beban. 

## Mengapa Kubernetes Populer?

Platform ini awalnya dikembangkan oleh Google dan kini dikelola oleh Cloud Native Computing Foundation (CNCF). Kubernetes menjadi standar industri karena kemampuannya menjaga aplikasi tetap stabil meski dijalankan di infrastruktur yang sangat besar, baik di cloud (seperti AWS, Google Cloud, Azure) maupun di server lokal. 

Untuk mulai mencoba, Anda bisa mengikuti Panduan Dasar di Situs Resmi Kubernetes yang tersedia dalam Bahasa Indonesia. 
