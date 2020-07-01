# Patch DSDT cho trạng thái của pin

\[RehabMan\] Hướng dẫn cách patch DSDT để trạng thái pin hoạt động - Trích từ bài gốc [\[Guide\] How to patch DSDT for working battery status](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/) và đã được chỉnh sửa để phù hợp với hiện tại.

### **Bối cảnh**

Do phần cứng pin trên PC không tương thích với Apple SMbus, chúng ta sử dụng ACPI để truy cập trạng thái pin khi chạy OS X trên máy tính xách tay. Thông thường, tôi khuyên bạn sử dụng [ACPIBatteryManager.kext](https://github.com/RehabMan/OS-X-ACPI-Battery-Driver).  
  
Các bản phát hành sau này của AppleACPIPlatform không thể truy cập chính xác các trường trong EC \(embedded controller hay bộ điều khiển nhúng\). Điều này gây ra sự cố cho ACPIBatteryManager as the various ACPI methods for battery fail \(\_BIF, \_STA, \_BST, etc\). Mặc dù có thể sử dụng phiên bản cũ hơn của AppleACPIPlatform \(từ Snow Leopard\), nhưng bạn nên sử dụng phiên bản mới nhất của AppleACPIPlatform, với các máy tính sử dụng Ivy Bridge CPUs, bạn chỉ cần sử dụng kext này là tự nhận bộ quản lý điện năng. Để sử dụng được phiên bản mới nhất, DSDT phải được chỉnh sửa lại để tuân thủ các giới hạn mà AppleACPIPlatform của Apple đưa ra.  
  
Cụ thể, bất cứ trường nào trong EC lớn hơn 8 bit, phải được thay đổi để được truy cập 8 bit cùng một lúc. Điều này bao gồm 16, 32, 64 và các trường lớn hơn.

Bạn nên làm quen với các nguyên tắc patch DSDT / SSDT cơ bản tại: [http://www.tonymacx86.com/yosemite-laptop-support/152573-guide-patching-laptop-dsdt-ssdts.html](http://www.tonymacx86.com/yosemite-laptop-support/152573-guide-patching-laptop-dsdt-ssdts.html)

### **Các bản patch hiện có sẵn**

Đầu tiên, có thể là các bản patch cho máy tính xách tay của bạn đã có sẵn. Bạn hãy kiểm tra tại đây: [https://github.com/RehabMan/Laptop-DSDT-Patch](https://github.com/RehabMan/Laptop-DSDT-Patch)

Để kết hợp DSDT với một bản patch, điều kiện tiên quyết là bạn phải hiểu các bản patch được tạo ra như thế nào, để bạn hiểu được là mình cần tìm gì trong DSDT và kết hợp với những bản patch đã có sẵn. Một bộ patch có khả năng cao là thành công khi được tạo ra mà không có lỗi, và vá các trường dữ liệu có khả năng và các trường cần được patch có điểm tương đồng có thể kết hợp lại với nhau được.

More info here: [https://www.tonymacx86.com/threads/...g-battery-status.116102/page-333\#post-1360697](https://www.tonymacx86.com/threads/...g-battery-status.116102/page-333#post-1360697)

{% hint style="danger" %}
Không sử dụng DSDT Editor hoặc bất kỳ chương trình nào khác ngoài **MaciASL**. Bởi @Rehabman không kiểm tra các bản vá của mình với DSDT Editor, chỉ được kiểm tra với **MaciASL**.
{% endhint %}

### **Các bản patch khác có liên quan**

Ngoài các trường EC nhiều byte \(multi-byte\), có nhiều vấn đề khác DSDT có thể ảnh hưởng đến trạng thái pin. Những vấn đề này không cụ thể cho trạng thái của pin, nhưng chúng thường được thông báo trong lần đầu tiên khi bạn implement trạng thái pin.

Mã pin có thể phụ thuộc vào việc có phiên bản Windows được công nhận là may depend on having a recognized version of Windows as the host OS. Để khắc phục, hãy sử dụng "OS Check Fix" từ kho lưu trữ DSDT patch của laptop. Điều này sẽ làm cho DSDT thực hiện các hành động tương tự như khi chạy "Windows 2006." Lựa chọn những bản patch khác nhau sẽ có những ảnh hưởng khác nhau \(ví dụ: "Windows 2012"\).

Một vấn đề phổ biến khác sự kế thừa của ACPI trên OS X gặp khó khăn với các đối tượng Mutex được khai báo với SyncLevel khác 0 \(non-zero\) \(để biết thêm thông tin hãy đọc thông số ACPI\). Để khắc phục, hãy sử dụng patch "Fix Mutex with non-zero SyncLevel" từ kho lưu trữ DSDT patch của laptop.

