# THIẾT KẾ KIẾN TRÚC LƯU TRỮ VÀ PHÂN QUYỀN GOOGLE DRIVE
**Dự án:** CampusPay  
**Nguyên tắc chỉ đạo:** Phân quyền tối thiểu (Least Privilege) & Giảm thiểu rủi ro rò rỉ dữ liệu

---

## Phần 1: Phân tích bài toán (I/O)

### 1. Xác định Đầu vào & Đầu ra (Input/Output)

* **Đầu vào (Input):**
  * **Nhân sự & Vai trò:** 
    * Ban Kỹ thuật (3 người): Viết code, test API, quản lý mã nguồn.
    * Ban Thiết kế (2 người): Thiết kế UI, tạo tài sản đồ họa.
    * Ban Kinh doanh (2 người): Viết Business Plan, liên hệ đối tác.
    * Nhà đầu tư tiềm năng (1 người ngoài): Cần xem Business Plan và Slide Pitch.
  * **Loại tài liệu cần quản lý:** Mã nguồn, API keys/Config, Tài sản đồ họa UI/UX, Business Plan (chi tiết & tóm tắt), Slide Pitch, Tài liệu quản lý chung.
  * **Yêu cầu nghiệp vụ (5 nguyên tắc):** Phân quyền tối thiểu, Không dùng `Anyone with the link` cho file nhạy cảm, Cộng tác linh hoạt nhưng soát quyền sửa, Version History $\ge$ 30 ngày, Tự động thu hồi quyền nhà đầu tư sau 14 ngày (Expiration Date).

* **Đầu ra mong đợi (Output):**
  * Mô hình cấu trúc thư mục 2 cấp minh bạch, dễ quản lý.
  * Quy trình 6 bước lập cấu hình chuẩn hóa.
  * Bảng phân quyền 6 cột (4 vai trò $\times$ 5 loại tài liệu).
  * Giải pháp bảo mật cụ thể cho 3 tình huống bẫy dữ liệu.

---

### 2. Đề xuất giải pháp: Cấu trúc thư mục Google Drive 2 cấp

Toàn bộ hệ thống được đặt dưới Thư mục gốc dự án (`CampusPay_Project_Root`) thuộc quyền sở hữu của Tài khoản Quản trị trung tâm (Admin/Owner), không sở hữu bởi cá nhân để tránh mất dữ liệu khi nhân sự nghỉ việc.

```text
CampusPay_Project_Root/
├── 01_General_Management/           (Cấp 2 - Quản lý chung)
├── 02_Engineering_Dept/             (Cấp 2 - Ban Kỹ thuật)
│   └── Restricted_Secrets/          (Thư mục con chứa API Keys/Env - Siêu mật)
├── 03_Design_Dept/                  (Cấp 2 - Ban Thiết kế)
├── 04_Business_Dept/                (Cấp 2 - Ban Kinh doanh)
└── 05_External_Shared/              (Cấp 2 - Chia sẻ đối tác ngoài)
    └── Investor_Pitch_Package/      (Tài liệu cấp cho Nhà đầu tư)
