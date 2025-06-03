# Laporan Proyek Machine Learning - Melina Kharisa

## Project Overview
Peningkatan jumlah penonton bioskop sejalan dengan semakin banyaknya film yang diproduksi, baik dari industri perfilman luar negeri maupun dalam negeri. Keberagaman alur cerita, genre, dan tema menjadikan pasar film semakin kompetitif, namun di sisi lain dapat menyulitkan calon penonton dalam menentukan film yang ingin ditonton. Hal ini menyebabkan pengguna menghabiskan lebih banyak waktu untuk mencari film yang sesuai dengan preferensi mereka. Beberapa orang memanfaatkan fitur pencarian di berbagai situs untuk membantu pengambilan keputusan. Mengingat selera setiap individu berbeda dan cenderung menyukai film yang serupa dengan yang pernah dinikmati sebelumnya, sistem rekomendasi menjadi solusi yang tepat untuk menyajikan informasi film yang relevan secara personal (Fajriansyah et al., [2021](https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163)).

Mengembangkan sistem rekomendasi film menjadi penting karena dapat meningkatkan kepuasan pengguna platform streaming film, mendorong penemuan konten baru, dan berpotensi meningkatkan retensi pengguna. Pendekatan berbasis genre dipilih sebagai langkah awal yang intuitif dan relatif sederhana untuk mengimplementasikan sistem rekomendasi. Saat ini, berbagai platform film menggunakan sistem rekomendasi yang lebih kompleks, menggabungkan berbagai faktor seperti riwayat tontonan pengguna, rating, aktor, sutradara, dan deskripsi film. Namun, memahami dasar rekomendasi berbasis konten seperti genre adalah langkah penting dalam membangun sistem yang lebih canggih.

## Business Understanding

### Problem Statements

- Pengguna platform film sering mengalami kesulitan dalam menemukan film baru yang sesuai dengan genre favorit mereka karena banyaknya pilihan yang tersedia.
- Sistem rekomendasi yang hanya berdasarkan tingkat kepopuleran tidak selalu mampu menyajikan film yang sesuai dengan selera spesifik pengguna.

### Goals

- Mengembangkan sistem yang dapat merekomendasikan film kepada pengguna berdasarkan kemiripan genre dengan film yang mereka sukai atau pertimbangkan.
- Mengevaluasi seberapa efektif pendekatan berbasis genre dalam memberikan rekomendasi yang sesuai dan relevan bagi pengguna.

## Data Understanding    
Dataset ini terdiri dari beberapa file yang menyimpan data film dan interaksi pengguna, antara lain:

1. Movies: Data mengenai film yang meliputi informasi identifikasi dan deskripsi film. Terdiri dari 9742 baris dan 3 kolom yaitu:        
 - **movieId**: Identifier unik untuk masing-masing film.
 - **title**: Judul film, yang sering kali mencakup tahun rilis.
 - **genres**: Kategori atau genre film. Informasi genre ini akan menjadi dasar bagi sistem rekomendasi berbasis kesamaan genre.
2. Ratings: Data mengenai interaksi pengguna dengan film, berupa rating atau penilaian. Terdiri dari 100836 dan 4 kolom, yaitu:        
 - **userId**: Identifier unik dari pengguna yang memberikan rating.
 - **movieId**: Identifier film, yang menghubungkan rating dengan data film.
 - **rating**: Nilai penilaian yang diberikan pengguna.
 - **timestamp**: Waktu terjadinya penilaian, yang dapat digunakan untuk analisis tren dan pola perilaku pengguna.    

Karakteristik Data:
- **Variasi Genre**: Setiap film dapat memiliki satu atau lebih genre, memungkinkan analisis kesamaan antar film berdasarkan genre.
- **Distribusi Rating**: Rating yang diberikan oleh pengguna memungkinkan identifikasi film yang populer atau memiliki penerimaan beragam.
- **Analisis Temporal**: Data timestamp pada rating memungkinkan analisis tren rating film dalam jangka waktu tertentu.

*Exploratory Data Analysis*     

Analisis eksploratif dilakukan untuk memahami karakteristik data sebelum proses pemodelan sistem rekomendasi. Langkah awal mencakup pemeriksaan nilai hilang (missing value) dan data duplikat. Berdasarkan hasil pengecekan, tidak ditemukan nilai hilang maupun data duplikat pada kedua file dataset, yaitu movies.csv dan ratings.csv. Hal ini menunjukkan bahwa data telah tersedia dalam kondisi yang bersih dan siap untuk dianalisis lebih lanjut. Link dataset: https://www.kaggle.com/datasets/gargmanas/movierecommenderdataset/data     

Analisis distribusi variabel dilakukan secara univariat dan multivariat. Secara univariat, visualisasi jumlah film per tahun menunjukkan tren peningkatan produksi film dari waktu ke waktu, sedangkan distribusi jumlah rating per tahun memperlihatkan fluktuasi aktivitas pengguna dalam memberikan penilaian. Rata-rata rating per genre mengindikasikan bahwa genre tertentu seperti dokumenter dan drama cenderung mendapatkan rating lebih tinggi dibanding genre lain, dan distribusi rating menunjukkan mayoritas film menerima nilai di kisaran menengah (3.0–4.5). Sementara itu, analisis multivariat menunjukkan bahwa jumlah rating tidak selalu berbanding lurus dengan rata-rata rating suatu film, artinya popularitas tidak menjamin kualitas yang dirasakan pengguna. Beberapa genre juga menunjukkan frekuensi rating yang lebih tinggi, mencerminkan tingkat popularitasnya, dan analisis hubungan antara tahun rilis dengan rata-rata rating menunjukkan bahwa kualitas film tidak terlalu dipengaruhi oleh tahun produksinya. Selain itu, identifikasi film dengan jumlah rating terbanyak dan tersedikit membantu mengenali film-film yang sangat populer maupun kurang dikenal dalam dataset.       

## Data Preparation
Tahapan data preparation yang dilakukan adalah sebagai berikut:        

1. **Ekstraksi Informasi Tahun**            
Langkah pertama dalam proses persiapan data adalah mengekstraksi informasi tahun dari atribut judul film. Informasi ini kemudian disimpan dalam kolom terpisah agar dapat dianalisis secara terpisah dari atribut judul. Tujuannya yaitu memisahkan informasi temporal dari teks judul memungkinkan analisis berdasarkan waktu, seperti tren distribusi atau evolusi karakteristik film dalam rentang waktu tertentu.              
2. **Menghapus Tahun dari Judul Film**             
Setelah informasi tahun berhasil dipisahkan, bagian tahun yang masih terdapat di dalam kolom judul dihapus. Hal ini dilakukan agar judul film hanya berisi nama film tanpa tambahan informasi lainnya. Tujuannya karena judul yang bersih lebih mudah digunakan dalam proses pencocokan string, sistem rekomendasi, atau analisis berbasis teks lainnya. Selain itu, penghapusan ini juga membantu menghindari duplikasi atau ketidaksesuaian saat membandingkan judul film dari sumber data lain.
3. **Menghilangkan Spasi di Awal dan Akhir Judul**            
Selanjutnya, dilakukan pembersihan pada setiap judul film dengan cara menghilangkan spasi kosong yang mungkin muncul di awal atau akhir string judul. Alasannya karena spasi yang tidak terlihat ini dapat menyebabkan kesalahan dalam pencocokan string atau analisis berbasis teks. Oleh karena itu, langkah ini penting untuk memastikan konsistensi dalam data.
4. **Konversi Format Waktu**             
Atribut waktu yang semula disimpan dalam format numerik dikonversi menjadi format tanggal dan waktu yang lebih mudah dipahami. Tujuannya adalah untuk mempermudah proses analisis data berbasis waktu, seperti mengidentifikasi pola distribusi data dalam kurun waktu tertentu atau melakukan segmentasi berdasarkan periode.
5. **Menggabungkan Dataset Film dan Rating**                  
Setelah data film dan data rating dibersihkan, kedua dataset tersebut digabungkan berdasarkan kolom movieId sebagai kunci penghubungnya. Tujuan penggabungan ini diperlukan agar setiap rating dapat dikaitkan langsung dengan informasi lengkap dari film tersebut, termasuk nama film, genre, dan tahun rilis. Data yang telah tergabung menjadi dasar utama untuk analisis selanjutnya.
6. **Mengurutkan dan Menghapus Duplikasi Film**              
Data film kemudian diurutkan berdasarkan movieId, dan dilakukan penghapusan duplikasi agar hanya terdapat satu data unik untuk setiap film. Tujuan penghapusan duplikasi ini penting untuk memastikan bahwa setiap film hanya direpresentasikan satu kali dalam daftar film, terutama ketika akan digunakan untuk membuat sistem rekomendasi atau pemetaan film.
7. **Mengubah Data Series menjadi List**                   
Setelah data difilter, kolom-kolom penting seperti ID film, nama film, dan genre diubah menjadi bentuk list. Tujuannya karena konversi ke dalam bentuk list memudahkan proses manipulasi data di tahap selanjutnya, seperti saat membuat dictionary film atau ketika digunakan dalam algoritma sistem rekomendasi.
8. **Pembentukan DataFrame (movie_dict)**         
Mengorganisir data ID film, nama film, dan genre ke dalam sebuah DataFrame Pandas untuk memudahkan analisis dan pemodelan. Tahap ini penting untuk memiliki struktur data yang terorganisir.               
9. **Transformasi Genre dengan TF-IDF**              
Atribut genre yang berupa teks diproses menggunakan teknik TF-IDF (Term Frequency-Inverse Document Frequency). TF-IDF digunakan untuk mengukur seberapa penting sebuah kata (genre) dalam dokumen tertentu (film) relatif terhadap seluruh kumpulan dokumen (semua film). Dengan menggunakan TfidfVectorizer, genre dikonversi ke dalam bentuk vektor numerik, menghasilkan matriks berdimensi n × m, di mana n adalah jumlah film dan m adalah jumlah genre unik. Genre yang lebih unik akan memiliki bobot yang lebih tinggi. Transformasi ini diperlukan agar data berbasis teks dapat direpresentasikan dalam bentuk numerik dan digunakan dalam perhitungan matematis, khususnya untuk pengukuran kemiripan.              

## Modeling                  
Sistem rekomendasi yang dibangun menggunakan pendekatan Content-Based Filtering, yaitu metode yang merekomendasikan film berdasarkan kemiripan fitur antara film satu dengan lainnya. Dalam kasus ini, fitur yang digunakan adalah genre, yang sebelumnya telah ditransformasi menjadi vektor numerik menggunakan teknik TF-IDF pada bagian data preparation.                    

Setelah setiap film direpresentasikan sebagai vektor TF-IDF berdasarkan genre-nya, langkah selanjutnya adalah menghitung kemiripan antar film menggunakan cosine similarity.                      

**Cosine similarity** adalah metode pengukuran kemiripan antara dua vektor dalam ruang berdimensi tinggi. Nilai cosine similarity berkisar antara 0 hingga 1, di mana:
- Nilai 1 menunjukkan bahwa dua vektor memiliki arah yang sama atau sangat mirip.
- Nilai 0 menunjukkan bahwa dua vektor saling tegak lurus atau tidak memiliki kemiripan sama sekali.
  
Menghitung cosine similarity antar representasi TF-IDF dari genre setiap film. Cosine similarity mengukur sudut antara dua vektor, sehingga memberikan ukuran kemiripan antar film berdasarkan representasi genrenya. Hasilnya adalah matriks kemiripan (mov_sim). Selanjutnya, mengubah matriks cosine similarity menjadi DataFrame dengan nama film sebagai indeks dan kolom. Ini memudahkan untuk mencari dan melihat skor kemiripan antar film.                    
Rumus cosine similarity adalah sebagai berikut:                    
![Screenshot-2021-11-27-at-15 57 39-1024x211](https://github.com/user-attachments/assets/e9525f6a-3f19-4c58-b775-bdcf394f1f0f)                      

Dengan menghitung nilai cosine similarity antar seluruh pasangan film, sistem dapat mengidentifikasi film-film yang memiliki genre paling mirip dengan film yang sedang dicari.                   

**Implementasi Fungsi Rekomendasi**            
Fungsi rekomendasi dirancang untuk memberikan daftar film yang paling mirip dengan film tertentu berdasarkan kemiripan genre. Proses kerja fungsi ini dimulai dengan mengidentifikasi film yang dipilih sebagai referensi, lalu mencari nilai kemiripan antara film tersebut dengan seluruh film lainnya menggunakan matriks kemiripan yang telah dihitung sebelumnya melalui metode cosine similarity. Dari hasil tersebut, sistem memilih sejumlah film dengan skor kemiripan tertinggi. Namun, agar hasil rekomendasi tetap relevan dan tidak redundan, film yang digunakan sebagai referensi secara eksplisit dikeluarkan dari daftar hasil. Hal ini dilakukan untuk memastikan bahwa sistem tidak merekomendasikan film yang sama dengan yang diminta oleh pengguna. Selanjutnya, daftar film yang paling mirip digabungkan dengan data metadata seperti nama dan genre film, sehingga hasil akhir berupa daftar film yang direkomendasikan dapat ditampilkan secara lengkap dan informatif kepada pengguna. Jumlah rekomendasi yang ditampilkan dapat diatur sesuai kebutuhan, umumnya sebanyak lima film teratas dengan tingkat kemiripan tertinggi. Langkah ini menjadi inti dari sistem rekomendasi berbasis konten karena seluruh proses dilakukan dengan membandingkan atribut film itu sendiri (dalam hal ini genre), tanpa melibatkan preferensi pengguna atau data perilaku sebelumnya.                  

**Top-N Recommendation**                            
Setelah kemiripan antar film dihitung, sistem akan menampilkan Top-N Recommendation, yaitu sejumlah rekomendasi film yang paling mirip dengan film input. Dalam implementasi ini, nilai N (jumlah rekomendasi) dapat disesuaikan, misalnya 5 rekomendasi teratas. Film yang sama dengan input tidak akan dimunculkan kembali dalam hasil rekomendasi.                        

Contoh output rekomendasi (berdasarkan implementasi fungsi):             

| movie_name                                  | genre                          |       
|---------------------------------------------|--------------------------------|        
| Darby O'Gill and the Little People          | Adventure\|Children\|Fantasy   |         
| NeverEnding Story II: The Next Chapter, The | Adventure\|Children\|Fantasy   |            
| Bridge to Terabithia                        | Adventure\|Children\|Fantasy   |           
| Return to Oz                                | Adventure\|Children\|Fantasy   |         
| NeverEnding Story, The                      | Adventure\|Children\|Fantasy   |        


**Kelebihan Pendekatan Content-Based Filtering**
1. **Personalized**: Rekomendasi bersifat spesifik terhadap item yang sedang ditinjau, bukan berdasarkan preferensi pengguna lain.
2. **Tidak membutuhkan interaksi pengguna lain**: Sistem tetap dapat bekerja meskipun hanya memiliki informasi deskriptif dari item, tanpa perlu data rating dari banyak pengguna.
3. **Dapat menjelaskan rekomendasi**: Karena sistem berdasarkan fitur item (dalam hal ini genre), alasan di balik rekomendasi menjadi lebih jelas dan mudah dipahami.                    

**Kekurangan**: Terbatas pada genre yang ada; tidak dapat merekomendasikan film di luar preferensi genre pengguna saat ini. Kemiripan genre yang sederhana mungkin tidak menangkap nuansa preferensi pengguna yang lebih kompleks.

## Evaluation
**Metrik Evaluasi yang Digunakan**                
Metrik yang digunakan untuk mengevaluasi performa sistem adalah Precision. Precision mengukur sejauh mana rekomendasi yang dihasilkan sesuai atau relevan dengan item acuan. Dalam konteks ini, genre dijadikan dasar penilaian relevansi. Jika genre film rekomendasi sama persis dengan genre film yang dicari pengguna, maka dianggap relevan. Precision dihitung dengan rumus:                    
![precision](https://github.com/user-attachments/assets/831d5f64-60dd-43cc-96d3-9a12218427bb)                                    

**Hasil Evaluasi**
Evaluasi dilakukan dengan mengambil sampel judul film dan menghitung precision untuk top-5 recommendation. Sebagai contoh, untuk film "Jumanji", precision yang diperoleh adalah:
**Precision**: `1.00`

| No | movie_name                                    | genre                         | similarity_score | relevant |
|----|-----------------------------------------------|-------------------------------|------------------|----------|
| 1  | Darby O'Gill and the Little People            | Adventure\|Children\|Fantasy | 1.0              | True     |
| 2  | NeverEnding Story II: The Next Chapter, The   | Adventure\|Children\|Fantasy | 1.0              | True     |
| 3  | Bridge to Terabithia                          | Adventure\|Children\|Fantasy | 1.0              | True     |
| 4  | Return to Oz                                  | Adventure\|Children\|Fantasy | 1.0              | True     |
| 5  | NeverEnding Story, The                        | Adventure\|Children\|Fantasy | 1.0              | True     |

Seluruh film yang direkomendasikan memiliki genre yang identik dengan film acuan, sehingga sistem berhasil mencapai nilai *precision* sempurna (1.00). Hal ini menunjukkan bahwa pendekatan yang digunakan sangat efektif dalam memberikan hasil rekomendasi yang relevan dengan minat pengguna.             

**Hubungan Evaluasi dengan Business Understanding**           
Hasil evaluasi sistem rekomendasi menunjukkan bahwa pendekatan content-based filtering berbasis genre memiliki relevansi yang cukup baik dalam memberikan rekomendasi film. Nilai precision yang diperoleh mencerminkan bahwa mayoritas film yang direkomendasikan memiliki genre yang identik dengan film acuan, sehingga sesuai dengan preferensi pengguna. Evaluasi ini sekaligus menunjukkan bahwa sistem mampu memberikan rekomendasi yang relevan dan terarah berdasarkan kemiripan konten, bukan sekadar popularitas film. Dengan demikian, Goal 1, yaitu mengembangkan sistem rekomendasi berbasis kemiripan genre, telah dicapai melalui penerapan metode TF-IDF untuk representasi teks genre dan penghitungan cosine similarity sebagai dasar penilaian kemiripan antar film. Selain itu, Goal 2, yaitu mengevaluasi efektivitas pendekatan berbasis genre, juga telah tercapai dengan mengukur kinerja sistem menggunakan metrik precision yang menggambarkan tingkat kesesuaian hasil rekomendasi terhadap genre film input. Secara keseluruhan, evaluasi ini mendukung pemahaman bisnis bahwa sistem yang dibangun mampu menjawab permasalahan pengguna dalam menemukan film yang sesuai dengan preferensi mereka, dan sekaligus mendorong kepuasan serta retensi pengguna melalui hasil rekomendasi yang lebih personal dan relevan.             

**Dampak terhadap Solusi dan Keputusan Bisnis**
Sistem ini berdampak positif terhadap pengalaman pengguna, khususnya dalam hal:
1. Personalisasi: Rekomendasi menjadi lebih sesuai dengan selera pengguna berdasarkan genre.
2. Retensi Pengguna: Kemungkinan pengguna bertahan lebih tinggi karena rekomendasi terasa lebih akurat.
3. Nilai Tambah Bisnis: Sistem ini dapat diterapkan pada platform streaming atau katalog film untuk meningkatkan kepuasan dan keterlibatan pengguna.                              

Evaluasi juga memberikan gambaran tentang area yang masih bisa dikembangkan, seperti menambahkan data deskriptif lain (sinopsis, aktor, sutradara) atau menggabungkan dengan pendekatan collaborative filtering di masa mendatang untuk meningkatkan relevansi rekomendasi.

## References
<a id="1">[1]</a> 
Fajriansyah, M., Adikara, P. P., & Widodo, A. W. (2021). Sistem Rekomendasi Film Menggunakan Content Based Filtering. Jurnal Pengembangan Teknologi Informasi Dan Ilmu Komputer, 5(6), 2188–2199. Diambil dari https://j-ptiik.ub.ac.id/index.php/j-ptiik/article/view/9163
**---Ini adalah bagian akhir laporan---**
