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
  > - Mở trình duyệt:http://192.168.1.18:1880
  
<img width="808" height="392" alt="image" src="https://github.com/user-attachments/assets/c085ba39-5f59-4095-bd6b-471bc11def70" />

- Kiểm tra Grafana
Mở: http://192.168.1.18:3000

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/016442fd-0bdf-4cfa-9e91-9fbd13c7628c" />

- Kiểm tra InfluxDB
Mở: http://192.168.1.18:8086

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2ccd5835-b825-4a6c-831e-63bdf0453802" />

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

- Kiểm tra flask_api: http://192.168.1.18:5000/api/data

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/788c6d70-e806-48c4-ac35-7a7487dfb14d" />

- Tạo file app.py: nano app.py

<img width="961" height="1066" alt="image" src="https://github.com/user-attachments/assets/f8bfae1d-2e18-4c2c-9877-ea5a957841ab" />

- Tạo requirements.txt: nano requirements.txt

<img width="935" height="844" alt="image" src="https://github.com/user-attachments/assets/650072ff-e777-41ed-81cf-43a251b22e28" />

- Tạo Dockerfile: nano Dockerfile

- Kiểm tra API Chạy: curl localhost:5000/api/data

<img width="815" height="86" alt="image" src="https://github.com/user-attachments/assets/dbd676ba-37b3-4e21-a89b-ff93a7dc862a" />

## Node-RED tự động lấy dữ liệu thời tiết
 - Mục tiêu: Open-Meteo ↓ Node-RED ↓ MariaDB

- Mỗi 1 phút:
```
lấy nhiệt độ Hà Nội
ghi vào bảng realtime_data
Cài các node cần thiết cho bài
```
``
-Góc trên bên phải: ☰ -> Manage palette -> Install
- lần lượt cài:

- MySQL: node-red-node-mysql
- InfluxDB: node-red-contrib-influxdb
- Telegram: node-red-contrib-telegrambot
= sau khi cài xong khởi động lại: docker restart nodered_bt5

```
SƠ ĐỒ FLOW Bài Tập 5 (PHẦN NODE-RED) Inject ↓ HTTP Request ↓ Function GetTemp ├───────────→ Function SQL → MySQL │ ├───────────→ Function Influx → InfluxDB Out │ └───────────→ Switch ↓ Function Alert ↓ Telegram Sender
```
1. KÉO NODE INJECT -> Double click -> Repeat -> interval -> Every 60 -> Seconds seconds -> Done

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a4af9e95-82d1-4df3-9710-d8d85b6c323a" />

2. KÉO NODE HTTP REQUEST -> Nối: Inject với HTTP Request -> Double click -> Method: GET -> URL: https://api.open-meteo.com/v1/forecast?latitude=21.0285&longitude=105.8542&current=temperature_2m -> Return: a parsed JSON object -> Done

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/78ce6129-368c-49e0-9123-77e4ff3cf81a" />

3.KÉO FUNCTION GetTemp: Kéo: Function -> nối HTTP Request với Function -> Double click -> Đổi tên: GetTemp -> code -> done

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/42937065-37ce-48da-8bba-38cca57ca1c7" />

4. KÉO DEBUG -> nối GetTemp với Debug -> deploy -> Nhấn nút Inject -> Debug phải hiện nhiệt độ

<img width="1655" height="356" alt="image" src="https://github.com/user-attachments/assets/8b81394f-654a-4db0-93d6-92a9b875783e" />

5. KÉO FUNCTION SQL : Kéo: Function -> nối GetTemp với Function SQL -> Đổi tên: SQL Insert -> Code -> Done.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/418c2729-41ec-4ee8-be44-a51e6bddd1c1" />

6. KÉO MYSQL -> Nối: SQL Insert với MySQL -> cấu hình MySQL -> done

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f0bf0013-83ed-4c88-a7e4-061c59d6c219" />

7. TEST GHI DATABASE Deploy -> Nhấn Inject -> Kiểm tra MariaDB:
docker exec -it mariadb_bt5 mariadb -u root -p sau đó nhập password, rồi nhập : USE monitordb; SELECT * FROM realtime_data;

<img width="893" height="696" alt="image" src="https://github.com/user-attachments/assets/4ab1deca-a58d-4611-b387-64c084af0725" />

- Node-RED đã ghi được MariaDB. 

8. KÉO FUNCTION INFLUX Kéo:Function-> đổi tên thành Function Influx-> Nối GetTemp với Function Influx -> code -> done

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/573755b3-f674-4fbd-993d-46e43fc3e839" />

9. KÉO INFLUXDB OUT -> Nối Function Influx với InfluxDB Out -> Cấu hình InfluxDB Mở InfluxDB: Mở trình duyệt: http://192.168.1.18:8086 Kiểm tra đã setup InfluxDB chưa Nếu lần đầu mở sẽ có: Get Started phải tạo: Username,Password,Organization,Bucket

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c178615a-d9e0-4b10-bd18-2885349a22ab" />

<img width="925" height="746" alt="image" src="https://github.com/user-attachments/assets/0150eab1-5fb4-4721-a873-ddf17c28d93e" />

 Vào Node-RED trong cửa sổ cấu hình InfluxDB:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/587f3705-c302-4122-9350-7550a56be924" />

Sau khi bấm Add, node InfluxDB Out thường sẽ hiện thêm: điền:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d7017879-645b-4e6e-9a3e-b63237ac5963" />

Test -> Deploy -> Bấm Inject -> Xem node influxdb out có báo lỗi đỏ không nếu không báo lỗi đỏ là đã ok

10. KÉO SWITCH -> Nối: GetTemp với Switch -> cấu hình

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5092d3bf-3188-475f-b0f5-bc9b9e6fe78d" />

11. KÉO FUNCTION ALERT : kéo Function -> đổi tên thành Function Alert -> Nối cả 2 output của Switch vào Function này-> code -> done

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/54cdcb0d-0adc-4f8e-bd6a-be8b2e90d350" />

12. KÉO TELEGRAM SENDER : Kéo: telegram sender Nối: Function Alert với Telegram Sender -> Cấu hình -> done -> deploy
( để lấy id nhóm chat ta truy cập vào telegram -> tìm @userinfobot -> nhấn start -> ấn group/ hoặc my group sau đó tìm nhóm chat cần lấy id )

<img width="1920" height="1080" alt="Ảnh chụp màn hình 2026-06-09 225025" src="https://github.com/user-attachments/assets/f980f909-a1e4-4c43-826e-90cbbdb38241" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ea07afa-4279-4b95-81bd-730c8b5e97a5" />

- khi đó bot sẽ gửi tin nhắn cảnh báo vì tầm này thời tiết không trong thời gian cảnh báo lên em test xem bot hoạt động

<img width="1125" height="2436" alt="image" src="https://github.com/user-attachments/assets/eb29dff3-3ef5-40a2-a80f-6b956f9e9d2b" />

## KẾT QUẢ SAU KHI LÀM XONG PHẦN NODE-RED
- Ta sẽ có:
✓ Open-Meteo lấy dữ liệu thật
✓ Node-RED lấy nhiệt độ mỗi 60 giây
✓ Ghi MariaDB
✓ Chuẩn bị ghi InfluxDB
✓ Kiểm tra ngưỡng
✓ Gửi Telegram khi vượt ngưỡng

- Grafana — vẽ biểu đồ từ InfluxDB
+ Vào trình duyệt: http://192.168.1.18:3000
+ Thêm Data Source InfluxDB
+ Menu trái → Connections → Data sources
+ Nhấn Add data source
+ Chọn InfluxDB

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/08b81e95-8dd2-4891-9aea-1501806d621d" />

- Data source đã kết nối thành công! Bây giờ tạo Dashboard: Menu trái → Dashboards → New → New dashboard → Nhấn Add visualization → Chọn data source InfluxDB vừa tạo

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f40160c0-afb8-4a01-916c-9fb0ca7e26e0" />

- Tạo thư mục frontend
 +  tạo mkdir web
 +  Tạo file web
       + nano ~/bt5-monitor/web/index.html

<img width="976" height="1022" alt="image" src="https://github.com/user-attachments/assets/d49db80a-eb25-4667-94db-63a220102c6a" />

Khởi động lại cd ~/bt5-monitor docker compose up -d
sau đó kiểm tra xem đã chạy thành công chưa

<img width="1919" height="279" alt="image" src="https://github.com/user-attachments/assets/f318ee2b-3888-4309-959b-aa00e2fe7ece" />


<img width="445" height="358" alt="image" src="https://github.com/user-attachments/assets/2c41c2a1-9f15-4195-be52-34be8b6131c0" />

- Nhúng Grafana
 + Mở file index.html: nano ~/bt5-monitor/nginx/html/index.html -> thêm iframe Grafana

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ea07afa-4279-4b95-81bd-730c8b5e97a5" />

kiểm tra xem đã hiện biểu đồ chưa: http://192.168.1.18:8088/

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0a6fe31e-b21c-4191-9b96-658cc2dfd01d" />











