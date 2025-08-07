# LLM-Fine-Tuning-for-Task-Oriented-Instruction-Generation

## 1. Open-Source LLM 
menggunakan Mistral-7B-Instruct v0.3 (huggingface.co/mistralai/Mistral-7B-Instruct-v0.3)
Keunggulan : Lebih ringan jika dibandingkan dengan model lain (hanya 7B parameter) sehingga latensinya juga lebih rendah.

## 2. Dataset
Tipe data menggunakan format JSON
Contoh format :
{
  "prompt": "How do I reset my password in the e-commerce XX mobile app?",
  "response": [
    "Open the app",
    "Go to the login page",
    "Tap on the 'Forgot Password' link",
    "Enter your registered email address",
    "Tap 'Submit' to request a reset link",
    "Check your email and verify the reset request"
    "Follow the link to set a new password"
  ]
}

Pengumpulan data mungkin bisa menggunakan data dari forum FAQ, dokumentasi produk, obrolan customer service dan augmentasi data menggunakan LLM lain. Kemudian dianotasi ke dalam struktur dan format sesuai contoh. 

Di tahap preproses, data prompt dan response disamakan semua menjadi lowercase dan tanpa tanda baca, kemudian dilakukan tokenisasi yang sesuai dengan format Mistral 7B dan split data untuk membagi data menjadi data training, validation dan test.

Untuk mencegah Data Imbalance, Sensitive‬ ‭Information,‬ atau‬ ‭memastikan ‭diversity‬ dan‬ ‭generalization‬, data yang dipakai perlu diperhatikan lebih lagi. Jika tidak seimbang dan kurang menjangkau keberagaman dan generalisasi maka perlu menambahkan data dengan cara augmentasi menggunakan LLM lain. Jika terdapat informasi sensitif berarti data yang pakai saat training mengandung informasi sensitif dan perlu diubah menjadi placeholder. 

## 3. Fine-Tuning

Fine-tuning menggunakan QLoRa, agar kebutuhan hardware untuk melakukan fine-tuning tidak terlalu besar dan mencegah catastrophic forgetting.

Hyperparameter yang perlu dilakukan tuning : 
- learning rate, untuk mengatur kecepatan training dan agar tidak terjebak di local optimum 
- epochs, untuk memastikan apakah model sudah cukup belajar pola dari data training, 
- batch size, untuk menyeimbangkan antara kecepatan training dan penggunaan GPU

Potensi tantangan yang dihadapi :
- Sumber daya komputasi yang dibutuhkan cukup besar dan potensi catastrophic forgetting, solusinya adalah menggunakan QLoRa.
- Overfitting, solusinya adalah melakukan tuning pada epochs.

## 4. Evaluasi dan Benchmarking
Metriks evaluasi menggunakan BERTScore untuk mengukur kesamaan intruksi output dengan ground-truth-nya dan order accuracy untuk mengukur seberapa benar urutan langkah-langkah instruksi.

Model yang di fine tune dibandingkan dengan model dasarnya (Mistral 7B Instruct) dan data test untuk benchmarking.

Untuk penilaian kuantitatif, semua prompt dari data test dijalankan pada model yang sudah difine-tune dan model dasarnya, kemudian dihitung menggunakan BERTScore dan order accuracy, lalu dibandingkan untuk melihat peningkatan dari model yang sudah difine-tune.

Sedangkan untuk penilaian kualitatif bisa menggunakan rating dari skala 1-5 untuk beberapa kriteria seperti kebenaran, kelengkapan dan kejelasan.

