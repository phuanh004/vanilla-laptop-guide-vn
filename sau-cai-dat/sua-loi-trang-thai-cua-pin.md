# Sửa lỗi trạng thái của pin



## 1. Battery Patching

Hiện tại, việc vá pin không được hỗ trợ trong hướng dẫn này. Tuy nhiên, chúng tôi vẫn sẽ nêu ra tài liệu hữu ích và kèm theo một số lưu ý hữu ích khi sử dụng chúng trong OpenCore.

### 1.1. DSDT Patching \(Static Patch\)

Mặc dù việc sử dụng bản tuỳ chỉnh của DSDT là điều không nên để tránh các sự cố với Windows and các bản cập nhập phần mềm máy tính, nhưng nó có thể là một điểm xuất phát khá hữu ích vì việc nắm bắt và tự làm có chút dễ dàng hơn với bạn:

[**Patch DSDT cho trạng thái của pin theo phương pháp của @Rehabman\)**](/patch-dsdt-cho-trang-thai-cua-pin)

{% hint style="info" %}
**DSDT phải được đặt đầu tiên trong config.plist \(ACPI -&gt; Add\). Cũng nên nhớ rằng DSDT cũng phải được đặt trong thư mục EFI/OC/ACPI.**
{% endhint %}

{% hint style="info" %}
**Tránh sử dụng MaciASL và iASL của Rehabman, bởi từ lâu, chúng đã bị lãng quên và không được cập nhật. Vì thế một phiên bản khác được khuyên dùng là** [**MaciASL**](https://github.com/acidanthera/MaciASL/releases) **từ Acidanthera.**
{% endhint %}

### 1.2. Patch "nóng" pin \(Hot-patching\) <a id="battery-hot-patching"></a>

Khi mà bạn đã có được bản DSDT đã được patch và trạng thái pin lúc này đã hoạt động trên macOS, đó là lúc bạn tạo ra những bản patch "nóng" cho mình. Patch "nóng" khác với patch thường như thế nào? is that it's done on the fly with the DSDT allowing for greater flexibility with firmware updates:

[**Rehabman's Guide to Using Clover to "hotpatch" ACPI**](https://www.tonymacx86.com/threads/guide-using-clover-to-hotpatch-acpi.200137/)

{% hint style="info" %}
Phương pháp patch "nóng" được đề cập ở \#2 của bài viết.
{% endhint %}



