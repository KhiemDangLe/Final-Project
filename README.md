# 1. Tổng quan
* Chủ đề : Dự đoán giá nhà tại thành phố Hồ Chí Minh
* Nguồn dữ liệu: https://batdongsan.vn/
* Mục Tiêu của Dự Án:
     * Phát triển khả năng cào dữ liệu, EDA, vẽ heatmap, xữ lí dữ liệu và feature engineering,
     *  Phát triển một mô hình dự đoán giá với mô hình dự đoán chính xác nhất với dữ liệu do người dùng nhập vào.
     *  Có thể được sử dụng để hỗ trợ người dùng trong việc đưa ra các quyết định mua bán bất động sản, định giá căn nhà.

### 1.1. Cách tổ chức các file
* **DataFolder**: chứa dữ liệu thu thập được và dữ liệu phục vụ cho quá trình vẽ heatmap
* **Image**: chứa hình ảnh minh họa. Các hình ảnh mình họa được chèn vào trong các file notebook
* Các file report: là các file Jupyter Notebook chứa code và báo cáo của từng phần trong quá trình thực hiện đồ án. Các file được đánh số theo thứ tự thực hiện. Các file thuộc cùng một quá trình sẽ được đánh dấu thêm bằng chỉ số phụ. Ví dụ: file 5_1_report_gradient_boosting là file báo cáo quá trình thực hiện mô hình Gradient Boosting. 
* Các file khác, bao gồm:
  * **2_1_prompt_for_extracting.txt**: chứa prompt sử dụng để truy vấn mô hình LLM
  * **6_1_result_heat_map_geospatial.html, 6_2_result_heat_map_house_address**: chứa heatmap thể hiện sự phân bố của nhà ở theo địa chỉ của ngôi nhà. File này có thể mở trực tiếp trên trình duyệt để xem kết quả.
### 1.2. Thành viên nhóm
#### Tên Nhóm: 
* Vũ Đăng Khôi - 22280049
* Trảo An Huy - 22280041
* Đặng Lê Khiêm - 22280045
* Thái Anh Khoa - 22280048
### 1.3. Đóng góp của các thành viên
thêm link
# 2. Tóm tắt quá trình thực hiện
Link vẽ sơ đồ tóm tắt quá trình: https://github.com/KhiemDangLe/Final-Project/assets/121539558/17014389-2181-4ae0-ae6e-4cfb6ae76b90
## 2.1 Crawl dữ liệu
### 2.1.1 Sơ lược về trang web
<div style="text-align: center;">
    <img src="https://github.com/KhiemDangLe/Final-Project/blob/main/image/image_for_craw_data/vi_du_mot_bai_dang.png?raw=true" width="700"/>
</div>

Trang web là trang web tĩnh. Phân loại các bài đăng được thể hiện trong đường dẫn của kết quả tìm kiếm. Các mụ dữ liệu được bố trí cố định trong mỗi bài đăng.

### 2.1.2. Mục tiêu

* Dự liệu thu được là dữ liệu thô chưa qua xử lý
* Nhóm chỉ tập trung vào các bất động sản nằm trong phân loại nhà ở ở Thành phố Hồ Chí Minh
* Dữ liệu thu được bao gồm các thông tin: tiêu đề bài viết, mã định danh bài viết, phân loại bất đông sản, số điện thoại người đăng, quận bất động sản được bán, ngày đăng bài, giá, diện tích, số phòng ngủ, số phòng wc, hướng nhà, hướng ban công, mô tả của bài viết
### 2.1.3. Cách thức thực hiện
* *Quá trình 1*: Dựa vào cấu trúc đường dẫn của kết quả tìm kiếm, lấy tất cả các đường dẫn của những bài đăng cần quan tâm
* *Quá trình 2*: Sau khi đã có tất cả các đường dẫn cần thiết, lần lượt lấy các thông tin quan trọng của từng bài viết
### 2.1.4. Công cụ sử dụng
* Thư viện Selenium
* Thư viện BeautifulSoup và requests
### 2.1.5. Kết quả
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
## 2.3. Sử dụng mô hình LLM để trích xuất địa chỉ từ mô tả bài đăng
### 2.3.1  Sơ lược về LLM
* Các mô hình ngôn ngữ lớn (LLM) là các mô hình học sâu rất lớn, được đào tạo trước dựa trên một lượng dữ liệu khổng lồ. Bộ chuyển hóa cơ bản là tập hợp các mạng nơ-ron có một bộ mã hóa và một bộ giải mã với khả năng tự tập trung. Bộ mã hóa và bộ giải mã trích xuất ý nghĩa từ một chuỗi văn bản và hiểu mối quan hệ giữa các từ và cụm từ trong đó.
### 2.3.2 Mục tiêu
* Dùng LLM để trích xuất ra các dữ liệu dạng không cấu trúc  về thành các feature. Phục vụ cho việc train mô hình ở những giai đoạn sau đó.
* Dùng LLM để trích xuất ra các thông tin có thể có được từ ‘description’ mà nó có thể không có trong các thông tin trên wed. Ví dụ như ta trong phần mô tả có thể có thông tin về số phòng ngủ, phòng wc trong khi phần trên không có liệt kê.
* Dùng LLM để kiểm tra tính đúng đắn của bài đăng. Ví dụ có thể đó là một bài đăng về bán cửa sắt, vật dụng, không phải là bài đăng về bán nhà giúp việc dự đoán chính xác hơn.
### 2.3.3 Hướng thực hiện:
* Sử dụng API code được dùng trên Anyscale
* Tải dữ liệu cho LLM thực hiện
* Sử dụng các câu lệnh yêu cầu LLM trả về các thông tin theo yêu cầu dưới đây là ví dụ”
* Dùng API được cung cấp sau đó cho LLM thực hiện encode cột ‘title’ và ‘description’ sau đó đưa về dạng Json của các thông tin như sau:
* Sau đó truyền dữ liệu của các feature trên để tiến hành trích xuất dữ liệu.
* Xử lí thông tin nhận được từ LLM
* Do thông tin nhận được từ LLM không phải lúc nào cũng giống như dạng Json, nếu có đúng dạng thì nó vẫn có khả năng bị dư thừa một số thứ như ‘Below is the information you requested’ nó sẽ làm cho việc tải dữ liệu vào các cột bị lỗi. Nhóm dùng các thao tác xử lí chuỗi như Regex.
* Load dữ liệu nhận được vào dataframe
* Thực hiện tải kết quả nhận được vào LLM vào các file
### 2.3.4 Kết quả
Dữ liệu thô sau khi craw data, có thể truy cập ở đường dẫn sau: https://github.com/KhiemDangLe/Final-Project/blob/main/1-CrawlData/raw_data_3_extracted_by_LLM.csv

## 2.4. Quá trình ETL
### 2.4.1 Sơ Lược Về ETL:
Quy trình ETL đóng vai trò quan trọng trong việc chuyển đổi dữ liệu thô thành dữ liệu có ý nghĩa và có thể sử dụng được. Nó giúp đảm bảo dữ liệu được tích hợp, sạch sẽ và nhất quán, từ đó hỗ trợ việc phân tích và ra quyết định.
### 2.4.2 Mục tiêu.
*  Chuyển đổi dữ liệu từ nhiều nguồn thô thành dữ liệu có ý nghĩa và có thể sử dụng được.
### 2.4.3 Tóm tắt cách làm
#### 2.4.3.1. Extract trích xuất dữ liệu.
      - 1.1 Trích xuất dữ liệu LLM_data từ phần LLM.
      - 1.2 Trích xuất dữ liệu raw_data phần raw data.
#### 2.4.3.2. Transform dữ liệu.
      - 2.1 Transform cột article_id từ hai nguồn dữ liệu từ dạng float sang kiểu string.
      - 2.2 Transform join article_id từ hai nguồn dữ liệu raw_data và LLM_data thành merged_data.
      - 2.3 Transform cột price.
          - 2.3.1 Loại bỏ các cột giá Thỏa Thuận và transform cột giá.
          - 2.3.2 Cài đặt miền tối thiểu cho cột Price.
      - 2.4 Transform cột area.
          - 2.4.1 Thêm miền chặn dưới của cột area.
      - 2.5 Thêm cột area_per_m2
      - 2.6 Transform cột date_posted từ dạng object sang kiểu datetime64
      - 2.7 Transfrom cột location sang longitude với latitude

#### 2.4.3.3. Load Dữ liệu vào file merged data.csv.
### 2.4.4 Kết quả:
* file merged_data: https://raw.githubusercontent.com/KhiemDangLe/Final-Project/main/1-CrawlData/raw_data_4_merged.csv
  -   0   page_link            11044 non-null  object 
  -   1   title                11043 non-null  object 
  -   2   article_id           11043 non-null  float64
  -   3   category             11043 non-null  object 
  -   4   phone                11042 non-null  float64
  -   5   district             11043 non-null  object 
  -   6   date_posted          11043 non-null  object 
  -   7   area                 10505 non-null  float64
  -   8   bedroom              6797 non-null   float64
  -   9   wc                   6463 non-null   float64
  -   10  direction            1528 non-null   object 
  -   11  balcony_direction    997 non-null    object 
  -   12  description          11043 non-null  object 
  -   13  is_real_estate_post  11036 non-null  float64
  -   14  area_LLM             10197 non-null  float64
  -   15  bedroom_LLM          7466 non-null   float64
  -   16  total_room_LLm       4757 non-null   float64
  -   17  wc_LLM               5780 non-null   float64
  -   18  count_conveniences   11044 non-null  int64  
  -   19  alleyway_property    7484 non-null   float64
  -   20  under_mortgage       1809 non-null   float64
  -   21  residential_purpose  4345 non-null   float64
  -   22  furnished            2708 non-null   float64
  -   23  has_lowerlevel       2652 non-null   float64
  -   24  has_rooftop          3292 non-null   float64
  -   25  numbers_of_floors    9687 non-null   float64
  -   26  street_name          10102 non-null  object 
  -   27  price                11043 non-null  float64
  -   28  price_per_m2         10505 non-null  float64
* hai file location : https://github.com/user-attachments/files/15845426/merged_data_with_long_lat_0_5000.csv | https://github.com/user-attachments/files/15845227/merged_data_with_long_lat_5000_10000.csv')
  gồm các cột article_id, latitude, longitude
  
## 2.4. Tiền xử lý dữ liệu và phân tích dữ liệu
### 2.4.1. Sơ lược về tiền xử lý dữ liệu và phân tích khám phá dữ liệu
 * Là các bước quan trọng trong quy trình xử lý và phân tích dữ liệu. Chúng giúp chuẩn bị dữ liệu và hiểu rõ hơn về dữ liệu trước khi thực hiện các mô hình phân tích hoặc học máy.  Tuy nhiên, ở phần sau của đồ án, nhóm sẽ dùng nhiều loại mô hình khác nhau để dự đoán giá nhà.Do đó, mục đích chính của quá trình tiền xử lý dữ liệu là phục vụ cho quá trình phân tích khám phá dữ liệu mà không làm thay đổi quá nhiều đặc trưng của dữ liệu.

### 2.4.2. Mục tiêu
 * Tiền xử lý dữ liệu để loại bỏ đi những yếu tố chưa chính xác, không phù hợp với bài toán mà nhóm đặt ra 
Xử lý sơ phần cột định lượng và định tính
Thực hiện visualize để miêu tả dữ liệu

### 2.4.3. Kế hoạch thực hiện
* Xoá những phần không phù hợp như : hàng thiếu dữ liệu,các bài viết bị đăng lại nhiều lần,  các bài đăng spam (dùng cột is_real_estate_post),các bài đăng không nằm trong phân loại bất động sản về Nhà (dùng cột category), hàng trùng lặp
* Sử dụng dữ liệu được trích xuất từ phần mô tả như bedroom , wc, area để điền vào dữ liệu bị thiếu
* Xử lý cột định lượng : Xử lý cột price_per_m2,giá trị ngoại lai
* Xử lý cột định tính: cột residential_purpose,các cột còn lại
* Thực hiện visualize dữ liệu  một thuộc tính,  2 thuộc tính và 3 thuộc tính

### 2.4.4. Kết quả
* Ta sẽ nhận được bộ dữ liệu sạch, chuẩn hóa và nhất quán đảm bảo dữ liệu đã sẵn sàng cho các mô hình phân tích và học máy. Cũng như hiểu biết chi tiết về dữ liệu, thông qua các phân tích thống kê và trực quan, giúp định hướng cho việc lựa chọn và xây dựng mô hình phù hợp.
* Link github:https://raw.githubusercontent.com/KhiemDangLe/Final-Project/main/2-PreprocessingAndEDA/preprocessed_data.csv

## 2.5. Tối ưu từng mô hình
Mỗi thành viên nhóm sẽ chịu trách nhiệm tìm hiểu, áp dụng, tối ưu các mô hình sau:
* Mô hình Linear Regression, Ridge Regression, Lasso Regression: Trảo An Huy
* Mô hình K-Nearest Neighbors: Thái Anh Khoa
* Mô hình Gradient Boosting, Histogram Gradient Boosting: Đặng Lê Khiêm
* Mô hình Decision Tree, Random Forest: Vũ Đăng Khôi
## 2.6. So sánh các mô hình
Sau khi tối ưu các mô hình, nhóm sẽ so sánh các mô hình dựa trên các tiêu chí như: R2, MSE, thời gian chạy. Mô hình có kết quả tốt nhất sẽ được chọn để triển khai trên website
# 3. Sản phẩm
Sản phẩm sau khi kết thúc đồ án bao gồm:
* Heatmap thể hiện sự phân bố của nhà ở theo địa chỉ của ngôi nhà. Địa chỉ của ngôi nhà là tên đường được trích xuất bằng mô hình LLM từ mô tả bài đăng
* Website dữ đoán giá nhà dựa trên các thông tin người dùng nhập vào với mô hình có kết qủa tốt nhầt là mô hình HistGradientBoostingRegressor. Website được triển khai tại: https://huggingface.co/spaces/Khoa710200/DS_2024
