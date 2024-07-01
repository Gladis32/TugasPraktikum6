# Tugas Praktikum 6 { Pertemuan ke 14 } <img src=https://logos-download.com/wp-content/uploads/2016/05/MySQL_logo_logotype.png width="130px" >

|**Nama**|**NIM**|**Kelas**|**Matkul**|
|----|---|-----|------|
|Gladis Toti Anggraini |312310566|TI.23.A5|Basis Data|

## ER-D

![erd](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/dc847885-3eb4-40a9-a177-28358b8c09c1)
![erdiagram](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/94adf07a-7ecf-4fba-b68a-bae46261453a)

## INPUT DATA
![tabel input](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/9fbe14b8-a97b-411e-b428-07aa64a79c44)

### 1. Perusahaan 
```
CREATE TABLE Perusahaan(
id_p VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
alamat VARCHAR(45)
);

INSERT INTO Perusahaan VALUES
('P01', 'Kantor Pusat', NULL),
('P02', 'Cabang Bekasi', NULL);

SELECT * FROM Perusahaan;
```

![tabel1](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/4e69281c-8806-416c-ae34-0e6cfbeb5c1a)


### 2. Departemen
```
CREATE TABLE Departemen(
id_dept VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
id_p VARCHAR(10) NOT NULL,
sup_nik VARCHAR(10),

INSERT INTO Departemen VALUES
('D01', 'Produksi', 'P02', 'N01'),
('D02', 'Marketing', 'P01', 'N03'),
('D03', 'RnD', 'P02', NULL),
('D04', 'Logistik', 'P02', NULL);

SELECT * FROM Departemen;
```

![tabel2](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/a7996176-bc9c-4325-9bc4-42b82e208efa)

### 3. Karyawan
```
CREATE TABLE Karyawan (
nik VARCHAR(10) PRIMARY KEY,
nama VARCHAR(50),
id_dept VARCHAR(10),
sup_nik VARCHAR(10),
gaji_pokok INT,
FOREIGN KEY (id_dept) REFERENCES Departemen(id_dept),
FOREIGN KEY (sup_nik) REFERENCES Karyawan(nik)
);

INSERT INTO Karyawan (nik, nama, id_dept, sup_nik, gaji_pokok) VALUES
('N01', 'Ari', 'D01', NULL, 2000000),
('N02', 'Dina', 'D01', NULL, 2500000),
('N03', 'Rika', 'D03', NULL, 2400000),
('N04', 'Ratih', 'D01', 'N01', 3000000),
('N05', 'Riko', 'D01', 'N01', 2800000),
('N06', 'Dani', 'D02', NULL, 2100000),
('N07', 'Anis', 'D02', 'N06', 5000000),
('N08', 'Dika', 'D02', 'N06', 4000000),
('N09', 'Raka', 'D03', 'N06', 2000000);

SELECT * FROM Karyawan;
```

![tabel3](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/c86a2cd6-fe1e-4dae-8e97-3067b773f4e5)

### 4. Project
```
CREATE TABLE Project(
id_proj VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
tgl_mulai DATE,
tgl_selesai DATE,
status INT
);

INSERT INTO Project VALUES
('PJ01', 'A', '2019-01-10', '2019-03-10', '1'),
('PJ02', 'B', '2019-02-15', '2019-04-10', '1'),
('PJ03', 'C', '2019-03-21', '2019-05-10', '1');

SELECT * FROM Project;
```

![tabel4](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/50d5c1d9-7670-4032-8187-25b6d6a5ddcd)

### 5. Project Detail
```
CREATE TABLE Project_detail(
id_proj VARCHAR(10),
nik VARCHAR(10),
PRIMARY KEY (id_proj, nik),
FOREIGN KEY (id_proj) REFERENCES Project(id_proj),
FOREIGN KEY (nik) REFERENCES Karyawan(nik)
);


INSERT INTO Project_detail VALUES
('PJ01', 'N01'),
('PJ01', 'N02'),
('PJ01', 'N03'),
('PJ01', 'N04'),
('PJ01', 'N05'),
('PJ01', 'N07'),
('PJ01', 'N08'),
('PJ02', 'N01'),
('PJ02', 'N03'),
('PJ02', 'N05'),
('PJ03', 'N03'),
('PJ03', 'N07'),
('PJ03', 'N08');

SELECT * FROM Project_detail;
```

![tabel5](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/daaccd49-00d1-465f-b39f-bb2846e68456)

## LATIHAN

### 1. Departemen Apa Saja Yang Terlibat Dalam Tiap-tiap Project.

```
SELECT Project.nama AS Project, GROUP_CONCAT(Departemen.nama) AS Departemen
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
GROUP BY Project.id_proj;
```

`Inner Join` Menggabungkan tabel **Project** dengan tabel **Project_detail** berdasarkan kolom **id_proj** dan menggabungkan tabel **Project_detail** dengan tabel Karyawan berdasarkan kolom **nik**. `GROUP_CONCAT()` untuk menggabungkan nama departemen yang terlibat dalam satu baris berdasarkan **id_proj**.

![1](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/92f06789-2d9d-4d52-a683-aa1788bd74ce)


### 2. Jumlah Karyawan Tiap Departemen Yang Bekerja Pada Tiap-tiap Project.

```
SELECT Project.nama AS Project, Departemen.nama AS Departemen, COUNT(*) AS 'Jumlah Karyawan'
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
GROUP BY Project.id_proj, Departemen.id_dept;
```
`COUNT(*)` untuk menghitung jumlah karyawan dan `GROUP BY` untuk mengelompokkan hasil berdasarkan **id_proj** dan **id_dept**.

![2](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/64489032-9a37-4957-8f68-1ea16202d275)


### 3. Ada Berapa Project Yang Sedang Dikerjakan Oleh Departemen ***RnD***? (ket: project berjalan adalah yang statusnya 1).

```
SELECT COUNT(*) AS 'Jumlah Project'
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
WHERE Departemen.nama = 'RnD' AND Project.status = 1;
```

Untuk menghitung jumlah project yang sedang dikerjakan oleh departemen dengan nama **'RnD'**. Menggunakan `COUNT(*)` untuk menghitung jumlah project dan filter menggunakan `WHERE` untuk nama departemen **'RnD'** dan status project yang sedang berjalan **(Project.status = 1)**.

![3](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/39ddc2e1-2414-428f-af1a-64c3f5175576)


### 4. Berapa banyak Project yang sedang dikerjakan oleh Ari ?

```
SELECT COUNT(*) AS 'Jumlah Project'
FROM Project_detail
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
WHERE Karyawan.nama = 'Ari' AND Project_detail.id_proj IN (SELECT id_proj FROM Project WHERE status = 1);
```
- Fungsi `COUNT(*)` untuk menghitung jumlah baris (project) yang sesuai dengan kriteria yang ditentukan. `FROM Project_detail` mengambil data dari tabel Project_detail, yang berisi hubungan antara project *(id_proj)* dan karyawan *(nik)* yang terlibat dalam project. `INNER JOIN` Karyawan *ON Project_detail.nik = Karyawan.nik** melakukan *INNER JOIN* antara Project_detail dan Karyawan berdasarkan kolom *nik* untuk mengakses informasi karyawan berdasarkan nik mereka yang terdaftar dalam tabel Project_detail. 
- Menggunakan kondisi `WHERE` untuk memfilter baris-baris di mana nama karyawan adalah 'Ari'. `AND Project_detail.id_proj IN (SELECT id_proj FROM Project WHERE status = 1)` kondisi ini memastikan bahwa hanya project dengan status berjalan **(status = 1) yang akan dihitung**. Subquery
- `SELECT id_proj FROM Project WHERE status = 1` digunakan untuk mengambil semua id_proj dari tabel Project yang memiliki status berjalan.

![4](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/71b487a4-3523-400a-9c78-c4d4e7d551ca) 


### 5. Siapa Saja Yang Mengerjakan Project B ?

```
SELECT Karyawan.nama
FROM Project_detail
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
WHERE Project_detail.id_proj IN (SELECT id_proj FROM Project WHERE nama = 'B');
```

Untuk menampilkan nama-nama karyawan yang sedang mengerjakan project dengan nama **'B'**. Menggunakan `INNER JOIN` antara **Project_detail** dan Karyawan berdasarkan **nik** dan filter menggunakan `WHERE` untuk memilih project dengan nama **'B'** berdasarkan **id_proj**.

![5](https://github.com/Gladis32/TugasPraktikum6/assets/148181064/d016ff82-43cd-42b5-8ecf-6a6bc7a776d9) 
