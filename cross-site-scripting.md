<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/40b2c69c-f406-47ab-abac-bbfa95d0e67e" />Tìm hiểu Cross-site scripting (XSS)
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
- Lab này chứa lỗ hổng XSS phản ánh trong chức năng tìm kiếm nhưng sử dụng tường lửa ứng dụng web (WAF) để bảo vệ chống lại các vectơ XSS phổ biến.
- Để giải quyết bài toán này, hãy thực hiện một cuộc tấn công  bỏ qua WAF và gọi print().
  Bước 1: Thử chèn XSS bằng thẻ <img> với thuộc tính onerror để thực thi JavaScript.
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a5dfbe16-5ec5-4a4c-b11b-047256ba284c" />
  Server chặn thẻ/thuộc tính nguy hiểm và trả về "Tag is not allowed", nên payload không chạy được
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e4650a5a-3bb7-4365-836b-a27f74ffaf75" />
  Bước 2: Mở trình duyệt của Burp và sử dụng chức năng tìm kiếm trong phòng thí nghiệm. Gửi yêu cầu kết quả đến Burp Intruder.Thay thế giá trị của từ khóa tìm kiếm bằng:<§§>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2412ec63-086b-4b9f-b604-cfc07a806dd8" />
Bước 3: Truy cập bảng hướng dẫn XSS và nhấp vào Sao chép thẻ vào bảng tạm .
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bba03bd0-23bd-41d0-b1a1-7f5a774d0efc" />
Nhấp vào Bắt đầu tấn công
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b9fe4730-5800-429b-b71c-09596fc020ae" />
Đây là kết quả chạy Burp Intruder để fuzz các thẻ HTML xem thẻ nào được phép trên trang 
      + 400 kèm độ dài 113 → thẻ bị filter, server từ chối (“Tag is not allowed”).
      + 200 với độ dài lớn hơn (3476, 3480, 3487) → thẻ được phép và phản chiếu lại.
Bước 4: Quay lại Burp Intruder. Giá trị của thuật ngữ tìm kiếm bây giờ sẽ trông như sau:<body%20§§=1>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/01d2b1fe-3589-4266-8530-99803d9d80ab" />
Bước 5:  Truy cập bảng hướng dẫn XSS và nhấp vào Sao chép sự kiện vào bảng tạm . Nhấn bắt đầu tấn công
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d918a79b-f627-4c42-8a34-be9ea819de9b" />
Đây là kết quả fuzz các sự kiện (event handler) trong Burp Intruder để xem sự kiện nào không bị filter
Bước 6: Truy cập máy chủ khai thác và dán đoạn mã sau: <iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/530367f1-55ee-4375-bed4-db42fb451f08" />
Bước 7: Nhấp vào Lưu trữ và Gửi mã độc đến nạn nhân 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/56592649-0d9d-4534-94ce-611c0689bd0d" />
Kết quả: bypass filter bằng cách lợi dụng thẻ <body> (không bị chặn) và sự kiện onresize (cho phép), nhúng vào exploit server → khiến victim tự động kích hoạt payload → lab được solve.
1.1.3 Reflected XSS vào HTML với tất cả các thẻ bị chặn ngoại trừ các thẻ tùy chỉnh
Lab này chặn tất cả các thẻ HTML ngoại trừ các thẻ tùy chỉnh. Bằng cách thực hiện một cuộc tấn công chèn một thẻ tùy chỉnh và tự động cảnh báo document.cookie.
Bước 1: Truy cập máy chủ khai thác và dán đoạn mã sau: 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/24ebba63-ee2f-4a6d-99fb-f2b61c1c7cc3" />
Việc tiêm này tạo một thẻ tùy chỉnh với ID x, chứa trình xử lý sự kiện onfocus kích hoạt alert. Hash ở cuối URL sẽ tập trung vào phần tử này ngay khi trang được tải, khiến payload được gọi.
Bước 2: Nhấp vào "Lưu trữ" và "Gửi mã khai thác cho nạn nhân".
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/675f295c-9c06-4af8-961c-958e68b1a58c" />
1.1.4 Reflected XSS với trình xử lý sự kiện và thuộc tính href bị chặn
Lab này chứa lỗ hổng XSS phản ánh với một số thẻ được phép, nhưng tất cả các sự kiện và href đều bị chặn. Thực hiện một cuộc tấn công bằng cách chèn một vectơ, khi được nhấp vào, sẽ gọi alert đó.
Lưu ý cần gắn nhãn vector của mình bằng từ "Click" để khuyến khích người dùng nhấp vào vector của bạn
Bước 1: Truy cập URL sao: 
https://0a1400ca04d85c84801ea8ed00ff0023.web-security-academy.net/?search=%3Csvg%3E%3Ca%3E%3Canimate+attributeName%3Dhref+values%3Djavascript%3Aalert(1)+%2F%3E%3Ctext+x%3D20+y%3D20%3EClick%20me%3C%2Ftext%3E%3C%2Fa%3E
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/58a3a92e-67cb-42a7-ab34-7dd00092fdc4" />
Giải thích:
- Vì không thể gán trực tiếp href="javascript:...", ta dùng cơ chế SMIL animation trong SVG để thay đổi động thuộc tính href sau khi DOM load.
      + Dùng <animate attributeName=href values=javascript:alert(1) /> bên trong thẻ <a>.
      + Điều này khiến thuộc tính href của <a> được tự động gán thành javascript:alert(1).
- Sau đó, ta thêm <text> để hiển thị chữ “Click me” nhằm khuyến khích người dùng click.
Kết quả: 
- Khi trang phản xạ lại input này, SVG được parse.
- Animation gán href động cho <a>.
- Người dùng click vào dòng chữ “Click me” → javascript:alert(1) chạy → XSS thành công.
1.1.5 Reflected XSS với một số đánh dấu SVG được cho phép
Trang web đang chặn các thẻ phổ biến nhưng lại bỏ sót một số thẻ và sự kiện SVG.
Bước 1,2,3 làm giống mục 1.1.2 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6c74e242-f288-4d0a-a1ff-8d5af8cd187e" />
Khi cuộc tấn công kết thúc. Quan sát thấy tất cả các payload đều tạo ra phản hồi 400 , ngoại trừ những payload sử dụng thẻ <svg>, <animatetransform>, <title>, và <image>đã nhận được phản hồi 200.
Bước 4: Quay lại tab Intruder, thêm thuật ngữ tìm kiếm: <svg><animatetransform%20§§=1>. Truy cập bảng hướng dẫn XSS và nhấp vào Sao chép sự kiện vào bảng tạm .
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1230e39d-9c42-4252-84f0-7b934444ccc5" />
Bước 5: Nhấp vào bắt đầu tấn công
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ce9d93c1-3322-4de9-8e05-f350d838e633" />
Khi cuộc tấn công kết thúc tất cả các tải trọng đều gây ra phản hồi 400, ngoại trừ onbegin tải trọng gây ra phản hồi 200.
Bước 6: Nhập <svg><animateTransform onbegin=alert(1)'> vào ô Search
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/05f745a4-e976-48b5-8579-39e2a4d2f690" />
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
1.2.2 Reflected XSS trong thẻ liên kết chuẩn
Lab này phản ánh thông tin đầu vào của người dùng trong thẻ liên kết chuẩn và thoát khỏi dấu ngoặc nhọn. Thực hiện một cuộc tấn công mã lệnh chéo trang trên trang chủ để chèn một thuộc tính gọi alert.
Giả định rằng người dùng được mô phỏng sẽ nhấn các tổ hợp phím sau:
      + ALT+SHIFT+X
      + CTRL+ALT+X
      + Alt+X
Bước 1: Truy cập URL sau: https://0a0b0017037ae73d813e67ba00e900ce.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1) -> Nhấn: ALT+SHIFT+X
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1793e735-a3a3-42ee-8f27-df526cfcc622" />
- Thao tác này sẽ đặt X làm khóa truy cập cho toàn bộ trang. Khi người dùng nhấn phím truy cập, alert sẽ được gọi.
- Do input được phản ánh vào một thẻ HTML hợp lệ và escape dấu < >, ta chỉ có thể tiêm thuộc tính thay vì mở thẻ mới.
- Payload tận dụng accesskey + onclick để ép trình duyệt chạy JavaScript khi nạn nhân nhấn tổ hợp phím.
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

  




















