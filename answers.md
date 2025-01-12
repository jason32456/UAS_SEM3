### Jawaban Exercise UAS

**1. Contoh Penggunaan Trigger** Trigger sebaiknya digunakan saat ingin mengotomatisasi proses tertentu di database, seperti:

- Melakukan logging setiap ada perubahan data (INSERT, UPDATE, DELETE).
- Memastikan integritas data, misalnya mencegah nilai negatif pada kolom stok.
- Sinkronisasi data antar tabel.

**Contoh:**

```sql
CREATE TRIGGER after_insert_order
AFTER INSERT ON Orders
FOR EACH ROW
BEGIN
    INSERT INTO OrderLogs (OrderID, Action, Timestamp)
    VALUES (NEW.OrderID, 'INSERT', NOW());
END;
```

**2. Syntax Membuat Trigger**

```sql
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
FOR EACH ROW
BEGIN
    -- SQL Statements
END;
```

**3. Dua Keuntungan Menggunakan Stored Procedure**

- **Meningkatkan Kinerja:** Stored procedure dieksekusi lebih cepat karena sudah dikompilasi.
- **Keamanan:** Membatasi akses langsung ke tabel dengan memberikan izin hanya untuk menjalankan procedure.

**4. Situasi Lebih Cocok Menggunakan Function** Function lebih cocok digunakan saat membutuhkan nilai balik (return value) dan tidak melakukan modifikasi data. **Contoh:** Perhitungan pajak atau diskon.

```sql
CREATE FUNCTION HitungPajak(harga DECIMAL(10,2))
RETURNS DECIMAL(10,2)
BEGIN
    RETURN harga * 0.1;
END;
```

**5. Clustered dan Non-Clustered Index**

- **Clustered Index:** Mengatur penyimpanan fisik data sesuai urutan index. Setiap tabel hanya bisa memiliki satu clustered index.
- **Non-Clustered Index:** Memiliki struktur terpisah dari data dan berisi pointer ke data asli. Bisa lebih dari satu di tabel yang sama.

**Perbedaan:** Clustered index mengurutkan data fisik, sedangkan non-clustered hanya menyimpan referensi.

**6. Reading Uncommitted Data dan Overwriting Uncommitted Data**

- **Reading Uncommitted Data (Dirty Read):** Terjadi ketika transaksi membaca data yang belum di-*commit*.
  - **Pencegahan:** Gunakan isolation level `READ COMMITTED`.
- **Overwriting Uncommitted Data (Lost Update):** Terjadi ketika dua transaksi memperbarui data yang sama dan salah satu update tertimpa.
  - **Pencegahan:** Gunakan locking atau isolation level `REPEATABLE READ` atau `SERIALIZABLE`.

**7. Query Berdasarkan Tabel yang Diberikan**

a. **Email Domain @yahoo.com:**

```sql
SELECT FirstName, LastName, Email
FROM Customers
WHERE Email LIKE '%@yahoo.com';
```

b. **Menampilkan FirstName:**

```sql
SELECT FirstName
FROM Customers
WHERE FirstName IN ('John', 'Jane', 'Alic');
```

c. **Total Produk Dibeli per Pelanggan:**

```sql
SELECT C.FirstName, C.LastName, SUM(O.Quantity) AS TotalProduk
FROM Orders O
JOIN Customers C ON O.CustomerID = C.CustomerID
GROUP BY C.FirstName, C.LastName;
```

d. **Sales dan Rata-rata Sales per Tanggal:**

```sql
SELECT OrderDate, SUM(TotalAmount) AS Sales, AVG(TotalAmount) AS Average_Sales
FROM Orders
GROUP BY OrderDate;
```

e. **Produk Terjual Lebih dari 3:**

```sql
SELECT P.ProductName, SUM(O.Quantity) AS NUMBER_OF_SOLD
FROM Orders O
JOIN Products P ON O.ProductID = P.ProductID
GROUP BY P.ProductName
HAVING SUM(O.Quantity) > 3;
```

f. **Order dengan Quantity di Atas Rata-rata:**

```sql
SELECT *
FROM Orders
WHERE Quantity > (SELECT AVG(Quantity) FROM Orders);
```

g. **Produk Terjual dengan Harga di Atas Rata-rata:**

```sql
SELECT P.ProductName, P.Price
FROM Products P
JOIN Orders O ON P.ProductID = O.ProductID
WHERE P.Price > (SELECT AVG(Price) FROM Products);
```

h. **Pesanan di Antara Tanggal dan Lebih dari Satu Order:**

```sql
SELECT O.OrderID, O.ProductID, O.TotalAmount
FROM Orders O
WHERE O.OrderDate BETWEEN '2023-05-10' AND '2023-05-11'
AND O.CustomerID IN (
    SELECT CustomerID
    FROM Orders
    GROUP BY CustomerID
    HAVING COUNT(OrderID) > 1
);
```

i. **Order Produk Harga >100 dan Stok <20, Nama Mengandung 'a':**

```sql
SELECT O.OrderID, P.ProductName, C.FirstName
FROM Orders O
JOIN Products P ON O.ProductID = P.ProductID
JOIN Customers C ON O.CustomerID = C.CustomerID
WHERE C.FirstName LIKE '%a%'
  AND P.Price > 100
  AND P.Quantity < 20;
```

j. **Order Pelanggan dengan Email 'yahoo.com' untuk Produk Tertentu:**

```sql
SELECT O.OrderID, P.ProductName, C.LastName
FROM Orders O
JOIN Products P ON O.ProductID = P.ProductID
JOIN Customers C ON O.CustomerID = C.CustomerID
WHERE C.Email LIKE '%@yahoo.com'
  AND P.ProductName IN ('Laptop', 'Tablet', 'Printer');
```

