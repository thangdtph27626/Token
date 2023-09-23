# Token, access token và refresh token trong bảo mật

## Token là gì?

Token là chữ ký số hay chữ ký điện tử được mã hóa thành những con số trên thiết bị chuyên biệt. Mã Token tạo ra là dạng mã OTP nghĩa là mã sử dụng được một lần và tạo ngẫu nhiên cho mỗi giao dịch.
Token thường được các doanh nghiệp áp dụng cho những giao dịch thông thường và đặc biệt là giao dịch online. Bạn có xem như đây là một mật khẩu bắt buộc phải nhập cho mỗi giao dịch vì mục đích bảo mật.
Bằng việc sử dụng mã Token xác nhận giao dịch, các doanh nghiệp sẽ đảm bảo được sự chính xác. Một khi bạn đã xác nhận bằng mã Token có nghĩa bạn đã ký kết vào hợp đồng giao dịch mà không cần tốn thêm giấy tờ chứng minh nào. Mã Token hoàn toàn có giá trị pháp lý như chữ ký của bạn.

## Ưu và nhược điểm khi sử dụng Token
### Ưu điểm:

- Máy Token có kích thước nhỏ gọn, bạn có thể bỏ trong ví và mang đi khắp mọi nơi.
- Đây được xem là cách bảo mật an toàn nhất của ngân hàng và khả năng bạn bị mất tiền do giao dịch là không có.
- Mã OTP là mã sử dụng một lần nên nếu bị lộ cũng vị vô hiệu cho những giao dịch sau.
- Cách sử dụng máy Token rất dễ dàng.

### Nhược điểm:

- Để sử dụng, bạn bỏ ra chi phí mua máy Token từ 200.000-400.000đ.
- Mã Token thường chỉ có hiệu lực trong 60 giây.
- Bắt buộc phải có máy Token thì bạn mới có thể giao dịch được.

## Access Token và Refresh Token là gì?

Đầu tiên cho những ai chưa biết, AT là một đoạn string (thường là jwt) được dùng để đại diện cho người dùng thao tác vào hệ thống. Ví dụ như khi bạn đăng nhập vào hệ thống sẽ trả về cho bạn một AT, bạn lưu lại mã AT đó cho mỗi lần gọi API sau đó.

Nếu như các trang web được xây dựng theo phong cách server render thì ngoài cách triển khai AT và RT chúng ta còn có thể định phiên người dùng bằng session – cookie. Nhưng xu thế ngày nay đang hướng đến những trang web client render như React, Angular, Vue… thì việc triển khai AT và RT rất là phổ biến.

Còn RT là đoạn mã được dùng để yêu cầu cấp một mã AT mới. Chúng ta hãy làm rõ vấn đề tại sao lại cần phải dùng RT?

## Vấn đề ủy quyền là như thế nào?
Đầu tiên hãy làm rõ việc ủy quyền là gì? Thông thường việc xây dựng một website thì tính năng đăng ký/đăng nhập khá phổ biến. Đăng nhập thì bạn sẽ phải nhập một tài khoản + mật khẩu để xác nhận danh tính. Hệ thống xác nhận thông tin bạn nhập là chính xác thì sẽ trả về cho bạn một đoạn mã AT để đại diện cho bạn với một số quyền hạn nhất định trong hệ thống đó.

Hay trong Oauth, vấn đề ủy quyền thì nằm ở chỗ khi bạn bấm vào nút đăng ký/đăng nhập bằng tài khoản Google, Facebook… Hệ thống sẽ chuyển bạn qua một màn hình mà ở đó bạn sẽ phải đồng ý với việc cho phép cho hệ thống kia lấy một số thông tin như email, tên, ngày tháng năm sinh… của bạn. Khi đó thì Google hay Facebook cũng trả về một mã AT mà dựa vào đó, hệ thống kia có thể lấy được những thông tin của bạn phục vụ cho mục đính xác nhận danh tính.

Tóm lại, ủy quyền là việc xác nhận danh tính để lấy được một đoạn mã AT mà dựa vào mã AT đó có thể thay mặt người dùng để sử dụng được hệ thống.

Do đó, đôi khi việc có mã AT gần như là có luôn tài khoản của bạn trong một hệ thống nào đó. Việc bị lộ mã AT là rất nguy hiểm bởi vì kẻ tấn công có thể mạo danh bạn thao tác với hệ thống mà bạn hoàn toàn không hay biết. Chính vì lý do đó mã AT cần được lưu trữ một cách nghiêm ngặt nhất có thể, hoặc nếu không thì hãy giảm thời gian hiệu lực của mã AT lại, ví dụ như mã AT chỉ có hiệu lực trong thời gian 5 phút, hết 5 phút sẽ yêu cầu người dùng đăng nhập lại!? Nếu bạn chọn cách đó thì quả là một trải nghiệm tội tệ cho người dùng :D.

## Mối quan hệ của Access Token và JWT
Một điều cần lưu ý là mã AT thường là mã JWT (json web token). Nó là một tiêu chuẩn để truyền thông tin an toàn giữa các bên với dữ liệu là một đối tượng JSON. Thông tin này có tính tin cậy cao bởi vì nó đã được kí bằng mã bí mật bằng các sử dụng các thuật toán như HMAC, RSA hoặc ECDSA.

![image](https://github.com/thangdtph27626/Token/assets/109157942/f9c66c99-3361-4426-bb6c-7545a8470209)

Một đối tượng JWT gồm có 3 phần là header, payload và signature. Trong đó header giữ các thông tin cơ bản về chuỗi JWT như thuật toán dùng để kí, hạn sử dụng… Payload là dữ liệu cần truyền tải là đối tượng JSON đã được base64 encode. Cuối cùng là signature là phần mã hóa của cả hai header và payload kết hợp với mã bí mật cùng thuật toán mã hóa đã được chỉ định ở header.

Để tìm hiểu kĩ hơn về JWT, các bạn có thể đọc thêm ở Introduction to JSON Web Tokens .

Vì những thông tin cần thiết để truyền tải được nằm trong payload thế nên dựa vào payload hệ thống có thể xác định được danh tính của người dùng thông qua nó.

Ví dụ payload của tôi có nội dung như sau:

```
{
  "id" : 1,
  "username": "hoaitx"
}
```

Thì khi lên hệ thống sẽ đọc được id của tôi và xác định được tôi là ai.

Có một lưu ý cực kì quan trọng đó là thông tin trong payload chỉ được mã hóa bằng base64, điều đó có nghĩa từ mã JWT tôi có thể trích xuất được những thông tin có trong payload vì thế bạn cần thận trọng trong việc đưa thông tin vào payload trước khi kí chúng. Chắc chắn rằng những thông tin nhạy cảm như password, hay các thông tin cá nhân không cần thiết thì không nên đưa vào.

Khi mã JWT được gửi lên máy chủ có thể dễ dàng xác định được phần chữ kí trong đó có hợp lệ không. Bởi bất kì một thông tin nào trong payload được sửa đổi để gửi lên sẽ kéo theo chữ kí sẽ bị thay đổi theo. Chưa kể đến kẻ tấn công sẽ không bao giờ biết được mã bí mật dùng để kí lại nội dung bị thay đổi đó.

## Có nên dùng Refresh Token không?

Tôi sẽ không khuyến khích mọi người nên triển khai RT hay như thế nào. Vì tôi biết có nhiều hệ thống không cần triển khai đến RT mà vẫn hoạt động tốt. Thay vào đó tôi sẽ đưa ra một vài điểm cần chú ý khi sử dụng AT và RT.

Có một vài lý do để AT chỉ nên tồn tại trong thời gian ngắn như:

- Access Token là một đoạn mã ủy quyền đã được kí bởi máy chủ ủy quyền, máy chủ tài nguyên chỉ cần nhận access token và xác nhận thế nên thời gian càng ngắn thì nguy cơ access token khi bị lộ càng ít rủi ro.
- Nếu thời gian hợp lệ của AT quá dài, khi bị lộ AT thì rủi ro cao hơn vì t kẻ gian có nhiều thời gian hơn để khai thác, đồng thời có thể bạn ko biết rằng AT đã bị lộ.
- Hãy tưởng tượng nếu bạn cho phép người dùng tạo quá nhiều mã AT, lúc đó nếu muốn vô hiệu hóa những mã AT còn lại thì bạn phải đánh dấu nó trong cơ sở dữ liệu. Hay nói cách khác là bạn cần phải - lưu lại những mã AT đã được tạo ra.

Có một vài lý do để RT được sử dụng như:

- Rút ngắn thời gian tồn tại của AT, khi AT bị hết hạn thì sử dụng RT để lấy mã AT mới.
- Bạn cần phân biệt được máy chủ ủy quyền và máy chủ tài nguyên. Máy chủ ủy quyền có nhiệm vụ cung cấp mã RT và kí mã AT để đại diện cho người dùng. Máy chủ tài nguyên nhận mã đã kí từ máy chủ ủy quyền để có quyền như người dùng đang thao tác với dữ liệu của họ trên hệ thống như thông tin tài khoản, dữ liệu… Máy chủ ủy quyền và máy chủ tài nguyên có thể là một. RT thường được lưu trữ ở máy chủ ủy quyền phục vụ cho mục đích cấp mã AT mới. Còn mã AT được dùng ở máy chủ tài nguyên, dùng để định danh một người dùng.
- Bạn không cần phải lưu lại nhiều AT, nếu muốn chấm dứt phiên chỉ đơn giản là vô hiệu hóa mã RT trên máy chủ ủy quyền.
Sau khi hiểu được những lý do trên thì việc sinh ra RT giúp giải quyết được một vài hạn chế khi chỉ sử dụng mỗi AT. Nhưng như vậy nếu mã AT hoặc RT bị lộ thì rủi ro sẽ rất cao sao? Và các lưu trữ chúng như thế nào thì mình xin viết tiếp ở phần sau.

## Quy tắc bảo mật của access token và refresh token

Để đảm bảo bảo mật của access token và refresh token, cần tuân thủ các quy tắc sau:

- Access token phải được mã hóa: Access token phải được mã hóa để ngăn chặn kẻ tấn công đọc được nội dung của nó. Có nhiều phương pháp mã hóa khác nhau có thể được sử dụng, chẳng hạn như mã hóa HMAC hoặc mã hóa RSA.
- Access token phải có thời gian hết hạn ngắn: Access token nên có thời gian hết hạn ngắn để giảm thiểu rủi ro bị kẻ tấn công đánh cắp. Thông thường, thời gian hết hạn của access token là 15 phút hoặc ít hơn.
- Refresh token phải được lưu trữ an toàn: Refresh token nên được lưu trữ an toàn để kẻ tấn công không thể truy cập được. Refresh token có thể được lưu trữ trong cookie, session hoặc cơ sở dữ liệu.
- Access token và refresh token phải được sử dụng đúng cách: Access token và refresh token phải được sử dụng đúng cách để giảm thiểu rủi ro bị kẻ tấn công khai thác. Ví dụ, access token không nên được lưu trữ trong mã nguồn hoặc được gửi qua HTTP không an toàn.
  
Dưới đây là một số mẹo cụ thể để đảm bảo bảo mật của access token và refresh token:

- Sử dụng phương pháp mã hóa mạnh: Phương pháp mã hóa sử dụng để mã hóa access token phải đủ mạnh để ngăn chặn kẻ tấn công bẻ khóa.
- Giới hạn thời gian hết hạn của access token: Thời gian hết hạn của access token nên ngắn để giảm thiểu rủi ro bị kẻ tấn công đánh cắp.
- Sử dụng cookie httpOnly cho refresh token: Cookie httpOnly chỉ có thể được truy cập bởi trình duyệt web, giúp ngăn chặn kẻ tấn công truy cập refresh token từ phía máy chủ.
- Sử dụng cơ chế xác thực hai yếu tố (2FA) cho refresh token: 2FA thêm một lớp bảo mật bổ sung bằng cách yêu cầu người dùng nhập mã xác thực từ thiết bị thứ hai.
