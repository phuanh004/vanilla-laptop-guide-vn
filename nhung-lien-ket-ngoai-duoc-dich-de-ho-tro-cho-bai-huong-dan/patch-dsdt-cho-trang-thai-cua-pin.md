# Patch DSDT cho trạng thái của pin

## Hướng dẫn cách patch DSDT để trạng thái pin hoạt động

\[RehabMan\] Hướng dẫn cách patch DSDT để trạng thái pin hoạt động - Trích từ bài gốc [\[Guide\] How to patch DSDT for working battery status](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/) và đã được chỉnh sửa để phù hợp với hiện tại.

### **Lời giới thiệu**

Do phần cứng pin trên PC không tương thích với Apple SMbus, chúng ta sử dụng ACPI để truy cập trạng thái pin khi chạy OS X trên máy tính xách tay. Thông thường, tôi khuyên bạn sử dụng [ACPIBatteryManager.kext](https://github.com/RehabMan/OS-X-ACPI-Battery-Driver).  
  
Các bản phát hành sau này của AppleACPIPlatform không thể truy cập chính xác các trường trong EC \(embedded controller hay bộ điều khiển nhúng\). Điều này gây ra sự cố cho ACPIBatteryManager as the various ACPI methods for battery fail \(\_BIF, \_STA, \_BST, etc\). Mặc dù có thể sử dụng phiên bản cũ hơn của AppleACPIPlatform \(từ Snow Leopard\), nhưng bạn nên sử dụng phiên bản mới nhất của AppleACPIPlatform, với các máy tính sử dụng Ivy Bridge CPUs, bạn chỉ cần sử dụng kext này là tự nhận bộ quản lý điện năng. Để sử dụng được phiên bản mới nhất, DSDT phải được chỉnh sửa lại để tuân thủ các giới hạn mà AppleACPIPlatform của Apple đưa ra.  
  
Cụ thể, bất cứ trường nào trong EC lớn hơn 8 bit, phải được thay đổi để được truy cập 8 bit cùng một lúc. Điều này bao gồm 16, 32, 64 và các trường lớn hơn.

Bạn nên làm quen với các nguyên tắc patch DSDT / SSDT cơ bản tại: [http://www.tonymacx86.com/yosemite-laptop-support/152573-guide-patching-laptop-dsdt-ssdts.html](http://www.tonymacx86.com/yosemite-laptop-support/152573-guide-patching-laptop-dsdt-ssdts.html)

### **Các bản patch hiện có sẵn**

Đầu tiên, có thể là các bản patch cho máy tính xách tay của bạn đã có sẵn. Bạn hãy kiểm tra tại đây: [https://github.com/RehabMan/Laptop-DSDT-Patch](https://github.com/RehabMan/Laptop-DSDT-Patch)

Để kết hợp DSDT với một bản patch, điều kiện tiên quyết là bạn phải hiểu các bản patch được tạo ra như thế nào, để bạn hiểu được là mình cần tìm gì trong DSDT và kết hợp với những bản patch đã có sẵn. Một bộ patch có khả năng cao là thành công khi được tạo ra mà không có lỗi, và vá các trường dữ liệu có khả năng và các trường cần được patch có điểm tương đồng có thể kết hợp lại với nhau được.

Xem thêm tại: [https://www.tonymacx86.com/threads/...g-battery-status.116102/page-333\#post-1360697](https://www.tonymacx86.com/threads/...g-battery-status.116102/page-333#post-1360697)

{% hint style="danger" %}
Không sử dụng DSDT Editor hoặc bất kỳ chương trình nào khác ngoài **MaciASL**. Bởi @Rehabman không kiểm tra các bản vá của mình với DSDT Editor, chỉ được kiểm tra với **MaciASL**.
{% endhint %}

### **Các bản patch khác có liên quan**

Ngoài các trường EC nhiều byte \(multi-byte\), có nhiều vấn đề khác DSDT có thể ảnh hưởng đến trạng thái pin. Những vấn đề này không cụ thể cho trạng thái của pin, nhưng chúng thường được thông báo trong lần đầu tiên khi bạn implement trạng thái pin.

Mã pin có thể phụ thuộc vào việc có phiên bản Windows được nhận là hệ điều hành trên máy chủ \(host OS\). Để khắc phục, hãy sử dụng "OS Check Fix" từ kho lưu trữ DSDT patch của laptop. Điều này sẽ làm cho DSDT thực hiện các hành động tương tự như khi chạy "Windows 2006." Lựa chọn những bản patch khác nhau sẽ có những ảnh hưởng khác nhau \(ví dụ: "Windows 2012"\).

Một vấn đề phổ biến khác sự kế thừa của ACPI trên OS X gặp khó khăn với các đối tượng Mutex được khai báo với SyncLevel khác 0 \(non-zero\) \(để biết thêm thông tin hãy đọc thông số ACPI\). Để khắc phục, hãy sử dụng patch "Fix Mutex with non-zero SyncLevel" từ kho lưu trữ DSDT patch của laptop.

### Các kỹ năng cần có

**DSDT** là một "chương trình máy tính." Như vậy, sẽ hữu ích khi có một số **kỹ năng lập trình** và **kỹ năng về máy tính** khi muốn sửa đổi nó. Các bản patch của DSDT cũng có ngôn ngữ riêng của chúng \(được mô tả vắn tắt trong [Wiki của MaciASL](http://sourceforge.net/projects/maciasl/)\). Cuối cùng, bản thân các bản patch về cơ bản về cơ bản là **tìm kiếm và thay thế biểu thức chính quy** \(regex\). Một số những điều cần thiết khác là **làm quen với trình biên dịch**, **lỗi biên dịch** \(compiler error\), và có khả năng **xác định lỗi** nằm ở đâu thông qua lỗi biên dịch.

Thêm nữa, làm quen với ACPI cũng là một ý không tồi. Bạn có thể tải xuống bản chi tiết kỹ thuật tại đây: [https://www.acpica.org/](https://www.acpica.org/)

Mục đích của hướng dẫn này không phải là để dạy cho bạn các kỹ năng lập trình cơ bản, các biểu thức chính quy hoặc ngôn ngữ ACPI.

### Quy trình patch DSDT

I use a rather 'mechanical' process to patching DSDT for battery status. I simply look for the parts that OS X finds offensive and mechanically convert it. Tôi không cố gắng quá nhiều để xác định phần nào của mã thực sự sẽ thực thi, tôi chỉ chuyển mã mọi thứ mà tôi thấy.

Để làm theo, hãy tải xuống ví dụ DSDT từ bài đăng này và làm theo. Ví dụ cụ thể DSDT này dành cho laptop HP Envy 14. Bản patch cuối cùng và hoàn chỉnh được tôi hiện đang được lưu trữ tại kho lưu trữ của tôi với tên gọi "HP Envy 14."

Trước tiên hãy bắt đầu bằng cách xác định các khu vực của DSDT có khả năng cần được thay đổi. Mở file DSDT bằng MaciASL và tìm kiếm từ khoá 'EmbeddedControl'. Có thể có nhiều 'EmbeddedControl' được tìm thấy trong một DSDT, mỗi EC có những field declarations đi kèm.

Vì vậy, tôi luôn bắt đầu tìm kiếm 'embeddedcontrol' để tìm declaration này.

Trong ví dụ DSDT, bạn sẽ tìm thấy vùng EC duy nhất này:

```text
OperationRegion (ECF2, EmbeddedControl, Zero, 0xFF)
```

Đoạn mã ở trên chỉ ra rằng đây là EC 255 byte.

Ta biết nó được gọi là ECF2, do đó việc tiếp theo chúng ta cần làm là tìm từ khoá 'Field \(ECF2'. Như có thể thấy ở đây trong DSDT được mang ra làm ví dụ, chỉ có duy nhất một trường được định nghĩa. Đôi lúc có có thể tìm thấy nhiều hơn một.

The Field definition describes a breakdown of that 255 byte EC region above. You can tell it is related because the name ECF2 is referred to by the Field. Hãy xem điều này như là một structure \(hay còn gọi là struct trong C\) into the EC.

Bước tiếp theo là kiểm tra các items trong Field definition, tìm các mục lớn hơn 8-bit. Ví dụ, trường đầu tiên được khai báo là BDN0, 56:

```text
Field (ECF2, ByteAcc, Lock, Preserve)
   {
       Offset (0x10),
       BDN0,   56,
...
```

Đây là một trường 56-bit. Lớn hơn 8-bit và nếu đoạn mã trên được truy cập vào, trong DSDT, đoạn mã đó sẽ cần chỉnh sửa theo definition của trường này. Trong file ví dụ, nếu bạn tìm kiếm phần còn lại của DSDT với từ khoá "BDN0", bạn sẽ tìm thấy:

```text
Store (BDN0, BDN)
```

Đoạn mã này có chức năng lưu trữ lại giá trị tại BDN0 \(ở trong EC\) vào BDN. Khi các trường **lớn hơn 32-bit** được truy cập, chúng sẽ được truy cập dưới dạng **Buffer** \(Dữ liệu tạm thời\). Các trường **32-bit hoặc nhỏ hơn** sẽ được truy cập dưới dạng **Integer** \(Số nguyên\). Điều này rất quan trọng khi bạn thay đổi mã nguồn. Việc thay đổi các dữ liệu ở dạng Buffer có phần khó hơn so với Integer. Ta phải nhận biết được đoạn mã là đọc từ EC hay ghi vào EC, bởi vì ta sẽ có cách xử lý khác nhau cho mỗi trường hợp. Đoạn mã phía trên được sử dụng để đọc từ EC.

Quan sát phần còn lại của EC, ta tìm tất cả các trường khác hớn hơn 8-bit, với mới trường, tìm kiếm phần còn lại của DSDT và xem chúng có được truy cập không. Chuyện thường thấy là có những trường không được truy cập, và đối với những trường đó, ta không cần phải làm gì cả. Trường tiếp theo mà ta thấy là BMN0:

```text
BMN0,   32,
```



