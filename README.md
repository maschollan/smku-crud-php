# code 1 (SETUP HTML)

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

# code 2
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
```
```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
    
```
# code 3

```html
<div class="container-fluid p-5 bg-primary text-white text-center">
    <h1>SMK KU</h1>
    <p>Selamat Datang di Website SMK KU</p>
</div>

<div class="container py-3">
    <div class="card">
        <div class="card-body">
            <h2>Data Siswa :</h2>
            <p>siswa SMK ku yang berjumlah 2 siswa</p>
        </div>
    </div>
</div>
```

# code 4 (CONNECT DB)

```php
<?php

$server = "localhost";
$user = "root";
$pass = "";
$nama_db = "smku";

$db = mysqli_connect($server, $user, $pass, $nama_db);

if (!$db) {
    die("Gagal terhubung dengan database: " . mysqli_connect_error());
}
```

# code 5 (SHOW DATA)
```php
<?php
include("config.php");

$sql = "SELECT * FROM siswa";
$query = mysqli_query($db, $sql);
?>
```

# code 6
```html
<p>siswa SMK ku yang berjumlah <?= mysqli_num_rows($query) ?> siswa</p>
```

# code 7

```html
<table class="table">
    <thead>
        <tr>
            <th>NO</th>
            <th>Nama</th>
            <th>Kelas</th>
            <th>Alamat</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody>

    </tbody>
</table>
```

# code 8

```php
<?php
$no = 1;
while($siswa = mysqli_fetch_array($query)){
    $id = $siswa['id'];
    $nama = $siswa['nama'];
    $kelas = $siswa['kelas'];
    $alamat = $siswa['alamat'];

    echo "<tr>";
    echo "<td>".$no++."</td>";
    echo "<td>$nama</td>";
    echo "<td>$kelas</td>";
    echo "<td>$alamat</td>";
    echo "<td>
        <btn class='btn btn-success'>edit</btn>
        <btn class='btn btn-danger'>hapus</btn>
    </td>";
    echo "</tr>";
}
?>
```

# code 9 (ADD DATA)
```html
<button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#tambah-modal">Tambah</button>
```

# code 10
```html
<div class="modal fade" id="tambah-modal">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header">
                <h5>Tambah Siswa</h5>
                <button class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <form action="tambah.php" method="post">
                    <div class="mb-3">
                        <label for="nama" class="form-label">Nama :</label>
                        <input type="text" name="nama" id="nama" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label for="kelas" class="form-label">Kelas :</label>
                        <input type="text" name="kelas" id="kelas" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label for="alamat" class="form-label">Alamat : </label>
                        <textarea name="alamat" id="alamat" class="form-control"></textarea>
                    </div>
                    <button type="submit" class="btn btn-primary" name="tambah">Tambah</button>
                </form>
            </div>
        </div>
    </div>
</div>
```

# code 11
```php
<?php
include("config.php");

if(isset($_POST['tambah'])){
    $nama = $_POST['nama'];
    $kelas = $_POST['kelas'];
    $alamat = $_POST['alamat'];

    $sql = "INSERT INTO siswa (nama, kelas, alamat) VALUE ('$nama', '$kelas', '$alamat')";
    $query = mysqli_query($db, $sql);

    if( $query ) {
        header('Location: index.php');
    } else {
        die("Gagal menyimpan perubahan...");
    }
} else {
    die("Akses dilarang...");
}
```

# code 12 (EDIT DATA)
```php
$edit = "edit(\"$id\", \"$nama\", \"$kelas\", \"$alamat\")";

```

```html
<btn class='btn btn-success' onclick='$edit'>edit</btn>
```

# code 13
```html
<div class="modal fade" id="edit-modal">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header">
                <h5>Edit Siswa</h5>
                <button class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <form action="edit.php" method="post">
                    <input type="hidden" name="id" id="id-edit">
                    <div class="mb-3">
                        <label for="nama" class="form-label">Nama :</label>
                        <input type="text" name="nama" id="nama-edit" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label for="kelas" class="form-label">Kelas :</label>
                        <input type="text" name="kelas" id="kelas-edit" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label for="alamat" class="form-label">Alamat : </label>
                        <textarea name="alamat" id="alamat-edit" class="form-control"></textarea>
                    </div>
                    <button type="submit" class="btn btn-primary" name="edit">Edit</button>
                </form>
            </div>
        </div>
    </div>
</div>
```

# code 14
```js
const modalEdit = new bootstrap.Modal(document.getElementById('edit-modal'));

function edit(id, nama, kelas, alamat){
    document.getElementById('id-edit').value = id;
    document.getElementById('nama-edit').value = nama;
    document.getElementById('kelas-edit').value = kelas;
    document.getElementById('alamat-edit').value = alamat;
    modalEdit.show();
}
```

# code 15 
```php
<?php
include("config.php");

if(isset($_POST['edit'])) {
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kelas = $_POST['kelas'];
    $alamat = $_POST['alamat'];

    $sql = "UPDATE siswa SET nama='$nama', kelas='$kelas', alamat='$alamat' WHERE id=$id";
    $query = mysqli_query($db, $sql);

    if( $query ) {
        header('Location: index.php');
    } else {
        die("Gagal menyimpan perubahan...");
    }
} else {
    die("Akses dilarang...");
}
```

# code 16 (DELETE DATA)
```php
$hapus = "hapus(\"$id\")";
```

```html
<btn class='btn btn-danger' onclick='$hapus'>hapus</btn>
```

# code 17
```html
<form action="hapus.php" method="post" id="form-hapus">
    <input type="hidden" name="id" id="id-hapus"/>
    <input type="hidden" name="hapus" value="hapus"/>
</form>
```

# code 18 
```js
function hapus(id){
    if(confirm('Apakah anda yakin ingin menghapus data ini?')){
        document.getElementById('id-hapus').value = id;
        document.getElementById('form-hapus').submit();
    }
}
```

# code 19
```php
<?php
include("config.php");

if(isset($_POST['hapus'])){
    $id = $_POST['id'];

    $sql = "DELETE FROM siswa WHERE id=$id";
    $query = mysqli_query($db, $sql);

    if( $query ) {
        header('Location: index.php');
    } else {
        die("Gagal menyimpan perubahan...");
    }
} else {
    die("Akses dilarang...");
}
```

# code 20 (LOGIN)
```html
<button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#login-modal">Login</button>
```

# code 21
```html
<div class="modal fade" id="login-modal">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header">
                <h5>Login Modal</h5>
                <button class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <div class="mb-3">
                    <label for="username" class="form-label">Username :</label>
                    <input type="text" name="username" id="username" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="password" class="form-label">Password :</label>
                    <input type="password" name="password" id="password" class="form-control">
                </div>
                <button class="btn btn-primary" onclick="login()">Login</button>
            </div>
        </div>
    </div>
</div>
```

# code 22
```js
function login() {
    let username = document.getElementById('username').value;
    let password = document.getElementById('password').value;
    fetch('login.php', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        },
        body: `username=${username}&password=${password}`
    }).then(response => response.json()).then(response => {
        if(response.status == 'berhasil') {
            alert("Berhasil login...Selamat datang " + response.data.nama);
        } else {
            alert(response.message);
        }
    })
}
```

# code 23
```php
<?php
include("config.php");
header('Content-Type:  application/json');

if(isset($_POST['username']) && isset($_POST['password'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];

    $sql = "SELECT * FROM user WHERE username='$username' AND password='$password'";
    $query = mysqli_query($db, $sql);

    if($query) {
        if(mysqli_num_rows($query) == 0) {
            echo json_encode([
                'status' => 'gagal',
                'message' => 'username atau password salah...'
            ]);
        } else {
            $user = mysqli_fetch_assoc($query);
            echo json_encode([
                'status' => 'berhasil',
                'message' => 'Berhasil membuat login...',
                'data' => $user
            ]);
        }
    } else {
        echo json_encode([
            'status' => 'gagal',
            'message' => 'Gagal membuat login...'
        ]);
    }
} else {
    echo json_encode([
        'status' => 'gagal',
        'message' => 'Akses dilarang...'
    ]);
}
```
