-Chapter 1

-Ứng dụng Rails được cấu hình sẵn với ba chế độ hoạt động tiêu chuẩn:
    Phát triển (Development): Dành cho việc phát triển ứng dụng, cho phép gỡ lỗi và thử nghiệm các tính năng mới.
    Kiểm thử (Test): Dành cho việc chạy các bài kiểm thử và đảm bảo chất lượng mã.
    Sản xuất (Production): Dành cho việc triển khai ứng dụng tới người dùng cuối với các tối ưu hóa hiệu suất.
    
-Tạo một ứng dụng Rails mới
    Để tạo một ứng dụng Rails mới, sử dụng các lệnh sau:
    -Mặc định (sử dụng SQLite3):
        rails new app_name
    -Sử dụng PostgreSQL hoặc MySQL:
        rails new app_name -d postgresql
    hoặc
        rails new app_name -d mysql
    Khuyến nghị sử dụng PostgreSQL cho hiệu suất và các tính năng tốt hơn.
    
-Bundler
    -Bundler quản lý các gem mà ứng dụng Rails của bạn yêu cầu. Đây là cách hoạt động của nó:
        Đọc Gemfile:
            Bundler bắt đầu bằng cách đọc Gemfile, liệt kê tất cả các gem mà ứng dụng của bạn yêu cầu.
        Đọc Gemfile.lock:
            Nếu có Gemfile.lock, Bundler sẽ đọc nó để biết các phiên bản cụ thể của các gem đã được cài đặt trước đó.
        Giải quyết phụ thuộc:
            Bundler chọn các phiên bản của mỗi gem sao cho chúng đều tương thích với nhau. Điều này có thể phức tạp vì các gem khác nhau có thể yêu cầu các phiên bản khác nhau của cùng một gem.
        Cập nhật Gemfile.lock:
            Sau khi giải quyết tất cả các phụ thuộc, Bundler sẽ cập nhật tệp Gemfile.lock để ghi lại các phiên bản chính xác của các gem đã được chọn.
        Đảm bảo nhất quán:
            Điều này đảm bảo rằng lần tiếp theo bạn hoặc một thành viên khác trong nhóm chạy bundle install (hoặc bundle), cùng một tập hợp các phiên bản sẽ được cài đặt, giữ cho môi trường phát triển và sản xuất nhất quán.
        
-Gemfile
    Yêu cầu:
        Rails yêu cầu tất cả các gem được liệt kê trong Gemfile khi khởi động.
    Sau khi chỉnh sửa:
        Sau khi chỉnh sửa Gemfile, sử dụng bundle install để cài đặt các gem mới được thêm vào
        
-RSpec and Haml

    Haml: Giúp mã HTML dễ đọc hơn và giảm bớt sự lặp lại.
    RSpec: Cung cấp các công cụ mạnh mẽ để viết và chạy các bài kiểm thử, giúp đảm bảo chất lượng và độ ổn định của ứng dụng.
    Debug: Hỗ trợ gỡ lỗi trong quá trình phát triển, giúp xác định và sửa lỗi nhanh chóng.
    
+Running a Rails application
    -bin/rails:
    require_relative "../config/boot"
        (yêu cầu tệp 'boot.rb' nằm trong thư mục 'config' và tệp 'boot.rb' thường được sử dụng để thiết lập các gem và môi trường cần thiết cho ứng dụng Rails trước khi được khởi động)
    require "rails/commands"
        yêu cầu tệp commands.rb(chứa các lệnh cần thiết để khởi động và chạy ứng dụng Rails, như server, console, db:migrate, và nhiều lệnh khác) từ thư viện rails (command.rb ở đâu ???)
    bin/rails thiết lập đường dẫn tới tệp application.rb , nạp các thiết lập cơ bản từ boot.rb và nạp các lệnh cần thiết từ thư viện Rails để quản lý và chạy ứng dụng.
    -config/boot.rb:

    ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../Gemfile', dir)
        để Bundler biết chính xác vị trí của Gemfile khi khởi động ứng dụng hoặc chạy các lệnh liên quan đến Bundler
    require 'bundler/setup'
        thiết lập môi trường để sử dụng các gem được liệt kê trong 'Gemfile' , đẩm bảo các gem đã được cài đặt và có phiên bản phù hợp với điều kiện trong 'Gemfile.lock'
    require 'bootsnap/setup'
        thiết lập Bootsnap để tăng tốc độ khởi động ứng dụng

    -config/application.rb:
    require_relative 'boot'
        Dòng này tải tệp config/boot.rb
    require 'rails/all'
        Dòng này tải toàn bộ thư viện của Rails (ActiveRecord, ActiveSupport, ActionPack, ActionMailer, ActiveJob, v.v.). Có thể loại trừ một số thânh phần không cần dùng.
    Bundler.require(*Rails.groups)
        yêu cầu các gem được liệt kê trong Gemfile đi theo các nhóm environment
    config.load_defaults 7.0
        tải các cấu hình mặc định cho phiên bản rails 7.0
    require "rails": Tải thư viện cơ bản của Rails mà không tải bất kỳ thành phần cụ thể nào.

    require "active_model/railtie": 
        Tải ActiveModel, một thư viện cung cấp các tính năng mô hình hóa cơ bản cho Rails.

    require "active_job/railtie": 
        Tải ActiveJob, một framework cho việc quản lý và chạy các công việc nền (background jobs).

    require "active_record/railtie": 
        Tải ActiveRecord, một ORM (Object-Relational Mapping) cho phép bạn làm việc với cơ sở dữ liệu một cách dễ dàng.

    require "active_storage/engine": 
        Tải ActiveStorage, một thư viện quản lý các tập tin tải lên (uploads) và lưu trữ chúng trên các dịch vụ lưu trữ như Amazon S3, Google Cloud Storage, v.v. (Không cần).

    require "action_controller/railtie": 
        Tải ActionController, thành phần chịu trách nhiệm xử lý các yêu cầu HTTP và đưa ra các phản hồi (responses).

    require "action_mailer/railtie": 
        Tải ActionMailer, một framework cho việc gửi email từ ứng dụng (Không cần).

    require "action_mailbox/engine": 
        Tải ActionMailbox, một framework xử lý các email nhận được (Không cần).

    require "action_text/engine": 
        Tải ActionText, một thư viện quản lý nội dung văn bản phong phú (rich text) (Không cần).

    require "action_view/railtie": 
        Tải ActionView, thành phần chịu trách nhiệm render các template cho phản hồi.

    require "action_cable/engine": 
        Tải ActionCable, một framework cho việc xử lý các kết nối WebSocket trong thời gian thực (Không cần).

    require "rails/test_unit/railtie": 
        Tải Rails TestUnit, một framework cho việc viết và chạy các test (Không cần)

    +Các câu lệnh để vào ActionView
        cd ~/.rvm/gems/ruby-3.2.2/gems/
        ls | grep actionview
        code actionview-7.0.8.3

    -Tệp config/application.rb là trung tâm cấu hình cho ứng dụng Rails. Nó đảm bảo rằng tất cả các gem cần thiết được tải đúng cách, thiết lập các cấu hình mặc định, và cho phép tùy chỉnh cấu hình cho từng môi trường. Việc sử dụng module và class giúp tổ chức mã nguồn một cách logic và cho phép chạy nhiều ứng dụng trong cùng một quy trình Ruby

    -Thư mục config/initializers là nơi quan trọng để thiết lập các cấu hình khởi tạo cho ứng dụng Rails 
        Tệp assets.rb quản lý các tệp tài sản (assets) như JavaScript, CSS, và hình ảnh trong ứng dụng Rails
        Tệp config/initializers/filter_parameter_logging.rb rất quan trọng để bảo vệ thông tin nhạy cảm trong ứng dụng Rails, cấu hình các tham số để bảo vệ các thông tin không bị lộ
+Zeitwerk
    Zeitwerk là một thư viện chịu trách nhiệm tự động tải (autoloading) các tệp Ruby trong các ứng dụng Rails, bằng cách tuân thủ các quy ước đặt tên, có thể tận dụng tối đa khả năng autoload của Zeitwerk

+Development Mode
    +Server timing 
        Để theo dõi các thông số và cải thiện hiệu năng ứng dụng

    +Verbose Query Logs
        Theo dõi và gỡ lỗi các yêu cầu trong ứng dụng
        Cung cấp thông tin chi tiết hơn về các truy vấn cơ sở dữ liệu, bao gồm vị trí trong mã nguồn nơi truy vấn được kích hoạt
    +Assets Debug Mode 
        Mặc dù mặc định việc ghi nhật ký cho các yêu cầu tài nguyên bị tắt, nhưng có những tình huống có thể muốn bật nó lên:
            Gỡ lỗi tài nguyên không tải đúng: Nếu các tài nguyên như JavaScript, CSS không tải đúng, việc bật ghi nhật ký có thể giúp xác định vấn đề.
            Phát hiện tài nguyên bị mất hoặc lỗi: Ghi nhật ký có thể giúp phát hiện khi nào một tài nguyên cụ thể bị mất hoặc gây ra lỗi.
            Kiểm tra đường dẫn tài nguyên: có thể xác minh rằng các đường dẫn tài nguyên được cấu hình và yêu cầu đúng.

+Test Mode 

+Production Mode
    Eager Load
        Để tăng tốc thời gian khởi động của máy chủ Rails trong quá trình phát triển, mã dự án và các thư viện  không được tải vào bộ nhớ ngay lập tức mà sẽ được tải khi cần thiết
    Assets
+Configuring a Database
    default: Thiết lập cấu hình mặc định, sử dụng biến môi trường để linh hoạt thay đổi giá trị.

    adapter: Sử dụng PostgreSQL.
    encoding: Mã hóa Unicode.
    pool: Số lượng kết nối tối đa trong pool, mặc định là 5.
    host: Địa chỉ máy chủ cơ sở dữ liệu, mặc định là localhost.
    username: Tên người dùng để kết nối cơ sở dữ liệu, mặc định là root.
    password: Mật khẩu kết nối, mặc định là Aa@123456.
    database: Tên cơ sở dữ liệu, mặc định lấy từ APP::NAME.
    socket: Đường dẫn socket, mặc định là /tmp/mysql.sock.
    port: Cổng kết nối cơ sở dữ liệu, mặc định là 5432.
    development: Kế thừa tất cả các thiết lập từ default. Có thể thêm hoặc ghi đè các giá trị cụ thể cho môi trường phát triển.

    Đoạn database: learn_development bị comment lại, có thể bật lên nếu cần cấu hình tên cơ sở dữ liệu cụ thể cho môi trường development.
    test: Kế thừa các thiết lập từ default, và định nghĩa tên cơ sở dữ liệu cho môi trường thử nghiệm.

    database: Tên cơ sở dữ liệu là learn_test.
    production: Kế thừa các thiết lập từ default.

    Các giá trị cụ thể như database, username và password được comment lại.
    Có thể bật lên và thiết lập giá trị từ biến môi trường cho môi trường production.
    Lợi ích của việc sử dụng Biến Môi Trường:
        Bảo mật: Giữ thông tin nhạy cảm như mật khẩu và tên người dùng ra khỏi mã nguồn.
        Linh hoạt: Dễ dàng thay đổi cấu hình giữa các môi trường mà không cần thay đổi mã nguồn.
        Quản lý: Dễ dàng quản lý và triển khai cấu hình trên các hệ thống
    Cấu hình database.yml sử dụng biến môi trường giúp tăng cường bảo mật và dễ dàng quản lý cấu hình giữa các môi trường khác nhau trong ứng dụng Rails

+Logging
    là quá trình ghi lại các sự kiện và thông điệp trong quá trình chạy ứng dụng. Logger trong Rails có thể được sử dụng để ghi lại các sự kiện với các mức độ nghiêm trọng khác nhau, từ debug đến fatal. Các thông điệp log có thể cung cấp thông tin hữu ích cho việc gỡ lỗi, theo dõi hiệu suất ứng dụng, hoặc giúp phát hiện và xử lý các vấn đề. Thông thường, các thông điệp log sẽ được ghi vào các tệp log tương ứng với mỗi môi trường, chẳng hạn như development, test, và production. Điều này giúp quản lý và phân tích log dễ dàng hơn
+Rack
    Rack là cầu nối giữa websever (puma) và ứng dụng rails
    Middleware là 1 phần của Rack: có các nhiệm vụ như logging, caching, xác thực trước khi vào Rails .