### 

### **Bài 7: Gỡ Lỗi Luồng Yêu Cầu Kết Quả Xét Nghiệm Và Sai Lệch Mã Trạng Thái HTTP** 

### **Phần 1: Phân tích vấn đề**

1\.**Chỉ ra 4 điểm sai lệch về mặt nguyên lý kỹ thuật trong bản giải trình của bạn thực tập sinh (về vai trò DNS, quy trình TCP/TLS, hiểu sai mã lỗi HTTP và hiểu lầm về việc sửa DOM trên Tab Elements).**

- Sai về DNS: trình duyệt không thể gửi trực tiếp chuỗi URL tới Web Server nếu chưa qua DNS,  phải nhờ DNS dịch tên miền thành địa chỉ IP của máy chủ trước thì mới gửi đc yêu cầu qua máy chủ.  
    
- Sai về TCP/TLS: giao thức HTTPS bắt buộc phải thực hiện bắt tay TCP/TLS thì mới thiết lập được kết nối an toàn để tải dữ liệu từ HTML,  sai về câu  "kết nối mở không điều kiện".  
    
- Sai về mã lỗi HTTP: lỗi sập nguồn là từ phía máy chủ sẽ hiện 500 Internal Server Error, chứ không phải “do hồ sơ bệnh án của anh đã bị xóa vĩnh viễn khỏi hệ thống nên server mới trả về mã 404 này “.  Do bị sập nguồn mới hiện lỗi 404\.  
    
- Sai khi mở Chrome DevTools (F12) và chỉnh sửa trực tiếp trên tab Elements: nó chỉ làm thay đổi nội dung tạm thời trên máy tính cá nhân, hoàn toàn không lưu ngược lại vào dữ liệu trên máy chủ. Chỉ cần nhấn F5 là màn hình sẽ hiển thị lại nội dung gốc.

**2\. Giải thích vì sao khi máy chủ cơ sở dữ liệu bị sập (crash dịch vụ backend), mã HTTP trả về đúng phải là 500 chứ không phải 404? Hậu quả tai hại đối với tâm lý bệnh nhân nếu thông báo sai thành 404 là gì?**

**Vì sao phải mã HTTP trả về đúng phải là 500 chứ không phải 404:**

- Vì mã 404 Not Found dùng khi đường dẫn không tồn tại. Khi cơ sở dữ liệu bị sập là do về phía máy chủ, nên mã chuẩn phải là 500 Internal Server Error.

**Hậu quả tai hại đối với tâm lý bệnh nhân nếu thông báo sai thành 404 là gì?**

- Hậu quả tai hại: thông báo sai thành 404 khiến các bạn thực tập sinh hiểu nhầm là hồ sơ bệnh án đã bị xóa vĩnh viễn. Điều này sẽ gây hoang mang và làm mất niềm tin nghiêm trọng của bệnh nhân.   
  .

**PHẦN 2: SỬA LỖI & HOÀN THIỆN GIẢI PHÁP**

**3.Viết lại biên bản giải trình kỹ thuật chuẩn xác theo đúng 4 bước vòng đời của HTTP Request khi bệnh nhân truy cập kết quả xét nghiệm.**

- (B1) DNS Lookup: khi bệnh nhân bấm xem kết quả, trình duyệt gửi đến hệ thống DNS để chuyển đổi tên miền thành địa chỉ IP của Web Server.

- (B2) Kết nối TCP/TLS: sau khi có IP, trình duyệt thực hiện bắt tay TCP/TLS để làm đường truyền bảo mật HTTPS an toàn giữa điện thoại của bệnh nhân và Web Server.

- (B3) HTTP Request: trình duyệt gửi yêu cầu lấy nội dung từ Web Server. Do máy chủ gặp sự cố, hệ thống trả về mã 500 Internal Server Error. (Hồ sơ của bệnh nhân vẫn còn nguyên vẹn)

- (B4) Rendering: Khi nhận mã lỗi từ máy chủ, trình duyệt sẽ phân tích hiển thị lại giao diện lỗi ra màn hình.

**4\. Giải thích rõ ràng bằng nguyên lý bộ nhớ: Tại sao hành động sửa chữ trên Tab Elements của bạn thực tập sinh hoàn toàn không có giá trị lưu trữ trên máy chủ CarePulse?**

- Vì khi chỉnh sửa trên tab Elements, nó chỉ có thể can thiệp vào cây DOM đã được trình duyệt lưu tạm thời trên bộ nhớ RAM của thiết bị cá nhân. Chỉ cần bấm F5, toàn bộ dữ liệu tạm trên RAM sẽ bị xóa sạch và màn hình sẽ hiển thị lại nội dung gốc ban đầu.

