[Thu Mar 12 13:40:20 2026] PHP Parse error:  syntax error, unexpected token "foreach" in D:\UIT_Subject\LTW\BTLT_2\web-development-\Buggy\pages\customers.php on line 5
[Thu Mar 12 13:40:20 2026] [::1]:34758 [500]: GET /?page=customers - syntax error, unexpected token "foreach" in D:\UIT_Subject\LTW\BTLT_2\web-development-\Buggy\pages\customers.php on line 5

[Thu Mar 12 13:40:22 2026] PHP Parse error:  syntax error, unexpected token "{", expecting "->" or "?->" or "[" in D:\UIT_Subject\LTW\BTLT_2\web-development-\Buggy\pages\reports.php on line 15
[Thu Mar 12 13:40:22 2026] [::1]:61864 [500]: GET /?page=reports - syntax error, unexpected token "{", expecting "->" or "?->" or "[" in D:\UIT_Subject\LTW\BTLT_2\web-development-\Buggy\pages\reports.php on line 15

[Thu Mar 12 13:40:24 2026] PHP Parse error:  syntax error, unexpected single-quoted string "language", expecting "]" in D:\UIT_Subject\LTW\BTLT_2\web-development-\Buggy\pages\settings.php on line 6
[Thu Mar 12 13:40:24 2026] [::1]:25277 [500]: GET /?page=settings - syntax error, unexpected single-quoted string "language", expecting "]" in D:\UIT_Subject\LTW\BTLT_2\web-development-\Buggy\pages\settings.php on line 6

[Dashboard]
Duyet them pending
Dieu kien cua lowStock
Tinh tong tien thieu itemprice

[Checkout]
Discount bi am tien

[Order]
Sort bi sai
Chinh lai logic status pending only
# 📝 BÁO CÁO FIX BUG & TỐI ƯU HÓA HỆ THỐNG (12/03/2026)

## 🛠 I. PHÂN TÍCH VÀ SỬA LỖI CÚ PHÁP (LOG ERRORS)

### 1. Lỗi Parse Error tại `customers.php` (Line 5)
* **Log:** `unexpected token "foreach"`
* **Nguyên nhân:** Thiếu dấu chấm phẩy (`;`) ở dòng khai báo biến ngay trước câu lệnh `foreach`.
* **Cách khắc phục:** ```php
    // Trước: $activeCustomers = []
    $activeCustomers = []; // Thêm dấu ;
    foreach ($customers as $customer) { ... }
    ```

### 2. Lỗi Parse Error tại `reports.php` (Line 15)
* **Log:** `unexpected token "{", expecting ")"`
* **Nguyên nhân:** Sai cấu trúc lệnh `foreach`, thiếu dấu đóng ngoặc đơn `)` để kết thúc khai báo vòng lặp.
* **Cách khắc phục:**
    ```php
    // Trước: foreach ($totalsByCategory as $category => $total {
    foreach ($totalsByCategory as $category => $total) { // Đóng ngoặc )
    ```

### 3. Lỗi Parse Error tại `settings.php` (Line 6)
* **Log:** `unexpected single-quoted string "language", expecting "]"`
* **Nguyên nhân:** Quên dấu phẩy (`,`) ngăn cách giữa các phần tử trong mảng (array).
* **Cách khắc phục:**
    ```php
    $config = [
        'theme' => 'dark', // Đảm bảo có dấu phẩy ở cuối mỗi phần tử
        'language' => 'vi'
    ];
    ```

---

## 📊 II. LOGIC DASHBOARD & INVENTORY

### 4. Hiển thị thêm các đơn hàng "Pending"
* **Vấn đề:** Dashboard hiện tại đang bỏ qua các đơn hàng đang chờ duyệt.
* **Giải pháp:** Cập nhật hàm đếm để bao gồm cả trạng thái `pending`.
    ```php
    $pendingOrders = array_filter($allOrders, fn($o) => $o['status'] === 'pending');
    $pendingCount = count($pendingOrders);
    ```

### 5. Điều kiện cảnh báo "Low Stock"
* **Vấn đề:** Logic kiểm tra hàng tồn kho đang quá đơn giản (có thể chỉ check == 0).
* **Giải pháp:** Thiết lập ngưỡng
