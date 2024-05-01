### Nama : Shaquille Athar Adista
### NPM : 2206081875

1. - Unary RPC:
     - Unary RPC melibatkan simple request-response komunikasi pattern, dimana client mengirim single request ke server dan menunggu single respons.
     - Unary RPC cocok untuk skenario dimana clien ingin melakukan operasi sederhana atau hanya ingin menerima jumlah data yang kecil dari server.
     - Contoh penggunaan unary RPC adalah ntuk menerima informasi profile user, fetching spesific record dari database.
   
   - Server Streaming RPC:
     - Streaming RPC, client akan mengirimkan single request ke server dan server akan merespons dengan stream of data, server akan merespons dengan serangkain pesan, bukan satu pesan tunggal seperti Unary RPC.
     - Server streaming RPC cocok untuk skenario dimana server butuh untuk mengirimkan data yang besar ke client sebagai respons terhadap satu permintaan.
     - Server streaming RPC cocok untuk tugas-tugas ketika ingin mengambil data berukuran besar atau menerima real-time data dari database. Sepertinya misalnya ketika melakukan stream video.
  
    - Bi-direction streaming RPC:
      - Bi-directional streaming RPC melibatkan klien dan server yang akan membuat koneksi, sehingga memungkinkan mereka untuk mengirim dan menerima pesan secara asinkron (server dan client sama-sama bisa mengirim dan menerima pesan).
      - Bi-directional streaming RPC cocok ketika klien dan server perlu mengirim pesan satu sama lain secara real-time, berguna ketika ingin membuat aplikasi obrolan real-time, multiplayer gaming, atau collaborative document editing.

2. - Otentikasi
      - Penerapan otentikasi yang benar, hal ini dapat dicapai dengan menerapkan mekanisme otentikasi yang benar, seperti OAuth, JWT, dan mTLS.
      - Otentikasi server gRPC untuk memastikan bahwa client berkomunikasi dengan server yang benar, hal ini dapat dicapai dengan menggunakan sertifikat TSL/SSL.
    - Otoriasi
      - Terapkan otorisasi yang benar untuk mengkontrol akses client berdasarkan identitas dan izin mereka
      - gRPC mungkin memiliki tantangan dalam menerapkan kontrol akses yang sesuai dengan kebutuhan aplikasi, terutama ketika memperhatikan permintaan yang berulang-ulang dalam koneksi yang tetap terbuka, maka developer harus menerapkan sistem otoriasi yang fleksibel, seperti penerapan RBAC atau ABAC
    - Enkripsi data
      - Enkripsi data yang dikirim antara client gRPC dan server, dapat menggunakan enkripsi TLS/SSL dan pastikan konfigurasi parameter TLS/SSL yang tepat.
    - Secure communication
      - Amankan saluran komunikasi antara klien gRPC dan server dengan menggunakan Transport Layer Security (TLS) untuk mengenkripsi data saat transit.
      -  Penggunaan Protokol Buffers untuk serialisasi data di gRPC dapat menyebabkan potensi kerentanan serialization yang dapat dimanfaatkan untuk melakukan serangan seperti buffer overflow atau injection attacks, untuk mengatasi ini, developer harus menerapkan validasi data yang tepat.
      - Implementasikan saluran aman dengan otentikasi bersama (mTLS) untuk memverifikasi identitas klien dan server, mencegah serangan man-in-the-middle.
    - Secure konfigurasi
      - Pastikan layanan gRPC dan dependensinya dikonfigurasi dengan baik dan tahan terhadap kerentanan keamanan umum. 

3. Dalam aplikasi real-time chat menggunakan bi-directional streaming memiliki beberapa tantangan, yaitu cara mengelola konkurensi, mengimplementasikan backpressure dengan baik, menjaga urutan pesan, sinkronisasi data (ketika pesan dikirim dan diterima secara bersamaan diperlukan sinkronisasi yang cermat agar pesan dapat sampai dengan benar), manajemen koneksi, error handling, kepadatan trafik (menggunakan streaming dua arah dapat membuat banyaknya pesan yang dikirim secara bersamaan antara klien dan server sehingga akan menghasilkan kepadatan trafik yang tinggi).

4. Keuntungan :
   - Terintegrasi dengan Tokio, sehingga memudahkan untuk menggabungkan fungsionalitas streaming ke dalam aplikasi rust yang asinkron.
   - Fleksibel, `ReceiverStream` menyediakan fleksibelitas dalam handling asinkron stream of data.
   - Abstraction, `ReceiverStream` membuat developer tidak perlu memusingkan compleksitas dari managin stream sehingga bisa fokus ke implementasi aplikasinya.
   
   Kerugian :
   - Terbatas pada ekosistem Tokio, `ReceiverStream` terkait erat dengan Tokio, sehingga tidak cocok untuk framework asinkron lain.
   - Learning curve, karena asinkron stream works di rust cukup sulit dipelajari bagi developer yang baru memakainya.
   - Complex, asinkron prorgam dapat terbilang jauh lebih kompleks dari sinkronus program.

5. Base kode gRPC dapat disusun dengan membuat suatu gRPC service interface menggunakan protocol buffer, kemudian pisahkan interface dengan detail implementasinya. Menerapkan layanan gRPC sebagai modul atau struct yang terpisah. Gunakan dependency injection untuk menginject dependency ke gRPC service (dapat mengurangi duplikasi kode). Pisahkan gRPC service ke beberapa modul atau file berdasarkan fungsionalitasnya. Extract component atau common functionality ke Rust moduls atau crates sehingga bisa di reuse nantinya.

6. Improve : 
   - Menambahkan berbagai skenario error handling yang mungkin terjadi.
   - Melakukan integrasi dengan payment gateway.
   - Membuat loggin untuk mencatat semua kejadian penting, seperti permintaan pembayaran, hasil pembayaran, dan data-data penting lainnya.
   - Merekam riwayat transaksi untuk pelacakan dan arsip.
   - Memastikan keamanan transaksi dan data pembayaran.
   - Mengirim notifikasi mengenai hasil pembayaran (gagal atau berhasil).
   - Melakukan validasi data pembayaran. 


7.  Impact :
    - gRPC menggunakan HTTP/2 sebagai protokol transpotnya sehingga menawarkan banyak fitur baru yang dapat bermanfaat seperti multiplexing dan kompresi header, sehingga akan menghasilkan komunikasi antar layanan yang efisien karena memiliki latensi dan overhead yang lebih rendah daripada penggunaan API dengan HTTP/1
       
    - Platform agnostik, gRPC dapat berjalan di platform apa pun, termasuk lingkungan cloud-native, container, dan perangkat seluler, sehingga memungkinkan untuk penyebaran layanan ke berbagai lingkungan infrastruktur tanpa modifikasi.

    - gRPC mendukung berbagai bahasa pemrograman, sehingga memungkinkan layanan untuk mengimplementasi berbagai bahasa pemrograman. Hal ini memungkinkan developer untuk menggunakan bahasa pemrograman sesuai dengan kebutuhan tanpa mengorbankan interoperabilitas.

    - gRPC memberikan dukungan untuk integrasi dengan sistem dan protokol yang ada melalui fitur seperti proxt gateway dan middleware.

    - gRPC mengandalkan protocol buffer untuk menentukan data serialization, sehingga mendorong pemisahan yang jelas antar service interface dan implementasinya, sehingga aplikasi akan lebih mudah di maintain.
      
    - Strongly Typed API, gRPC akan menghasilkan strongly typed API sehingga memungkinkan client untuk generate native stubs atau client library dalam berbagai bahasa pemrograman langsung dari protobuf definitions, sehingga akan memastikan type safety dan akan menghasilkan komunimasi antar layanan yang lebih robust.

8.  Keuntungan :
    - HTTP/2 mendukung multiplexing sehingga memungkinkan beberapa request dan response dikirim dan diterima melalui satu koneksi TCP secara bersamaan, hal ini akan mengurangi latensi dan meningkatkan pemanfaatan jaringan, terutama untuk aplikasi dengan banyak permintaan kecil.
    - HTTP/2 menggunakan kompresi header untuk mengurangi overhead dengan mengompresi header HTTP, hal ini akan menyebabkan berkurangnya komsumsi bandwidth dan komunikasi yang lebih cepat antara klien dan server. 
    - HTTP/2 memiliki kemampuan untuk melakukan prioritas stream, sehingga memungkinkan klien menetapkan tingkat prioritas untuk permintaan yang berbeda, hal ini akan memastikan sumber daya yang penting akan dikirimkan lebih cepat.

    Kerugian :
      - HTTP/2 jauh lebih complex
      - Ada beberapa browser yang mungkin belum support HTTP/2
      - Berpotensi menghabiskan lebih banyak sumber daya server.
  
9.  REST API memiliki cara kerja yang berbeda dengan gRPC, REST API bekerja dengan cara client mengirim permintaan ke server dan server memberikan single respons untuk setiap permintaan, sementara gRPC dapat melakukan streaming dua arah, sehingga memungkinkan client dan server untuk saling berkomunikasi secara asinktron dan kontinu. Hal ini memungkinkan komunikasi real-time tanpa perlu polling berkala dari client ke server. Dalam responsivitas, kemampuan streaming dua arah dari gRPC memungkinkan komunikasi yang lebih responsif, selain itu gRPC akan lebih efisien dalam hal komunikasi real-time. Tetapi gRPC akan jauh lebih kompleks daripada penerapan REST API.

10. Penerapan gRPC menggunakan protocol buffer akan mememberikan pengaruh dalam ketatnya skema untuk definisi format pesan dan service interface, pengembang harus menentukan struktur dan jenis data yang dipertukarkan antara client dan service sehingga memastikan data yang dikirim sesuai dengan skema yang sudah dibuat, hal ini akan meningkatkan keselarasan antara client dan server serta memungkinkan pengembang untuk menghasilkan kode yang diderivasi selain itu penerapan gRPC akan membuat serialiasi biner yang lebih efisien. Sementara JSON dalam payload REST API lebih fleksibel dalam pemodelan dan representasi data karena memungkinkan struktur yang dinamis, namun hal ini dapat menyebabkan inkonsistensi dan ambiguitas dalam interpretasi dan pertukaran data.
