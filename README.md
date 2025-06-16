# Dự án Nhận dạng Chữ cái Tiếng Việt bằng YOLOv11
![GitHub stars](https://img.shields.io/github/stars/[ten-github-cua-ban]/[ten-repo-cua-ban]?style=social)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

Dự án mã nguồn mở sử dụng mô hình **YOLOv11** để nhận dạng các chữ cái trong bảng chữ cái Tiếng Việt trong thời gian thực từ hình ảnh, video hoặc webcam.

## Giới thiệu

Mục tiêu của dự án là xây dựng một mô hình có khả năng phát hiện và xác định chính xác vị trí của từng chữ cái Tiếng Việt (bao gồm cả các ký tự có dấu như ă, â, đ, ê, ô, ơ, ư). Mô hình này có thể được ứng dụng trong nhiều lĩnh vực như hỗ trợ giáo dục, số hóa tài liệu, hoặc các hệ thống tự động hóa.

## Demo

![Demo GIF]([link_den_file_gif_demo_cua_ban.gif])
*Ví dụ về khả năng nhận dạng của mô hình trên một hình ảnh mẫu.*

## Tính năng nổi bật

- **Độ chính xác cao**: Mô hình được huấn luyện trên một tập dữ liệu lớn và đa dạng để đạt được độ chính xác cao.
- **Thời gian thực**: Tối ưu hóa để có thể chạy trên webcam với tốc độ khung hình (FPS) cao.
- **Hỗ trợ đầy đủ ký tự**: Nhận dạng toàn bộ 29 chữ cái trong bảng chữ cái Tiếng Việt và các ký tự có dấu.
- **Dễ dàng mở rộng**: Có thể dễ dàng huấn luyện lại (retrain) với tập dữ liệu của riêng bạn để cải thiện hiệu suất hoặc nhận dạng thêm các ký tự khác.

## Công nghệ sử dụng

- **Model**: YOLOv11 (You Only Look Once version 11)
- **Framework**: PyTorch
- **Thư viện**:
  - `ultralytics`: Framework chính để huấn luyện và sử dụng các mô hình YOLO.
  - `OpenCV (cv2)`: Để xử lý hình ảnh và video.
  - `NumPy`: Để xử lý mảng dữ liệu.
  - `Roboflow` (tùy chọn): Hỗ trợ quản lý và tiền xử lý dữ liệu.

## Cài đặt

### Yêu cầu
- Python 3.8+
- PyTorch 1.7+
- Git

### Hướng dẫn cài đặt

1.  **Clone repository về máy:**
    ```bash
    git clone [https://github.com/](https://github.com/)[ten-github-cua-ban]/[ten-repo-cua-ban].git
    cd [ten-repo-cua-ban]
    ```

2.  **Tạo và kích hoạt môi trường ảo (khuyến khích):**
    ```bash
    # Trên Windows
    python -m venv venv
    .\venv\Scripts\activate

    # Trên macOS/Linux
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Cài đặt các thư viện cần thiết:**
    ```bash
    pip install -r requirements.txt
    ```
    *Ghi chú: Nội dung file `requirements.txt` nên bao gồm các gói như `ultralytics`, `opencv-python`, `numpy`.*

4.  **Tải trọng số (weights) đã huấn luyện:**
    Tải file trọng số `best.pt` từ [Link Google Drive/GitHub Releases/... của bạn] và đặt vào thư mục gốc của dự án hoặc thư mục `weights/`.

## Tập dữ liệu (Dataset)

Mô hình được huấn luyện trên tập dữ liệu tùy chỉnh gồm các hình ảnh chứa chữ cái Tiếng Việt.

- **Nguồn**: [Ví dụ: Tự thu thập, Roboflow, Kaggle,...]
- **Số lượng lớp (Classes)**: 29+ (tùy thuộc vào việc bạn có gộp chữ hoa/thường và các dấu không).
- **Định dạng**: Dữ liệu được gán nhãn theo định dạng YOLO (`.txt`).
- **Cấu trúc file `data.yaml`:**
    ```yaml
    train: ../train/images
    val: ../valid/images
    test: ../test/images

    nc: 29 # Số lượng lớp
    names: ['a', 'ă', 'â', 'b', 'c', 'd', 'đ', 'e', 'ê', 'g', 'h', 'i', 'k', 'l', 'm', 'n', 'o', 'ô', 'ơ', 'p', 'q', 'r', 's', 't', 'u', 'ư', 'v', 'x', 'y']
    ```

Bạn có thể truy cập và tải tập dữ liệu tại đây: [Link tới dataset của bạn]

## Huấn luyện (Training)

Để huấn luyện lại mô hình trên tập dữ liệu của bạn:

1.  **Chuẩn bị dữ liệu**: Sắp xếp dữ liệu theo cấu trúc yêu cầu và tạo file `.yaml` như trên.
2.  **Chạy lệnh huấn luyện**:
    ```bash
    # Bắt đầu huấn luyện từ đầu với model yolov11n.pt (nano version)
    yolo train model=yolov11n.pt data=data/dataset.yaml epochs=100 imgsz=640

    # Hoặc tiếp tục huấn luyện từ một checkpoint
    yolo train model=path/to/last.pt data=data/dataset.yaml epochs=50 imgsz=640
    ```
-   `model`: File model `.pt` để bắt đầu (có thể là model gốc của YOLO hoặc checkpoint của bạn).
-   `data`: Đường dẫn tới file `.yaml` mô tả dataset.
-   `epochs`: Số chu kỳ huấn luyện.
-   `imgsz`: Kích thước ảnh đầu vào.

Kết quả huấn luyện (bao gồm trọng số `best.pt` và `last.pt`) sẽ được lưu trong thư mục `runs/detect/train/`.

## Sử dụng (Inference)

Sử dụng trọng số tốt nhất (`best.pt`) từ quá trình huấn luyện để nhận dạng.

-   **Nhận dạng trên ảnh:**
    ```bash
    yolo predict model=weights/best.pt source=path/to/your/image.jpg
    ```

-   **Nhận dạng trên video:**
    ```bash
    yolo predict model=weights/best.pt source=path/to/your/video.mp4
    ```

-   **Nhận dạng qua Webcam:**
    ```bash
    yolo predict model=weights/best.pt source=0
    ```

Kết quả sẽ được tự động lưu vào thư mục `runs/detect/predict/`.

## Kết quả

Mô hình đạt được các kết quả sau trên tập kiểm thử (validation set):

- **mAP50-95**: `[Điền giá trị mAP50-95 của bạn]`
- **mAP50**: `[Điền giá trị mAP50 của bạn]`

![Confusion Matrix]([link_toi_anh_confusion_matrix.png])
*Ma trận nhầm lẫn trên tập validation.*

![Results]([link_toi_anh_results.png])
*Biểu đồ các chỉ số trong quá trình huấn luyện.*

## Hướng phát triển trong tương lai

- [ ] Cải thiện độ chính xác bằng cách tăng cường và đa dạng hóa tập dữ liệu.
- [ ] Tối ưu hóa mô hình với TensorRT để tăng tốc độ inference trên các thiết bị NVIDIA Jetson.
- [ ] Xây dựng một giao diện đồ họa (GUI) hoặc ứng dụng web đơn giản để người dùng dễ dàng tương tác.
- [ ] Mở rộng thành mô hình nhận dạng ký tự quang học (OCR) để đọc toàn bộ từ và câu.

## Đóng góp

Mọi sự đóng góp đều được chào đón! Vui lòng tạo `Fork` cho dự án này, tạo một nhánh mới cho tính năng của bạn (`git checkout -b feature/AmazingFeature`), commit các thay đổi (`git commit -m 'Add some AmazingFeature'`) và tạo một `Pull Request`.

## Giấy phép

Dự án này được cấp phép theo Giấy phép MIT. Xem file `LICENSE` để biết thêm chi tiết.

## Liên hệ & Lời cảm ơn

- **Tác giả**: [Tên của bạn] - [email@cua.ban]
- **Project Link**: [https://github.com/[ten-github-cua-ban]/[ten-repo-cua-ban]](https://github.com/[ten-github-cua-ban]/[ten-repo-cua-ban])

- **Lời cảm ơn**:
  - Cảm ơn [Ultralytics](https://ultralytics.com/) đã phát triển và duy trì một framework YOLO tuyệt vời.
  - Cảm ơn [Nguồn dữ liệu của bạn] đã cung cấp dữ liệu cho dự án.
