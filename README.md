# Three Ways of Storing and Accessing Lots of Images in Python
# 1. Storing to Disk (as individual image files)
# 2. Storing to LMDB
# 3. Storing to HDF5
📁 1. Menyimpan ke Disk (File .png dan .csv)

📌 Cara kerja:
Gambar disimpan sebagai file .png di folder menggunakan Pillow (Image.save).

Label (metadata) disimpan terpisah dalam file .csv.

✅ Kelebihan:
- Mudah diakses manusia: File dapat dibuka langsung di file explorer atau editor gambar.

- Portabilitas tinggi: Bisa dipindah dan dibaca di banyak sistem operasi dan bahasa pemrograman.

- Cocok untuk dataset kecil hingga sedang.

❌ Kekurangan:
- Tidak efisien secara performa saat jumlah gambar banyak.

- Metadata terpisah → sulit sinkronisasi kalau tidak hati-hati.

- Lambat untuk pemrosesan batch → harus load file satu per satu dari disk.

📦 2. Menyimpan ke LMDB (Lightning Memory-Mapped Database)

📌 Cara kerja:
- Gambar dikonversi menjadi byte (.tobytes()), lalu dibungkus ke dalam class CIFAR_Image bersama label.

- Diserialisasi dengan pickle dan disimpan dalam database key-value (lmdb).

- Kunci (key) berupa ID unik, nilai (value) berupa objek gambar+label yang telah diserialisasi.

✅ Kelebihan:
- Kecepatan tinggi dalam membaca data karena menggunakan memory-mapping.

- Mendukung akses paralel untuk membaca (multi-thread read).

- Cocok untuk deep learning pipeline besar (seperti Caffe dan PyTorch).

❌ Kekurangan:
- Harus menentukan map_size (jumlah memori awal) saat pembuatan — bisa rumit untuk dataset besar atau dinamis.

- Tidak bisa overwrite data lama → hanya bisa append, kecuali database dibuat ulang.

- Lebih kompleks dibanding menyimpan ke disk.

🧬 3. Menyimpan ke HDF5 (Hierarchical Data Format 5)

📌 Cara kerja:
- Gambar dan label disimpan dalam satu file .h5 dengan dua dataset (image, label).

- Gunakan library h5py.

✅ Kelebihan:
- Struktur fleksibel dan hierarkis (grup dan dataset bisa bersarang).

- Sangat efisien untuk batch processing dan subset indexing (misalnya ambil hanya sebagian data: dataset[0:100]).

- Banyak digunakan di scientific computing dan deep learning (misalnya Keras).

❌ Kekurangan:
- File .h5 tidak bisa dibuka langsung seperti gambar biasa.

- Tidak cocok untuk dataset sangat dinamis atau yang sering berubah-ubah.

- Potensi masalah jika struktur datanya tidak konsisten.


