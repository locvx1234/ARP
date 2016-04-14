# ARP (Address Resolution Protocol)

## 1. ARP là gì ?
*ARP* (Address Resolution Protocol) là cơ chế tìm địa chỉ vật lý của máy khác trong mạng LAN dựa trên địa chỉ IP nhằm mục đích truyền thông giữa hai máy tính với nhau.

ARP được định nghĩa trong [RFC-826](https://tools.ietf.org/html/rfc826)

<img src="http://i.imgur.com/oaXBLYp.png">

## 2. Nguyên tắc hoạt động 

<img src="http://i.imgur.com/2nf63F0.png">

- Bước 1: Thiết bị A kiểm tra cache, nếu đã có địa chỉ MAC của 192.168.1.120 thì sang bước 9.
- Bước 2: Khởi tạo gói tin ARP resquest, gói tin mang IP và MAC của chính nó, IP đích, MAC đích là `ff:ff:ff:ff:ff:ff`
- Bước 3: Broadcast ra toàn mạng
- Bước 4: Các máy trong mạng khi nhận ARP resquest sẽ kiểm tra trường Target Protocol Address, nếu trùng thì tiếp tục xử lý.
- Bước 5: Thiết bị B trùng IP khởi tạo ARP repply : 
	+ Lấy Sender Hardware Address và Sender Protocol Address trong ARP resquest là Target trong ARP repply.
	+ Lấy MAC của mình đưa vào Sender Hardware Address
- Bước 6: Cập nhật bảng ánh xạ IP và MAC vào bảng ARP cache
- Bước 7: Thiết bị B gửi ARP repply cho thiết bị A.
- Bước 8: Thiết bị A nhận ARP repply và lưu Sender Hardware Address vào địa chỉ phần cứng của thiết bị Broadcast
- Bước 9: Thiết bị A cập nhật ARP cache. 
Như vậy là quá trình kết nối đã xong.
Khi A cần truyền thông với B, chỉ cần đưa MAC của B vào trường Target Hardware Address thì gói tin sẽ được chuyển đến B.

## 3. ARP table
Để hiện thị bảng ARP đã lưu, trong cửa sổ `cmd` ta gõ lệnh :

	> arp -a

<img src="http://i.imgur.com/cnKnPgi.png">

## 4. Định dạng gói tin ARP

<img src="http://i.imgur.com/dJzyeig.png">

| Tên trường | Mô tả |
|------------|-------|
| Hardware Type | Công nghệ phần cứng sử dụng (1: Ethernet, 15: Frame Relay, ...) |
| Protocol Type | Kiểu giao thức máy gửi cung cấp (IPv4 là 2048) |
| Hardware Address Length | Độ dài địa chỉ vật lý |
| Protocol Address Length | Độ dài địa chỉ logic  |
| Opcode | 1: ARP request, 2: ARP repply, 3: RARP request, 4: RARP repply |
| Sender Hardware Address | Địa chỉ MAC máy gửi |
| Sender Protocol Address | Địa chỉ IP máy gửi |
| Target Hardware Address | Địa chỉ MAC máy nhận |
| Target Protocol Address | Địa chỉ IP máy nhận |


## 5. Tham khảo

Book : Computer Networking A Top-Down Approach 6th-edition - Kurose Ross.

https://viblo.asia/pham.tri.thai/posts/mPjxMeZKG4YL

http://www.tcpipguide.com/free/t_ARPMessageFormat.htm
