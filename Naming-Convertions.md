# Naming cheatsheet

1. [Thống nhất ngôn ngữ](#1-thống-nhất-ngôn-ngữ)
2. [Thống nhất quy ước](#2-thống-nhất-quy-ước)
   - [2.1 Theo chuẩn quy tắt Framework](#21-theo-chuẩn-quy-tắt-framework)
   - [2.2 Biến/hàm đúng như kết quả mong đợi đầu ra](#22-biếnhàm-đúng-như-kết-quả-mong-đợi-đầu-ra)
   - [2.4 Số ít và số nhiều](#24-số-ít-và-số-nhiều)
3. [Mô hình (tham khảo)](#3-mô-hình-tham-khảo)
   - [3.1 A/HC/LC (Hàm)](#31-ahclc-hàm)
   - [3.2 S-I-D (Short, Intuitive, Description)](#32-s-i-d-short-intuitive-description)

## 1. Thống nhất ngôn ngữ
- **Tiếng Anh** là ngôn ngữ chung từ nên cần thiết khi dùng trong lập trình.

```php
// Ví dụ tốt
$userName = "John Doe";
$friendsList = ['Alice', 'Bob'];

// Ví dụ xấu
$tenNguoiDung = "John Doe"; // Không nên dùng tiếng Việt
$dsBanBe = ['Alice', 'Bob']; // Không nên dùng tiếng Việt
```
[⬆ Quay lại Mục lục](#naming-cheatsheet)
 
## 2. Thống nhất quy ước

### 2.1 Theo chuẩn quy tắt Framework 
  - Trong Laravel, quy ước đặt tên mặc định là **camelCase** cho biến và **PascalCase** cho các lớp. Tuân thủ theo các quy ước này giúp mã dễ đọc và dễ bảo trì.
    ```php
    // Ví dụ tốt
    public function getUserProfile() {
        return $this->userProfile;
    }

    public function setUserProfile($profile) {
        $this->userProfile = $profile;
    }

    // Ví dụ xấu
    public function get_user_profile() { // không đúng quy ước camelCase
        return $this->user_profile;
    }

    public function SetUserProfile($profile) { // không đúng quy ước PascalCase
        $this->userProfile = $profile;
    }
    ```
[⬆ Quay lại Mục lục](#naming-cheatsheet)
 
### 2.2 Biến/hàm đúng như kết quả mong đợi đầu ra
  - Các tên hàm và biến nên phản ánh chính xác kết quả mà bạn mong đợi.
  - Dùng các tiền tố `is` `has` `should` `min/max` `prev/next` làm rõ chức năng hàm hoặc biến.
    ```php
    // Ví dụ tốt
    $isDisabled = $user->postCount <= 3;
    return view('button', ['disabled' => $isDisabled]);

    // Ví dụ xấu
    $isEnabled = $user->postCount > 3;
    return view('button', ['disabled' => !$isEnabled]); // Khó hiểu
    ```
[⬆ Quay lại Mục lục](#naming-cheatsheet)
 
### 2.4 Số ít và số nhiều
  - Sử dụng số ít cho các giá trị đơn.
  - Sử dụng số nhiều cho các tập hợp hoặc danh sách.
    ```php
    // Ví dụ tốt
    $friend = 'Alice';
    $friends = ['Alice', 'Bob', 'Charlie'];

    // Ví dụ xấu
    $friends = 'Alice'; // Không đúng vì là một chuỗi đơn
    $friend = ['Alice', 'Bob']; // Không đúng vì là một mảng
    ```
[⬆ Quay lại Mục lục](#naming-cheatsheet)

## 3. Mô hình (tham khảo)

### 3.1 A/HC/LC (Hàm)
  - **Action (A)**: Mô tả hành động của hàm, ví dụ `get`, `set`, `handle`.
  - **High Context (HC)**: Mô tả ngữ cảnh chung của đối tượng hoặc dữ liệu mà hàm hoạt động trên.
  - **Low Context (LC)**: Các chi tiết cụ thể hơn hoặc các thông số bổ sung cho hàm. 
    ```php
    // Ví dụ tốt
    public function getUserProfile($userId) {
        return User::find($userId);
    }
    public function handleUserLogin(Request $request) {}

    // Ví dụ xấu
    public function getProfile() { // Không rõ ràng về đối tượng nào
        return User::find($userId);
    }
    public function handleLogin() {} // Không có thông tin ngữ cảnh
    ```
[⬆ Quay lại Mục lục](#naming-cheatsheet)

### 3.2. S-I-D (Short, Intuitive, Description)
  - **Short (S)**: Tên ngắn gọn và dễ nhớ, nhưng không được quá rút gọn đến mức khó hiểu.
  - **Intuitive (I)**: Phản ánh đúng ý nghĩa, hiểu được mục đích ngay từ cái nhìn đầu tiên.
  - **Description (D)**: Mô tả chức năng.
    - **Không nên sử dụng viết tắt** trừ khi chúng rất phổ biến và dễ hiểu, chẳng hạn như `ID`, `URL`.
    ```php
    // Ví dụ tốt
    $postsCount = 10;
    $isAuthenticated = true;

    public function getUserDetails() {}

    // Ví dụ xấu
    $a = 10; // Không có ý nghĩa rõ ràng
    $isAuth = true; // Không rõ ràng như `isAuthenticated`
    public function getDetails() {} // Không rõ ràng, thiếu ngữ cảnh
    ```
[⬆ Quay lại Mục lục](#naming-cheatsheet)
