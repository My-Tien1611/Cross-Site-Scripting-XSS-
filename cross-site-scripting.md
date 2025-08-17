Tìm hiểu Cross-site scripting (XSS)
A.	LÝ THUYẾT
1.	Khái niệm và mục đích:
-	Khái niệm: Cross-site scripting (XSS) là lỗ hổng bảo mật web cho phép kẻ tấn công chèn và thực thi mã độc (thường là JavaScript) vào trình duyệt của người dùng thông qua một ứng dụng web dễ bị tấn công.
2.	Mục đích: Khai thác XSS nhằm 
	Mạo danh hoặc giả dạng là người dùng là nạn nhân.
	Thực hiện bất kỳ hành động nào mà người dùng có thể thực hiện.
	Đọc bất kỳ dữ liệu nào mà người dùng có thể truy cập.
	Ghi lại thông tin đăng nhập của người dùng.
	Thực hiện phá hoại ảo trang web.
	Chèn chức năng trojan vào trang web.
3.	Phân loại XSS:
-	Reflected XSS: Trong đó tập lệnh độc hại xuất phát từ yêu cầu HTTP hiện tại.
-	Stored XSS: Trong đó tập lệnh độc hại xuất phát từ cơ sở dữ liệu của trang web.
-	DOM-based XSS: Trong đó lỗ hổng tồn tại ở mã phía máy khách chứ không phải mã phía máy chủ
4.	Cơ chế hoạt động:
Tấn công cross-site scripting hoạt động bằng cách thao túng một trang web dễ bị tấn công để trả về mã JavaScript độc hại cho người dùng. Khi mã độc được thực thi bên trong trình duyệt của nạn nhân, kẻ tấn công có thể xâm phạm hoàn toàn tương tác của họ với ứng dụng.
<img width="975" height="561" alt="image" src="https://github.com/user-attachments/assets/a2da79da-80bc-43d5-8353-83b0e7057fd2" />
5.	Hậu quả: 
Tác động thực tế của một cuộc tấn công XSS thường phụ thuộc vào bản chất của ứng dụng, chức năng và dữ liệu của ứng dụng, cũng như trạng thái của người dùng bị xâm phạm. Ví dụ:
	Trong ứng dụng brochureware, nơi tất cả người dùng đều ẩn danh và mọi thông tin đều được công khai, tác động thường sẽ rất nhỏ.
	Trong ứng dụng lưu trữ dữ liệu nhạy cảm, chẳng hạn như giao dịch ngân hàng, email hoặc hồ sơ chăm sóc sức khỏe, tác động thường sẽ rất nghiêm trọng.
	Nếu người dùng bị xâm phạm có quyền cao hơn trong ứng dụng, thì tác động thường rất nghiêm trọng, cho phép kẻ tấn công kiểm soát hoàn toàn ứng dụng dễ bị tấn công và xâm phạm tất cả người dùng và dữ liệu của họ.
6.	Cách tìm và kiểm tra lỗ hổng XSS: 
-	Kiểm tra thủ công Reflected XSS và Stored XSS thường gồm việc gửi chuỗi đầu vào đơn giản vào mọi điểm nhập, tìm nơi đầu vào xuất hiện trong phản hồi HTTP và thử khai thác để thực thi JavaScript.
-	Kiểm tra DOM-based XSS từ tham số URL cũng tương tự, nhưng dùng công cụ dev của trình duyệt để tìm đầu vào trong DOM. Với DOM XSS từ nguồn khác (như document.cookie hoặc hàm không liên quan HTML), cần xem xét mã JavaScript thủ công — việc này rất tốn thời gian. Burp Suite hỗ trợ phát hiện DOM XSS bằng phân tích tĩnh kết hợp động.
7.	Cách ngăn chặn các cuộc tấn công XSS:
-	Lọc dữ liệu đầu vào khi nhận được. Tại thời điểm nhận được dữ liệu đầu vào của người dùng, hãy lọc càng chặt chẽ càng tốt dựa trên dữ liệu đầu vào mong đợi hoặc hợp lệ.
-	Mã hóa dữ liệu khi xuất. Tại điểm dữ liệu người dùng có thể kiểm soát được xuất ra trong phản hồi HTTP, hãy mã hóa dữ liệu xuất để tránh bị diễn giải thành nội dung đang hoạt động. Tùy thuộc vào ngữ cảnh xuất, việc này có thể yêu cầu kết hợp mã hóa HTML, URL, JavaScript và CSS.
-	Sử dụng tiêu đề phản hồi phù hợp. Để ngăn chặn XSS trong các phản hồi HTTP không chứa bất kỳ HTML hoặc JavaScript nào, bạn có thể sử dụng tiêu đề Content-Typevà X-Content-Type-Optionsđể đảm bảo trình duyệt diễn giải các phản hồi theo cách bạn muốn.
-	Chính sách Bảo mật Nội dung. Là tuyến phòng thủ cuối cùng, bạn có thể sử dụng Chính sách Bảo mật Nội dung (CSP) để giảm mức độ nghiêm trọng của bất kỳ lỗ hổng XSS nào vẫn còn tồn tại.
B.	THỰC HÀNH
1.	Reflected XSS:
Reflected XSS là dạng tấn công cross-site scripting đơn giản nhất. Nó phát sinh khi một ứng dụng nhận dữ liệu trong một yêu cầu HTTP và đưa dữ liệu đó vào phản hồi tức thời theo cách không an toàn.
Sau đây là một ví dụ đơn giản về lỗ hổng XSS phản ánh:https://insecure-website.com/status?message=All+is+well.<p>Status: All is well.</p>
Ứng dụng không thực hiện bất kỳ xử lý dữ liệu nào khác, do đó kẻ tấn công có thể dễ dàng thực hiện một cuộc tấn công như thế này:
https://insecure-website.com/status?message=<script>/*+Bad+stuff+here...+*/</script>
<p>Status: <script>/* Bad stuff here... */</script></p>
Nếu người dùng truy cập URL do kẻ tấn công tạo ra, tập lệnh của kẻ tấn công sẽ được thực thi trên trình duyệt của người dùng, trong bối cảnh phiên làm việc của người dùng với ứng dụng. Tại thời điểm đó, tập lệnh có thể thực hiện bất kỳ hành động nào và truy xuất bất kỳ dữ liệu nào mà người dùng có quyền truy cập.
1.1	XSS giữa các thẻ HTML
1.1.1 Reflected XSS vào HTML mà không được mã hóa:
-	Reflected XSS xảy ra khi dữ liệu người dùng nhập vào (trong URL, form, v.v.) được phản hồi lại trang HTML mà không qua bước mã hóa hoặc lọc.
-	"HTML context with nothing encoded" nghĩa là dữ liệu được đưa thẳng vào HTML mà không escape các ký tự đặc biệt (<, >, ", '...), nên attacker có thể chèn trực tiếp thẻ <script> hoặc HTML độc hại.
Bước 1: Gõ từ khóa "myiterm" vào thanh tìm kiếm → server trả về kết quả hiển thị "0 search results for 'myiterm'".
Bước 2: Trong HTML response, giá trị myiterm được chèn thẳng vào bên trong thẻ <h1> và một số phần khác mà không mã hóa.
<img width="975" height="515" alt="image" src="https://github.com/user-attachments/assets/9a44666e-8b06-4dc5-9661-b586226dfdd6" />
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/5e2ec2e2-50e8-4854-a5b7-9c2bda2c6295" />
Dữ liệu nhập (myiterm) nằm trực tiếp trong nội dung HTML → có khả năng chèn script.
Bước 3: Thử payload XSS:
Thay myiterm bằng <script>alert(1)</script> 
Server trả về payload nguyên vẹn trong HTML không hề encode → trình duyệt khi render sẽ chạy <script>alert(1)</script>.
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/3ef7c7c9-6a4d-4c9f-80d1-d3bf1ba9cd7c" />
Một popup alert(1) xuất hiện → chứng minh XSS thành công.
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/8cb3f2bd-db42-460d-a73c-87b614cb8bac" />
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/c5502edc-e145-49b8-832d-43d1fc9ce766" />
1.1.2 Reflected XSS vào HTML với hầu hết các thẻ và thuộc tính bị chặn
  
1.2	XSS trong thuộc tính thẻ HTML
1.2.1 Reflected XSS vào thuộc tính có dấu ngoặc nhọn được mã hóa HTML:
-	Đây là dạng Reflected XSS nhưng dữ liệu đầu vào được đưa vào giá trị của một thuộc tính HTML (ví dụ: value="", title="", onmouseover=""…).
-	Dấu ngoặc nhọn < > bị HTML encode → tức là nếu gửi <script>alert(1)</script> thì trang web sẽ hiển thị dưới dạng &lt;script&gt;... → không thực thi được.
-	Tuy nhiên, phần thuộc tính HTML lại chưa được lọc các ký tự đặc biệt như dấu nháy ", ' hoặc tên sự kiện (event handler) → vẫn có thể chèn JavaScript.
-	Bước 1: Gửi một chuỗi ký tự chữ và số ngẫu nhiên vào hộp tìm kiếm. Giá trị 1234 đang nằm bên trong thuộc tính value của thẻ <input>.
-	<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/df69a2d9-bb7e-4167-a8eb-cadd8c029e90" />
-	Bước 2: Thay thế đầu vào bằng "onmouseover="alert(1). Trên URL mã hóa thành: ?search="onmouseover%3Dalert(1). Trên render, nó trở thành:
<input type="text" placeholder="Search the blog..." name="search" value="" onmouseover=alert(1)>
<img width="802" height="451" alt="image" src="https://github.com/user-attachments/assets/673e4e6b-8482-4ab9-87e2-6a930d8caabd" />
Trình duyệt hiểu đây là một thẻ <input> có thuộc tính sự kiện onmouseover gán với alert(1). Khi người dùng di chuột vào, JavaScript alert(1)  chạy → popup xuất hiện.
<img width="813" height="457" alt="image" src="https://github.com/user-attachments/assets/2d3bde8b-8b91-426a-b0ea-5cfa0c8cc89f" />
1.3 XSS vào JavaScript: 
1.3.2 Reflected XSS vào chuỗi JavaScript có dấu ngoặc nhọn được mã hóa HTML
- Bối cảnh (Context): Payload người dùng nhập vào được chèn vào bên trong chuỗi JavaScript (string) trong HTML.
- Dấu ngoặc nhọn < > đã được mã hóa HTML → nghĩa là nếu chèn trực tiếp thẻ <script> sẽ bị vô hiệu (< → &lt;, > → &gt;).
- Tuy nhiên, ta vẫn có thể thoát ra khỏi chuỗi JavaScript bằng cách đóng chuỗi (dùng dấu ' hoặc ") và thêm mã JavaScript độc hại. 
Bước 1: Gửi một chuỗi ký tự chữ và số ngẫu nhiên vào hộp tìm kiếm. chuỗi ngẫu nhiên đã được phản ánh bên trong chuỗi JavaScript
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/6aa4abf4-c7f2-4489-bcda-7cc81ea7003d" />
Bước 2: Thay thế đầu vào bằng '-alert(1)-' để thoát khỏi chuỗi JavaScript
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/f66a9167-f7f2-4e80-bfc8-2c7bb576c9f1" />
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/c35ba8bd-0a7e-434c-8622-4ba869522cc0" />

  




















