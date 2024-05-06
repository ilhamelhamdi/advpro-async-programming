# Advance Programming - Module 10 : Async Programming

Name  : Ilham Abdillah Alhamdi <br>
NPM   : 2206081194 <br>
Class : Advpro - A <br>

## Reflection
### 1.2. Understanding how it works

Setelah ditambahkan _statement_ `println!("Ilham's Computer: hey hey");` di luar _async block_, pesan "hey hey" muncul pertama kali, kemudian diikuti oleh pesan "howdy!" serta diakhiri oleh pesan "done!"

![](./assets/images/commit-1.2.png)

Hal ini bisa terjadi karena _async block_ dimana pesan "howdy!" dan "done!" akan dieksekusi pada saat `executor.run()` dijalankan. Pesan "hey hey" sendiri dijalankan sebelum `executor.run()` dijalankan. Dengan demikian pesan "hey hey" seolah-olah mendahului kedua pesan lainnya, meski dituliskan di baris terakhir.

Adapun terkait bagaimana _async block_ tersebut bekerja, berikut merupakan penjelasannya.

- Async block di-passing sebagai argument di `spawner.spawn`
- Ketika method `spawn` dipanggil, objek spawner mengirimkan objek Task yang berisikan future berupa _async block_ sebelumnya ke executor melalui _channel queue_.
- Objek Task tersebut kemudian disimpan dalam _channel queue_ dan siap untuk diterima oleh executor sebagai _receiver_-nya.
- Executor dijalankan. Pada saat dijalankan, tersedia satu task pada _channel queue_, yaitu task yang dikirimkan oleh spawner sebelumnya. Executor berjalan terus sampai task dalam queue tersebut habis dan tidak ada _transmitter_ lainnya.
- Saat executor dijalankan, pesan "howdy!" langsung muncul karena langsung dijalankan tanpa menunggu.
- Saat executor sampai pada keyword `await`, task tersebut menunggu state dari `TimerFuture` jika statusnya masih `Poll:Pending`.
- Pada fungsi `TimerFuture::new`, ketika waktu durasinya sudah selesai, variabel `completed` di-set true kemudian memanggil `waker.wake`. Executor kemudian akan melanjutkan kode yang tadi sempat tertunda dan melakukan `poll` kembali, namun kali ini dengan status `Poll:Ready`. Timer selama 2 detik pun selesai dan pesan "done!" ditampilkan.




