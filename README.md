<!DOCTYPE html>
<html>
<head>
    <title>Form Pembelian Seblak</title>
    <style>
        /* Efek bayangan dan bingkai untuk kontainer */
        .container {
            width: 400px;
            margin: 0 auto;
            padding: 20px;
            background: #f9f9f9;
            border: 1px solid #ccc;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        /* Tampilan CSS untuk elemen-elemen formulir */
        #total-harga {
            display: none;
            margin-top: 20px;
        }

        /* Tampilan CSS untuk tombol "Selanjutnya" */
        #btn-selanjutnya {
            background-color: #0077cc;
            color: #fff;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
        }

        /* Tampilan CSS untuk tombol "Bayar" */
        #btn-bayar {
            background-color: #0077cc;
            color: #fff;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
        }

        /* CSS untuk mengatur checkbox sejajar ke bawah */
        .toping-label {
            display: block;
            margin-bottom: 10px;
        }
        #downloaded-image {
            background: #0d08ff;
            border: 1px solid #150202;
            box-shadow: 0 0 10px rgba(7, 211, 99, 0.1);
        }
        #btn-download {
            background-color: #0077cc;
            color: #fff;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            margin: 20px auto; /* Mengatur margin untuk tombol agar berada di tengah */
            display: block;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            transition: background 0.3s, box-shadow 0.3s;
        }

        #btn-download:hover {
            background-color: #005799; /* Warna latar belakang berubah saat digulirkan */
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Efek bayangan berubah saat digulirkan */
        }
        
    </style>
</head>
<body>
    <div class="container">
        <h1 style="background: #0077cc; color: #fff; padding: 10px;">Form Pembelian Seblak</h1>

        <div id="jumlah-porsi-container">
            <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="post">
                <label for="jumlah_porsi">Jumlah Porsi Seblak:</label>
                <input type="number" id="jumlah_porsi" name="jumlah_porsi" required><br><br>
                <button type="button" id="btn-selanjutnya">Selanjutnya</button>
            </form>
        </div>

        <div id="toping-container" style="display: none;">
            <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="post">
                <label for="toping">Pilih Toping:</label>
                <label class="toping-label">
                    <input type="checkbox" id="sosis" name="toping[]" value="Sosis">
                    Sosis (Rp 2,000)
                </label>
                <label class="toping-label">
                    <input type="checkbox" id="bakso" name="toping[]" value="Bakso">
                    Bakso (Rp 3,000)
                </label>
                <label class="toping-label">
                    <input type="checkbox" id="ikan" name="toping[]" value="Ikan">
                    Ikan (Rp 1,000)
                </label><br><br>

                <label for="metode_pembayaran">Metode Pembayaran:</label>
                <select id="metode_pembayaran" name="metode_pembayaran">
                    <option value="tunai">Tunai</option>
                </select><br><br>
                <input type="submit" value="Bayar" id="btn-bayar">
            </form>
        </div>

        <div id="total-harga">
            <h2>Total Harga Seblak dan Toping yang Anda beli:</h2>
            <p id="total-harga-tekst"></p>
            <button id="btn-download" style="display: none;">Unduh Gambar</button>
        </div>
        <p id="whatsapp-link" style="display: none;">Kirim pesan ke Kumiko di WhatsApp: <a href="https://wa.me/6282170710590">https://wa.me/6282170710590</a></p>
    </div>

    <script>
        document.getElementById("btn-selanjutnya").addEventListener("click", function() {
            var jumlah_porsi = parseInt(document.getElementById('jumlah_porsi').value);

            // Validasi bahwa jumlah porsi harus diisi
            if (isNaN(jumlah_porsi) || jumlah_porsi <= 0) {
                alert("Harap isi jumlah porsi dengan benar.");
                return;
            }

            // Mengaktifkan elemen berikutnya
            document.getElementById('jumlah-porsi-container').style.display = "none";
            document.getElementById('toping-container').style.display = "block";
        });

        document.getElementById("btn-bayar").addEventListener("click", function(event) {
            event.preventDefault();

            // Menerima data dari formulir
            var jumlah_porsi = parseInt(document.getElementById('jumlah_porsi').value);
            var metode_pembayaran = document.getElementById('metode_pembayaran').value;
            var topingCheckboxes = document.querySelectorAll('input[name="toping[]"]:checked');
            var toping = [];
            topingCheckboxes.forEach(function(checkbox) {
                toping.push(checkbox.value);
            });

            // Menghitung total harga toping
            var harga_toping = 0;
            toping.forEach(function(item) {
                if (item === "Sosis") {
                    harga_toping += 2000;
                } else if (item === "Bakso") {
                    harga_toping += 3000;
                } else if (item === "Ikan") {
                    harga_toping += 1000;
                }
            });

            // Menghitung total harga
            var harga_per_porsi = 15000;
            var total_harga_seblak = jumlah_porsi * harga_per_porsi;
            var total_harga = total_harga_seblak + harga_toping;

            // Menampilkan total harga dan toping yang dibeli
            document.getElementById('total-harga-tekst').textContent = "Rp " + total_harga;
            document.getElementById('total-harga').style.display = "block";

            // Membuat gambar dari total harga dan toping
            var canvas = document.createElement('canvas');
            canvas.width = 400;
            canvas.height = 150;
            var context = canvas.getContext('2d');
            context.fillStyle = '#f9f9f9';
            context.fillRect(0, 0, canvas.width, canvas.height);
            context.font = '16px Arial';
            context.fillStyle = '#000';
            context.fillText('Total Harga Seblak dan Toping yang Anda beli:', 20, 30);
            context.fillText('Rp ' + total_harga, 20, 60);
            context.fillText('Toping yang Anda beli: ' + toping.join(', '), 20, 90);

            // Menyembunyikan tombol Bayar dan menampilkan tombol Unduh Gambar
            document.getElementById('btn-bayar').style.display = 'none';
            document.getElementById('btn-download').style.display = 'block';
            document.getElementById('whatsapp-link').style.display = 'block';

            document.getElementById('btn-download').addEventListener('click', function() {
                var image = canvas.toDataURL('image/png');
                var a = document.createElement('a');
                a.href = image;
                a.download = 'struk_pembelian.png';
                a.click();

            });
        });
    </script>
</body>
</html>
