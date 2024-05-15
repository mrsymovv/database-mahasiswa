<?php
$host    = "localhost";
$user    = "root";
$pass    = "";
$db      = "akademik";

$koneksi = mysqli_connect($host,$user,$pass,$db);
if(!$koneksi){//cek koneksi
     die("Tidak bisa terkoneksi ke database");

}
$nim        ="";
$nama       ="";
$alamat     ="";
$prodi      ="";
$fakultas   ="";
$sukses     ="";
$error      ="";

if(isset($_GET['op'])){
    $op = $_GET['op'];
}else{
    $op = "";
}
if($op == 'delete'){
    $id         = $_GET['id'];
    $sql1       = "delete from mahasiswa where id = '$id'";
    $q1         = mysqli_query($koneksi,$sql1);
    if($q1){
        $sukses = "Berhasil hapus data";
    }else{
        $error  = "Gagal melakukan delete data";
    }
}
if($op == 'edit'){
    $id         = $_GET['id'];
    $sql1       = "select * from mahasiswa where id = '$id'";
    $q1         = mysqli_query($koneksi,$sql1);
    $r1         = mysqli_fetch_array($q1);
    $nim        = $r1['nim'];
    $nama       = $r1['nama'];
    $alamat     = $r1['alamat'];
    $prodi      = $r1['prodi'];
    $fakultas   = $r1['fakultas'];

    if($nim == ''){
        $error = "Data tidak di temukan";
    }
}
if(isset($_POST['simpan'])){ //untuk create
    $nim        = $_POST['nim'];
    $nama       = $_POST['nama'];
    $alamat     = $_POST['alamat'];
    $prodi      = $_POST['prodi'];
    $fakultas   = $_POST['fakultas'];

    if($nim && $nama && $alamat && $prodi && $fakultas){
       if($op == 'edit'){ //untuk update
           $sql1       = "update mahasiswa set nim = '$nim',nama ='$nama',alamat = '$alamat',prodi='$prodi',fakultas ='$fakultas' where id = '$id'";
           $q1         = mysqli_query($koneksi,$sql1);
           if($q1){
               $sukses = "Data berhasil di update";
           }else{
               $sukses = "Data gagal di update";
           }
       }else{ //untuk insert
          $sql1 = "insert into mahasiswa(nim,nama,alamat,prodi,fakultas) values ('$nim','$nama','$alamat','$prodi','$fakultas')";
          $q1    = mysqli_query($koneksi,$sql1);
          if ($q1) {
             $sukses    = "Berhasil memasukkan data baru";
          }else{
             $error     = "Gagal memasukkan data";
          }
        }
    }else{
        $error = "Silahkan masukkan semua data";
    }
             
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Mahasiswa</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">
    <style>
        .mx-auto { width:800px }
        .card { margin-top: 20px;}
    </style>
</head>


<body>
   <div class="mx-auto">
       <!-- untuk memasukkan data -->
       <div class="card">
           <div class="card-header">
               Create Database Mahasiswa / Edit Data
           </div>
           <div class="card-body">
               <?php
               if ($error) {
               ?>
                   <div class="alert alert-danger" role="alert">
                       <?php echo $error ?>
                   </div>
               <?php
                   header("refresh:5;url=index.php");//5 : detik
               }
               ?>
               <?php
               if ($sukses) {
               ?>
                   <div class="alert alert-success" role="alert">
                       <?php echo $sukses ?>
                   </div>
               <?php
                   header("refresh:5;url=index.php");
               }
               ?>
               <form action="" method="POST">
                   <div class="mb-3 row">
                        <label for="nim" class="col-sm-2 col-form-label">NIM</label>
                        <div class="col-sm-10">
                            <input type="text" class="form-control" id="nim" name="nim" value="<?php echo $nim ?>">
                        </div>
                   </div>
                   <div class="mb-3 row">
                        <label for="nama" class="col-sm-2 col-form-label">Nama</label>
                        <div class="col-sm-10">
                            <input type="text" class="form-control" id="nama" name="nama" value="<?php echo $nama ?>">
                        </div>
                   </div>
                   <div class="mb-3 row">
                        <label for="alamat" class="col-sm-2 col-form-label">Alamat</label>
                        <div class="col-sm-10">
                            <input type="text" class="form-control" id="alamat" name="alamat" value="<?php echo $alamat ?>">
                        </div>
                   </div>
                   <div class="mb-3 row">
                        <label for="prodi" class="col-sm-2 col-form-label">Prodi</label>
                        <div class="col-sm-10">
                            <select class="from-control" name="prodi" id="prodi">
                                <option value="">- Pilih Prodi -</option>
                                <option value="S1 Informatika" <?php if($prodi == "S1 Informatika") echo "selected"?>>S1 Informatika</option>
                                <option value="S1 Sistem Informasi" <?php if($prodi == "S1 Sistem Informasi") echo "selected"?>>S1 Sistem Informasi</option>
                                <option value="S1 Sistem Komputer" <?php if($prodi == "S1 Sistem Komputer") echo "selected"?>>S1 Sistem Komputer</option>
                                <option value="S1 Akutansi" <?php if($prodi == "S1 Akutansi") echo "selected"?>>S1 Akutansi</option>
                                <option value="S1 Manajemen" <?php if($prodi == "S1 Manajemen") echo "selected"?>>S1 Manajemen</option> 
                            </select>
                        </div>
                   </div>
                   <div class="mb-3 row">
                        <label for="fakultas" class="col-sm-2 col-form-label">Fakultas</label>
                        <div class="col-sm-10">
                            <select class="from-control" name="fakultas" id="fakultas">
                                <option value="">- Pilih Fakultas -</option>
                                <option value="Ilmu Komputer" <?php if($fakultas == "Ilmu Komputer") echo "selected"?>>Ilmu Komputer</option>
                                <option value="Fakultas Ekonomi dan Bisnis" <?php if($fakultas == "Fakultas Ekonomi dan Bisnis") echo "selected"?>>Fakultas Ekonomi dan Bisnis</option>
                            </select>
                        </div>
                   </div>
                   <div class="col-12">
                       <input type="submit" name="simpan" value="Simpan data" class="btn btn-primary"/>
               </form>
           </div>
       </div>

       <!-- untuk mengeluarkan data --> 
       <div class="card-header text-white bg-secondary">
               Data Mahasiswa
           </div>
           <div class="card-body">
               <table class="table">
                   <thead>
                       <tr>
                           <th scope="col">#</th>
                           <th scope="col">NIM</th>
                           <th scope="col">Nama</th>
                           <th scope="col">Alamat</th>
                           <th scope="col">Prodi</th>
                           <th scope="col">Fakultas</th>
                           <th scope="col">Aksi</th>
                       </tr>
                       <tbody>
                           <?php
                           $sql2   = "select * from mahasiswa order by id desc";
                           $q2     = mysqli_query($koneksi,$sql2);
                           $urut   = 1;
                           while($r2 = mysqli_fetch_array($q2)){
                               $id           = $r2['id'];
                               $nim          = $r2['nim'];
                               $nama         = $r2['nama'];
                               $alamat       = $r2['alamat'];
                               $prodi        = $r2['prodi'];
                               $fakultas     = $r2['fakultas'];

                               ?>
                               <tr>
                                   <th scope="row"><?php echo $urut++ ?></th>
                                   <td scope="row"><?php echo $nim ?></td>
                                   <td scope="row"><?php echo $nama ?></td>
                                   <td scope="row"><?php echo $alamat ?></td>
                                   <td scope="row"><?php echo $prodi ?></td>
                                   <td scope="row"><?php echo $fakultas ?></td>
                                   <td scope="row">
                                       <a href="index.php?op=edit&id=<?php echo $id?>"><button type="button" class="btn btn-warning">Edit</button></a>
                                       <a href="index.php?op=delete&id=<?php echo $id?>" onclick="return confirm('Yakin mau delete data?')"><button type="button" class="btn btn-danger">Delete</button></a>
                                       
                                
                                   </td>
                               </tr>
                               <?php

                           }
                           ?>
                       </tbody>
                   </thead>      
               </table>
           </div>
       </div>
   </div> 
</body>
</html>
