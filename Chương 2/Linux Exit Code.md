`echo $?`: exit code

$? là biến đặc biệt của shell.

Lưu Exit Code của lệnh thực thi gần nhất.

0 → lệnh thực thi thành công.

1 → lỗi chung (general error).

2 → Lỗi do cách dùng lệnh, tham số, hoặc lỗi đặc thù của chương trình

126 →	Có file nhưng không có quyền thực thi

127	→ Command not found

130	→ Bị dừng bằng Ctrl + C

255 →	Lỗi ngoài phạm vi hoặc lỗi nghiêm trọng

Security tool không chỉ in kết quả ra màn hình, mà còn dùng Exit Code để báo cho script, CI/CD biết có lỗ hổng hay không.

Khi dùng pipe (|), echo $? chỉ trả về Exit Code của lệnh cuối cùng trong pipeline.

commandA | commandB | commandC

`echo $?` trả về Exit Code của commandC chứ không phải của commandA, commandA hay cả pipeline.
