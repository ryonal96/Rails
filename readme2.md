+Cấu hình MVC
-Models: là các tài nguyên trong ứng dụng, như người dùng, bài đăng, bài viết, bạn bè, công việc,...,
-Views: tạo nên phần giao diện của ứng dụng, những gì người dùng nhìn thấy trong trình duyệt hoặc trên các thiết bị di động. Views sẽ bao gồm HTML, CSS và JavaScript. Để sử dụng thêm được code Ruby thì file HTML sẽ được định dạng thêm .erb . Ví dụ : application.html.erb
-Controllers: là phần back-end, trung tâm xử lý, ví dụ: users_controller sẽ quản lý các chức năng của users
+Cách thức MVC hoạt động:
    -Từ trình duyệt sẽ tạo request: http://localhost:3000/home/about
    -Request sẽ được nhận tại config/routes.rb
            Rails.application.routes.draw do
                get 'home/about'
            end
        Khớp với resquet này GET "/home/about" to HomeController#about.
    -Request Routed sẽ dẫn đến Action thích hợp trong Controller:
        Request yêu cầu hành động about trong HomeController.
    -Action trong controller sẽ render Views hoặc liên hệ với Models
        class HomeController < ApplicationController
            def about
            end
        end
    -Models sẽ liên lạc với Database
    -Models gửi thông tin trở lại Controller
    -Controller sẽ render Views
        Rendering layout layouts/application.html.erb
        Rendering home/about.html.erb within layouts/application
    -Phản hổi được gửi đến trình duyệt.
        HTML được hiển thị được gửi trở lại trình duyệt dưới dạng phản hồi HTTP.