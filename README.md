# Bai5_phat_trien_ung_dung_tren_ma_nguon_mo
BÀI TẬP LỚN
môn: phát triển ứng dụng với mã nguồn mở - tee0421
bt5:
  - lý thuyết: 
    + docker là gì? 
    + các keyword được sử dụng trong docker-compose.yml
      để mô tả 1 service, network, volume,...
      liệt kê + ý nghĩa của từ khoá đó + ví dụ minh hoạ
    + ưu điểm khi triển app sử dụng docker là gì?
    + dùng docker: tạo app, test app OK trên laptop cá nhân
      giờ muốn triển khai app này trên máy chủ thật ko có internet
      thì các bước cần làm là?
  - thực hành áp dụng: APP MONITOR + ALERT DATA REALTIME
    sử dụng docker compose có nhiều serivce 
    và các thành phần cần thiết để tạo thành ứng dụng:
     + nodered liên tục lấy dữ liệu từ nguồn nào đó (chứng khoán, thời tiết, giá vàng,...)
       nguồn thực tế, số liệu luôn động sau thời gian ngắn
     + nodered lưu trữ dữ liệu vào 2 database: mariadb để lưu giá trị tức thời
       lưu lịch sử vào influxdb
     + sử dụng grafana để trực quan hoá dữ liệu: vẽ biểu đồ
     + sử dụng nginx để làm webserver
       chạy 1 trang web html+js+css làm front-end
       js: lấy dữ liệu tức thời trong mariadb qua (ajax | socket) 
           gọi api (api tự build bằng Flask giống bt1)
           api trả về giá trị tức thời trong mariadb
           hiển thị lên web, auto hiển thị số mới khi thay đổi
       sử dụng iframe để gọi grafana
       hiển thị biểu đồ dữ liệu lịch sử của thông số đã lưu
     + QUAN SÁT DỮ LIỆU LỊCH SỬ => GIÁ TRỊ BẤT THƯỜNG
       (VD MIỀN A..B: OK, DƯỚI A: ALERT LOW, TRÊN B: ALERT HIGH)
     + nodered: kết hợp bot Telegram
       khi dữ liệu not OK, thì gửi tin nhắn từ bot => group trên telegram
       group đã add bot vào: (nhóm đã có 2 người), add thêm 1875746636 thành 3 người
       mỗi khi bot gửi dữ liệu vào nhóm: mọi member of group đều nhận đc
       nội dung alert: tường minh, có value gây alert

     xuất tất cả các container ra file nén.
     xoá mọi container đang chạy
     load lại các container  từ file nén để khôi phục các container đã xoá
=========
quá trình làm: chụp ảnh lại, mô tả cho ảnh
  lưu vào trong github => paste link access public của repo: vào file excel online
=====================

cả 5 bài tập:
biên tập lại xíu để phù hợp với bản print
đóng quyển, ko cần bìa xanh, ko cần giấy bóng kính
header+footer của các trang giấy: có tên + masv, bài tập lớp Môn, số trang
trang 1 có đầy đủ thông tin cá nhân.

lưu ở bm để chấm điểm.

## BÀI LÀM
# Lý thuyết
# Thực hành
- Tạo thư mục BT5
Chạy:
mkdir ~/bt5-monitor
cd ~/bt5-monitor
Kiểm tra xem đã thành công hay chưa : pwd
<img width="694" height="94" alt="image" src="https://github.com/user-attachments/assets/e9c0287a-49c2-432a-ad98-63dcfc3a5c5d" />
- Tạo file docker-compose.yml
Chạy: nano docker-compose.yml
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/04bba456-a8ae-4d1c-a87c-4dd8aad5deb5" />
- Khởi động các container
Chạy: docker compose up -d
<img width="1083" height="551" alt="image" src="https://github.com/user-attachments/assets/689ad89a-f715-4a20-bd2c-224f640546c3" />

- Kiểm tra container
Chạy: docker ps
<img width="1841" height="243" alt="image" src="https://github.com/user-attachments/assets/62e6ad2b-50ba-434a-9013-b0a90fa61b34" />

- Kiểm tra Node-RED
  > - Mở trình duyệt:http://localhost:1880
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/33a338db-1be9-42ac-9b4e-7dac1e9141a9" />
- Kiểm tra Grafana
Mở: http://localhost:3000
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1bc0b261-089d-452e-8b15-3b45a120507f" />

- Kiểm tra InfluxDB
Mở: http://localhost:8086
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/455d91ea-d50e-4e04-9b2a-0cd95b1e2ee1" />

- Kiểm tra MariaDB
docker exec -it mariadb_bt5 mariadb -u root -p
- <img width="1083" height="226" alt="image" src="https://github.com/user-attachments/assets/f16a9cfe-da80-4cc3-a226-3ce20ff004e2" />

- Kiểm tra các database: SHOW DATABASES;
<img width="396" height="269" alt="image" src="https://github.com/user-attachments/assets/ff451dd7-0325-4f32-a3e1-ce65c20e3181" />

- Chọn database: USE monitordb;
- Tạo bảng realtime_data
<img width="506" height="204" alt="image" src="https://github.com/user-attachments/assets/cb23201d-e375-4788-9464-f8700eb4d62d" />
- Kiểm tra bảng đã tạo: SHOW TABLES;
<img width="381" height="172" alt="image" src="https://github.com/user-attachments/assets/6b1c0a47-adbf-467a-aa16-4800a0cb56d0" />

- Chèn thử dữ liệu mẫu
<img width="681" height="94" alt="image" src="https://github.com/user-attachments/assets/5ca172ee-b88d-4c63-aec2-bf139cdec2b9" />

- Kiểm tra dữ liệu: SELECT * FROM realtime_data;
<img width="525" height="534" alt="image" src="https://github.com/user-attachments/assets/9bbde619-c696-436a-a254-b751582cf748" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ea07afa-4279-4b95-81bd-730c8b5e97a5" />








