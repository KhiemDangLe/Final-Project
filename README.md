> Mục lục
> [Overview](# Overview)
> [Thành viên nhóm] (# Thành viên nhóm)
# Overview
* Chủ đề : Dự đoán giá nhà
* Nguồn dữ liệu: https://batdongsan.vn/
### Cách tổ chức các file
* **DataFolder**: chứa dữ liệu thu thập được và dữ liệu phục vụ cho quá trình vẽ heatmap
* **Image**: chứa hình ảnh minh họa. Các hình ảnh mình họa được chèn vào trong các file notebook
* Các file report: là các file Jupyter Notebook chứa code và báo cáo của từng phần trong quá trình thực hiện đồ án. Các file được đánh số theo thứ tự thực hiện. Các file thuộc cùng một quá trình sẽ được đánh dấu thêm bằng chỉ số phụ. Ví dụ: file 5_1_report_gradient_boosting là file báo cáo quá trình thực hiện mô hình Gradient Boosting. 
* Các file khác, bao gồm:
  * **2_1_prompt_for_extracting.txt**: chứa prompt sử dụng để truy vấn mô hình LLM
  * **6_1_result_heat_map_geospatial.html, 6_2_result_heat_map_house_address**: chứa heatmap thể hiện sự phân bố của nhà ở theo địa chỉ của ngôi nhà.
### Thành viên nhóm
* Vũ Đăng Khôi - 
* Trảo An Huy - 
* Đặng Lê Khiêm - 22280045
* Thái Anh Khoa - 
### Đóng góp của các thành viên
# Tóm tắt quá trình thực hiện
## Crawl dữ liệu
### Sơ lược về trang web
<div style="text-align: center;">
    <img src="https://github.com/KhiemDangLe/Final-Project/blob/main/image/image_for_craw_data/vi_du_mot_bai_dang.png?raw=true" width="700"/>
</div>

Trang web là trang web tĩnh. Phân loại các bài đăng được thể hiện trong đường dẫn của kết quả tìm kiếm. Các mụ dữ liệu được bố trí cố định trong mỗi bài đăng.

### Mục tiêu

* Dự liệu thu được là dữ liệu thô chưa qua xử lý
* Nhóm chỉ tập trung vào các bất động sản nằm trong phân loại nhà ở ở Thành phố Hồ Chí Minh
* Dữ liệu thu được bao gồm các thông tin: tiêu đề bài viết, mã định danh bài viết, phân loại bất đông sản, số điện thoại người đăng, quận bất động sản được bán, ngày đăng bài, giá, diện tích, số phòng ngủ, số phòng wc, hướng nhà, hướng ban công, mô tả của bài viết
### Cách thức thực hiện
* *Quá trình 1*: Dựa vào cấu trúc đường dẫn của kết quả tìm kiếm, lấy tất cả các đường dẫn của những bài đăng cần quan tâm
* *Quá trình 2*: Sau khi đã có tất cả các đường dẫn cần thiết, lần lượt lấy các thông tin quan trọng của từng bài viết
### Công cụ sử dụng
* Thư viện Selenium
* Thư viện BeautifulSoup và requests
### Kết quả
Dữ liệu thu được sẽ lưu trữ ở đường dẫn sau: https://raw.githubusercontent.com/KhiemDangLe/Final-Project/main/DataFolder/2_raw_data_from_post.csv. Bao gồm các cột: 
* page_link: đường dẫn của bài viết
* title: tiêu đề bài viết
* article_id: mã định danh bài viết
* category: phân loại bất động sản
* phone: số điện thoại người đăng
* district: quận bất động sản được bán
* date_posted: ngày đăng bài
* price: giá
* area: diện tích
* bedroom: số phòng ngủ
* wc: số phòng wc
* direction: hướng nhà
* balcony_direction: hướng ban công
* description: mô tả của bài viết
## Tiền xử lý dữ liệu và phân tích dữ liệu
Bổ sung sau
## Tối ưu từng mô hình
Bổ sung sau
## So sánh các mô hình
Bổ sung sau
# Sản phẩm
Sản phẩm sau khi kết thúc đồ án bao gồm:
* Heatmap thể hiện sự phân bố của nhà ở theo địa chỉ của ngôi nhà. Địa chỉ của ngôi nhà là tên đường được trích xuất bằng mô hình LLM từ mô tả bài đăng
* Website dữ đoán giá nhà dựa trên các thông tin người dùng nhập vào với mô hình có kết qủa tốt nhầt là mô hình HistGradientBoostingRegressor. Website được triển khai tại: https://huggingface.co/spaces/Khoa710200/DS_2024