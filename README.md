### Chapter 1: Rails configurations and environments
=============
>Ứng dụng Rails được cấu hình sẵn với ba chế độ hoạt động tiêu chuẩn:
    Phát triển (Development): Dành cho việc phát triển ứng dụng, cho phép gỡ lỗi và thử nghiệm các tính năng mới.
    Kiểm thử (Test): Dành cho việc chạy các bài kiểm thử và đảm bảo chất lượng mã.
    Sản xuất (Production): Dành cho việc triển khai ứng dụng tới người dùng cuối với các tối ưu hóa hiệu suất.   
>Tạo một ứng dụng Rails mới
    Để tạo một ứng dụng Rails mới, sử dụng các lệnh sau:
    -Mặc định (sử dụng SQLite3):
        rails new app_name
    -Sử dụng PostgreSQL hoặc MySQL:
        rails new app_name -d postgresql
    hoặc
        rails new app_name -d mysql
    Khuyến nghị sử dụng PostgreSQL cho hiệu suất và các tính năng tốt hơn.  
>Bundler
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
>RSpec and Haml
    Haml: Giúp mã HTML dễ đọc hơn và giảm bớt sự lặp lại.
    RSpec: Cung cấp các công cụ mạnh mẽ để viết và chạy các bài kiểm thử, giúp đảm bảo chất lượng và độ ổn định của ứng dụng.
    Debug: Hỗ trợ gỡ lỗi trong quá trình phát triển, giúp xác định và sửa lỗi nhanh chóng.
+Running a Rails application
    >bin/rails:
    require_relative "../config/boot"
        (yêu cầu tệp 'boot.rb' nằm trong thư mục 'config' và tệp 'boot.rb' thường được sử dụng để thiết lập các gem và môi trường cần thiết cho ứng dụng Rails trước khi được khởi động)
    require "rails/commands"
        yêu cầu tệp commands.rb(chứa các lệnh cần thiết để khởi động và chạy ứng dụng Rails, như server, console, db:migrate, và nhiều lệnh khác) từ thư viện rails (command.rb ở đâu ???)
    bin/rails thiết lập đường dẫn tới tệp application.rb , nạp các thiết lập cơ bản từ boot.rb và nạp các lệnh cần thiết từ thư viện Rails để quản lý và chạy ứng dụng.
    >config/boot.rb:
    ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../Gemfile', dir)
        để Bundler biết chính xác vị trí của Gemfile khi khởi động ứng dụng hoặc chạy các lệnh liên quan đến Bundler
    require 'bundler/setup'
        thiết lập môi trường để sử dụng các gem được liệt kê trong 'Gemfile' , đẩm bảo các gem đã được cài đặt và có phiên bản phù hợp với điều kiện trong 'Gemfile.lock'
    require 'bootsnap/setup'
        thiết lập Bootsnap để tăng tốc độ khởi động ứng dụng
    >config/application.rb:
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
### Chapter 2: ROUTING

    >Các routes được định nghĩa trong tập tin config/routes.rb
    >Hai mục đích của Routing:
        +Mapping yêu cầu đến các phương thức action của controller
        +Tạo URL động: tạo các URL động để sử dụng trong các phương thức như 'link_to' và 'redirect_to'
    #root to: "static_pages#index"
        Định nghĩa route gốc của ứng dụng. Khi người dùng truy cập vào URL gốc ("/"), action index của controller static_pages sẽ được gọi.
    #get "/signup", to: "users#new"
        Định nghĩa route để hiển thị form đăng ký người dùng. Khi người dùng truy cập vào URL "/signup", action new của controller users sẽ được gọi.
    #get "/login", to: "sessions#new"
        Hiển thị form đăng nhập. Khi người dùng truy cập vào URL "/login" bằng phương thức GET, action new của controller sessions sẽ được gọi.
    #post "/login", to: "sessions#create"
        Xử lý dữ liệu đăng nhập khi người dùng gửi form. Khi người dùng gửi form đăng nhập (bằng phương thức POST), action create của controller sessions sẽ được gọi.
    #delete "/logout", to: "sessions#destroy"
        Định nghĩa route để đăng xuất người dùng. Khi người dùng gửi yêu cầu DELETE tới URL "/logout", action destroy của controller sessions sẽ được gọi để thực hiện quá trình đăng xuất
    #resources :shared_urls, only: %i(new create)
        Định nghĩa các routes RESTful cho tài nguyên shared_urls, nhưng chỉ tạo các routes cho các action new và create.
            new: Hiển thị form tạo mới shared_url.
            create: Xử lý việc tạo mới shared_url.
    #resources :users, only: %i(create new show)
    Định nghĩa các routes RESTful cho tài nguyên users, nhưng chỉ tạo các routes cho các action create, new và show.
        new: Hiển thị form đăng ký người dùng mới.
        create: Xử lý việc tạo mới người dùng.
        show: Hiển thị thông tin người dùng cụ thể
    Segment Keys
        Segment keys là các tham số được chèn vào trong chuỗi mẫu URL, được đánh dấu bằng dấu hai chấm (:)
        Segment keys là một phần quan trọng trong hệ thống routing của Rails, giúp linh hoạt trong việc định nghĩa và tạo các URL động. Hiểu rõ cách sử dụng segment keys và các phương thức liên quan sẽ giúp bạn tận dụng tối đa sức mạnh của hệ thống routing
        #get "products/:id" => "products#show", constraints: { :id => /\d+/ }
            Đoạn mã trên có nghĩa là:
                Định nghĩa một route get "products/:id".
                Route này sẽ chỉ khớp (match) nếu :id trong URL là một chuỗi số.
                Nếu :id không phải là một chuỗi số, route này sẽ không khớp và Rails sẽ tiếp tục tìm kiếm các route khác.
                Ví dụ URL khớp với route trên:
                /products/123 - khớp vì 123 là chuỗi số.
                /products/456 - khớp vì 456 là chuỗi số.
                Ví dụ URL không khớp với route trên:
                /products/abc - không khớp vì abc không phải là chuỗi số.
                /products/123abc - không khớp vì 123abc không phải là chuỗi số thuần túy.
            # config/routes.rb
            Rails.application.routes.draw do
                get "products/:id" => "products#show", constraints: { :id => /\d+/ }
            end
            # app/controllers/products_controller.rb
            class ProductsController < ApplicationController
                def show
                    @product = Product.find(params[:id])
                    # Do something with @product
                end
            end
    "constraints" là một cơ chế mạnh mẽ dùng để kiểm soát các điều kiện khi định tuyến các yêu cầu HTTP. Constraints có thể được sử dụng để giới hạn các tuyến đường (routes) dựa trên một số điều kiện cụ thể
        >Constraints cơ bản
            Constraints cơ bản được định nghĩa ngay trong tệp config/routes.rb để giới hạn các tuyến đường dựa trên các điều kiện như định dạng của URL, giá trị của tham số, hay giá trị của subdomain.
            Ví dụ:
            Rails.application.routes.draw do
            # Giới hạn tuyến đường chỉ cho subdomain 'admin'
            constraints subdomain: 'admin' do
                get 'dashboard', to: 'admin#dashboard'
            end
            # Giới hạn tuyến đường chỉ cho định dạng JSON
            constraints format: 'json' do
                get 'profile', to: 'users#profile'
            end
            end
        >Constraints nâng cao với Custom Constraints
            Có thể định nghĩa các constraints tùy chỉnh (custom constraints) bằng cách tạo ra các lớp hoặc mô-đun riêng. Các lớp hoặc mô-đun này phải có phương thức matches? trả về giá trị boolean.
            Ví dụ về Custom Constraint:
            Giả sử chúng ta muốn giới hạn truy cập vào một tuyến đường dựa trên địa chỉ IP của người dùng.
            class IpConstraint
            def initialize(allowed_ip)
                @allowed_ip = allowed_ip
            end
            def matches?(request)
                request.remote_ip == @allowed_ip
            end
            end
            Rails.application.routes.draw do
            constraints IpConstraint.new('192.168.1.1') do
                get 'admin', to: 'admin#dashboard'
            end
            end
            Trong ví dụ trên, chỉ những yêu cầu đến từ địa chỉ IP 192.168.1.1 mới có thể truy cập vào tuyến đường /admin.
        >Constraints với Regex
            Có thể sử dụng biểu thức chính quy (regex) để giới hạn các giá trị của tham số trong tuyến đường.
            Ví dụ về Regex Constraint:
            Rails.application.routes.draw do
            get 'products/:id', to: 'products#show', constraints: { id: /\d+/ }
            end
            Trong ví dụ này, chỉ những giá trị số (chỉ chứa các ký tự số) mới hợp lệ cho tham số :id.
        >Constraints với Lambda
            Lambda có thể được sử dụng như một cách ngắn gọn để định nghĩa constraints mà không cần tạo lớp hoặc mô-đun riêng.
            Ví dụ về Lambda Constraint:
            Rails.application.routes.draw do
            get 'admin', to: 'admin#dashboard', constraints: lambda { |req| req.remote_ip == '192.168.1.1' }
            end
        -Constraints kết hợp
            Có thể kết hợp nhiều constraints cùng lúc cho một tuyến đường để đạt được các điều kiện phức tạp hơn.
        Ví dụ về Constraints kết hợp:
            Rails.application.routes.draw do
            constraints(subdomain: 'admin', format: 'json') do
                get 'dashboard', to: 'admin#dashboard'
            end
            end
    class DateFormatConstraint 
        def self.matches?(request)
            request.params[:date] =~ /\A\d{4}-\d\d-\d\d\z/ # YYYY-MM-DD 
        end
    end
    #in routes.rb
    constraints(DateFormatConstraint) do 
        get 'since/:date' => :since
    end
    +phân tích code
        class DateFormatConstraint
            Đây là định nghĩa của một lớp Ruby có tên DateFormatConstraint.
            Lớp này được thiết kế để kiểm tra xem một yêu cầu (request) có chứa tham số ngày (:date) hợp lệ hay không
        def self.matches?(request)
            self.matches? là một phương thức tĩnh (class method), nghĩa là bạn có thể gọi nó trên lớp mà không cần phải khởi tạo một đối tượng của lớp đó.
            Phương thức này nhận vào một đối tượng request làm tham số. Đây thường là một đối tượng đại diện cho yêu cầu HTTP trong Rails.  
        request.params[:date]
            request.params là một hash chứa các tham số từ yêu cầu HTTP.
            request.params[:date] lấy giá trị của tham số :date từ yêu cầu
        =~ /\A\d{4}-\d\d-\d\d\z/ # YYYY-MM-DD
            =~ là toán tử khớp biểu thức chính quy trong Ruby.
            \A khớp với bắt đầu của chuỗi.
            \d{4} khớp với chính xác bốn chữ số, đại diện cho năm.
            - khớp với dấu gạch ngang.
            \d\d khớp với hai chữ số, đại diện cho tháng hoặc ngày.
            \z khớp với kết thúc của chuỗi.
            Toàn bộ biểu thức chính quy \A\d{4}-\d\d-\d\d\z đảm bảo rằng chuỗi phải khớp chính xác với định dạng YYYY-MM-DD, không nhiều hơn và không ít hơn.
        request.params[:date] =~ /\A\d{4}-\d\d-\d\d\z/
            Biểu thức chính quy này sẽ trả về true nếu chuỗi khớp với định dạng ngày, và nil nếu không khớp.
            Phương thức matches? sẽ trả về true hoặc false dựa trên kết quả của phép so sánh biểu thức chính quy.
    Mục đích:
        Lớp DateFormatConstraint và phương thức matches? được thiết kế để sử dụng trong định tuyến của Rails.
        Nó kiểm tra xem tham số :date trong một yêu cầu có hợp lệ theo định dạng YYYY-MM-DD hay không.
        Nếu tham số :date hợp lệ, các tuyến đường được định nghĩa trong khối constraints sẽ được khớp, nếu không, tuyến đường sẽ không khớp và Rails sẽ trả về trạng thái 404.
    #in routes.rb
    constraints(DateFormatConstraint) do
    get 'since/:date' => :since
    end
        Khi một yêu cầu đến với đường dẫn since/:date, phương thức matches? của DateFormatConstraint sẽ được gọi.
        Nếu tham số :date khớp với định dạng YYYY-MM-DD, tuyến đường get 'since/:date' => :since sẽ được xử lý bởi hành động since.
        Nếu không, Rails sẽ trả về trạng thái 404, ngăn chặn việc xử lý các yêu cầu không hợp lệ.
        Như vậy, đoạn mã trên giúp đảm bảo rằng chỉ các yêu cầu có tham số ngày hợp lệ mới được xử lý bởi các tuyến đường được bảo vệ bởi lớp ràng buộc này
    ------------------------
**Scope**:
   - `scope` trong Rails cho phép nhóm các tuyến đường (routes) với các tùy chọn giống nhau. Nó thường được sử dụng để áp dụng một tiền tố chung cho các tuyến đường hoặc để áp dụng các tùy chọn mặc định cho một nhóm tuyến đường.
   - Ví dụ:
     ```ruby
     scope '/admin' do
       resources :users
     end
     ```
     Các tuyến đường được tạo sẽ có tiền tố `/admin`, như `/admin/users`.
**Namespace**:
   - `namespace` tạo ra một phạm vi định tuyến có tên và cũng áp dụng các mô-đun điều khiển. Nó thường được sử dụng để nhóm các tuyến đường có liên quan đến nhau và quản lý chúng trong một thư mục con của thư mục điều khiển (controller).
   - Ví dụ:
     ```ruby
     namespace :admin do
       resources :users
     end
     ```
     Các tuyến đường được tạo sẽ có tiền tố `/admin`, như `/admin/users`, và các điều khiển tương ứng sẽ được tìm trong `Admin::UsersController`.
**Path**:
   - `path` trong Rails là phần của URL mà Rails sẽ khớp với tuyến đường đã định nghĩa. Bạn có thể sử dụng tùy chọn `path` để tùy chỉnh phần đường dẫn của tuyến đường.
   - Ví dụ:
     ```ruby
     resources :users, path: 'members'
     ```
     Các tuyến đường được tạo sẽ sử dụng `members` thay vì `users`, như `/members`.
**Resources**:
   - `resources` là một phương thức trong Rails để tạo ra một nhóm tuyến đường chuẩn cho một tài nguyên RESTful. Nó tạo ra các tuyến đường cho các hành động CRUD (Create, Read, Update, Delete) chuẩn.
   - Ví dụ:
     ```ruby
     resources :users
     ```
     Điều này sẽ tạo ra các tuyến đường như `/users`, `/users/new`, `/users/:id`, `/users/:id/edit`, v.v.
**Resource**:
   - `resource` tương tự như `resources`, nhưng nó tạo ra các tuyến đường cho một tài nguyên duy nhất. Điều này thường được sử dụng khi chỉ có một đối tượng của một loại tài nguyên cụ thể (ví dụ: một trang thông tin người dùng).
   - Ví dụ:
     ```ruby
     resource :profile
     ```
     Điều này sẽ tạo ra các tuyến đường như `/profile`, `/profile/new`, `/profile/edit`, v.v., nhưng không có các tuyến đường yêu cầu một `id` vì chỉ có một đối tượng.
Các khái niệm này giúp quản lý và tổ chức các tuyến đường trong ứng dụng Rails một cách hiệu quả và logic. Sự khác biệt chính giữa chúng nằm ở cách chúng nhóm, định danh, và quản lý các tuyến đường cũng như các điều khiển liên quan.
### Chapter 3: REST, Resources, and Rails
=========================================
    *REST (Representational State Transfer) là một tập hợp các nguyên tắc kiến trúc xác định cách sử dụng các tiêu chuẩn web, đặc biệt là HTTP, để tạo ra các dịch vụ web đơn giản và dễ mở rộng
        Kiến trúc Client-Server:
            Phân tách các chức năng bằng cách chia hệ thống thành các client và server. Client xử lý giao diện người dùng và trạng thái người dùng, trong khi server quản lý dữ liệu và logic của ứng dụng.
        Giao tiếp Không Trạng Thái (Stateless Communication):
            Mỗi yêu cầu từ client đến server phải chứa tất cả thông tin cần thiết để hiểu và xử lý yêu cầu. Server không lưu trữ bất kỳ trạng thái nào về phiên làm việc của client giữa các yêu cầu. Điều này giúp hệ thống RESTful dễ dàng mở rộng.
        Khả năng Cache (Cacheability):
            Các phản hồi phải tự định nghĩa là có thể cache được hay không để ngăn client tái sử dụng dữ liệu cũ hoặc không phù hợp trong các yêu cầu tiếp theo.
        Giao diện Đồng Nhất (Uniform Interface):
            Một cách giao tiếp tiêu chuẩn giữa client và server. Điều này đơn giản hóa và tách biệt kiến trúc, cho phép mỗi phần phát triển độc lập
    *Lợi Ích Của REST Trong Phát Triển Web
        Khả Năng Mở Rộng (Scalability):
            Nhờ giao tiếp không trạng thái và khả năng cache, các hệ thống RESTful có thể dễ dàng mở rộng theo chiều ngang, hỗ trợ nhiều client hơn chỉ bằng cách thêm nhiều server.
        Linh Hoạt Và Tính Mô Đun (Flexibility and Modularity):
            Giao diện đồng nhất cho phép các client khác nhau (web, mobile, thiết bị IoT) tương tác với cùng một dịch vụ RESTful theo cách tiêu chuẩn.
        Nhận Diện Ổn Định (Long-Lived Identifiers):
            Các URI ổn định đảm bảo rằng các tài nguyên có thể được tham chiếu một cách nhất quán theo thời gian, tạo điều kiện cho các tương tác client-server tin cậy.
*Controller Trong Rails Và REST
    Trong Rails, controller đóng vai trò trung tâm trong việc triển khai các route RESTful. Tuân theo các nguyên tắc REST thường dẫn đến việc tạo ra nhiều controller hơn, mỗi controller dành riêng cho một tài nguyên cụ thể hoặc một nhóm hành động liên quan. Cách tiếp cận này tránh được việc controller quá tải, vốn có thể trở nên khó quản lý và mở rộng.
*Routing Resourceful:
    Rails cung cấp một DSL (Domain-Specific Language) để khai báo các route resourceful, ánh xạ các động từ HTTP (GET, POST, PATCH, DELETE) đến các hành động controller theo cách tiêu chuẩn.
REST in Rails
    Rails đã tiếp nhận các nguyên tắc RESTful để giúp các nhà phát triển cấu trúc ứng dụng của họ. Bằng cách tuân theo REST, các ứng dụng Rails đạt được một thiết kế có tổ chức và dự đoán được, làm cho ứng dụng dễ điều hướng và bảo trì hơn
CRUD trong Rails
    Mọi thứ bắt đầu với CRUD:
        Create: Tạo mới tài nguyên.
        Read: Đọc hoặc truy xuất tài nguyên.
        Update: Cập nhật tài nguyên.
        Delete: Xóa tài nguyên.
    Rails sử dụng các phương thức HTTP tiêu chuẩn để ánh xạ các hành động CRUD vào các yêu cầu HTTP tương ứng:
        GET: Truy xuất thông tin từ server (đọc tài nguyên).
        POST: Gửi dữ liệu đến server để tạo mới tài nguyên.
        PUT/PATCH: Cập nhật tài nguyên hiện có.
        DELETE: Xóa tài nguyên.
    resources :users
        Chỉ cần dòng này trong routes.rb là tương đương với việc định nghĩa bốn named routes
        Index: Hiển thị danh sách các người dùng
        GET /users → users#index
        Show: Hiển thị chi tiết một người dùng cụ thể
        GET /users/:id → users#show
        New: Hiển thị form để tạo mới một người dùng
        GET /users/new → users#new
        Create: Tạo mới một người dùng
        POST /users → users#create
        Edit: Hiển thị form để chỉnh sửa một người dùng cụ thể
        GET /users/:id/edit → users#edit
        Update: Cập nhật một người dùng cụ thể
        PATCH /users/:id → users#update
        PUT /users/:id → users#update
        Destroy: Xóa một người dùng cụ thể
        DELETE /users/:id → users#destroy
    Ta có hành động đi với các phương thức lần lượt như sau
    GET: index, show, new, edit
    POST: create
    PUT/PATCH: update
    DELETE: destroy
    Vì các tên đường dẫn được kết hợp với phương thức yêu cầu HTTP, bạn cần biết cách chỉ định phương thức yêu cầu khi bạn tạo ra một URL để mà các yêu cầu GET và POST cho clients_url của bạn không kích hoạt cùng một hành động của controller. Hầu hết những gì bạn cần làm trong vấn đề này có thể được tóm tắt trong vài quy tắc:
    Phương thức yêu cầu mặc định là GET.
    Khi thêm form_with , phương thức POST sẽ được sử dụng tự động.
    Khi cần thiết (điều này chủ yếu xảy ra với các hoạt động PATCH và DELETE), bạn có thể chỉ định phương thức yêu cầu cùng với URL được tạo ra bởi tên đường dẫn có tên.
    Một ví dụ cụ thể khi cần chỉ định hoạt động DELETE là khi bạn muốn kích hoạt hành động destroy với một biểu mẫu:
    form_with url: auction_path(auction), method: :delete do |f|
    Sự khác nhau giữa PATCH và PUT
    PATCH được sử dụng để áp dụng cập nhật một phần cho tài nguyên, nghĩa là chỉ những trường cần thay đổi mới được gửi trong nội dung yêu cầu.
    PUT được sử dụng để thay thế toàn bộ tài nguyên bằng một cách thể hiện mới, nghĩa là tất cả các trường của tài nguyên đều được gửi trong body yêu cầu, ngay cả khi chúng không được sửa đổi.
    Khi cập nhật một tài nguyên trong Rails, bạn hiếm khi thay thế hoàn toàn nó mà thay vào đó là cập nhật một hoặc nhiều thuộc tính. Do đó, yêu cầu PATCH phù hợp hơn với ý nghĩa của hành động cập nhật điển hình trong Rails.
    Singular and Plural RESTful Routes
    một số tuyến đường RESTful là số ít; một số là số nhiều. Lý do là như sau:
    Các tuyến đường cho các hành động show, new, edit và destroy là số ít vì chúng hoạt động trên một tài nguyên cụ thể.
    Các tuyến đường còn lại là số nhiều. Chúng xử lý với các bộ sưu tập các tài nguyên liên quan.
    Các tuyến đường RESTful số ít yêu cầu một đối số vì chúng cần có thể tìm ra id của thành viên trong bộ sưu tập được tham chiếu.
    item_url(item)
    Bạn không cần phải gọi phương thức id trên item. Rails sẽ tự động xác định nó (bằng cách gọi to_param trên đối tượng được truyền vào).
    The Special Pairs: new/create and edit/update
    new và edit tuân theo các quy ước đặt tên RESTful đặc biệt. Lý do cho điều này liên quan đến cách create và update hoạt động và cách new và edit liên quan đến chúng. Thông thường, các thao tác create và update bao gồm việc gửi một biểu mẫu. Điều này có nghĩa là chúng thực sự bao gồm hai hành động—hai yêu cầu—mỗi cặp:
    Hành động dẫn đến việc hiển thị biểu mẫu
    Hành động xử lý đầu vào của biểu mẫu khi biểu mẫu được gửi
    Do đó, chúng ta có các cặp hành động như sau:
    new: Hiển thị biểu mẫu để tạo một tài nguyên mới.
    create: Xử lý dữ liệu biểu mẫu và thực hiện việc tạo tài nguyên.
    edit: Hiển thị biểu mẫu để chỉnh sửa một tài nguyên hiện có.
    update: Xử lý dữ liệu biểu mẫu và thực hiện việc cập nhật tài nguyên.
    Các hành động new và edit là cần thiết để chuẩn bị dữ liệu cho các hành động create và update tương ứng.
    Điểm mấu chốt, như được thực hiện trong RESTful Rails, là: Hành động new được hiểu là cung cấp cho bạn một tài nguyên mới, số ít (khác với số nhiều). Tuy nhiên, vì động từ logic cho giao dịch này là GET, và GETting một tài nguyên đơn lẻ đã được dành cho hành động show, nên new cần một tuyến đường có tên riêng của nó.
    link_to "Create a new item", new_item_path
    Cũng tương tự hành động edit sử dụng cùng một URL như show, nhưng với một loại biến đổi, dưới dạng /edit, đính kèm ở cuối, giống với định dạng URL cho new:
    Ví dụ:
    /items/5/edit
    Tuyến đường có tên tương ứng là edit_item_url(@item).
    The PATCH and DELETE Cheat
    Hầu hết các HTTP client đều có thể sử dụng các động từ này, nhưng các biểu mẫu trong trình duyệt web chỉ có thể được gửi bằng GET hoặc POST do tiêu chuẩn HTML chỉ cho phép hai động từ này trong các biểu mẫu nên Rails cung cấp một cách giải quyết. Một yêu cầu PATCH hoặc DELETE thực sự là một yêu cầu POST với một trường ẩn gọi là _method được đặt thành "patch" hoặc "delete". Ứng dụng Rails xử lý yêu cầu này sẽ nhận ra và định tuyến yêu cầu tương ứng đến hành động update hoặc destroy.
    form_with model: @item, method: :patch do |f|
    <%= f.submit "Update Item" %>
    end
    form_with url: item_path(@item), method: :delete do
    <%= submit_tag "Delete Item" %>
    end
    Limiting Routes Generated
    Bạn có thể thêm các tùy chọn :except và :only vào resources để giới hạn các tuyến đường được tạo ra.
    resources :clients, except: [:index]
    resources :clients, only: [:new, :create]
    Singular Resource Routes
    Ngoài resources, còn có một dạng tuyến đường tài nguyên đơn (singleton): resource. Nó được sử dụng để đại diện cho một tài nguyên chỉ tồn tại một lần trong ngữ cảnh cụ thể của nó. Ví dụ là một hồ sơ cá nhân cho mỗi người dùng. Khi bạn sử dụng resource, bạn sẽ nhận được hầu hết các tuyến đường tài nguyên đầy đủ, ngoại trừ tuyến đường dành cho tập hợp (collection route - index). Lưu ý rằng tên phương thức resource, tham số cho phương thức đó, và tất cả các tuyến đường được tạo ra đều ở dạng số ít.
    resource :profile
    Cụ thể, bạn sẽ nhận được các tuyến đường sau:
    GET /profile → profiles#show
    GET /profile/new → profiles#new
    POST /profile → profiles#create
    GET /profile/edit → profiles#edit
    PATCH/PUT /profile → profiles#update
    DELETE /profile → profiles#destroy
    Nested Resources
    Khi làm việc với các tài nguyên lồng nhau, bạn tạo ra một cấu trúc phân cấp trong các URL để biểu diễn mối quan hệ giữa các tài nguyên. Điều này đặc biệt hữu ích khi một tài nguyên gắn liền với một tài nguyên khác. Trong ví dụ của bạn, một bid (đấu giá) thuộc về một auction (cuộc đấu giá), vì vậy hợp lý khi lồng các bids trong auctions trong các routes của bạn. Để tạo các routes tài nguyên lồng nhau, bạn có thể thêm đoạn sau vào tệp config/routes.rb
    resources :auctions do
    resources :bids
    end
    Giải Thích:
    resources :auctions định nghĩa các routes tiêu chuẩn cho tài nguyên auctions (các phiên đấu giá). Nó sẽ tạo ra các routes cho các hành động CRUD (Create, Read, Update, Delete) như index, show, new, create, edit, update, và destroy.
    resources :bids bên trong block resources :auctions định nghĩa các routes tiêu chuẩn cho tài nguyên bids (các đấu thầu) và đặt chúng trong ngữ cảnh của tài nguyên cha auctions. Điều này có nghĩa là các routes cho bids sẽ phụ thuộc vào auctions, tức là bạn sẽ có các URL dạng như /auctions/:auction_id/bids.
    Tuy nhiên, lệnh tài nguyên lồng nhau cũng bao hàm rằng bạn đang thực hiện một lời hứa. Bạn đang hứa rằng bất cứ khi nào bạn sử dụng các helper route có tên cho bid, bạn sẽ cung cấp một tài nguyên auction để chúng có thể được lồng vào. Trong mã ứng dụng của bạn, điều này dịch thành một tham số cho phương thức route có tên:
    link_to "See all bids", auction_bids_path(auction)
    Các tài nguyên không nên được lồng nhau quá một cấp độ sâu. Các phương thức trợ giúp cho các đường dẫn lồng nhau hơn hai cấp độ trở nên dài và khó quản lý. Rất dễ mắc lỗi với chúng và khó tìm ra lỗi khi chúng không hoạt động như mong đợi. Lý do? Các URL kết quả ngắn hơn và các phương thức trợ giúp dễ làm việc hơn.
    resource routes chấp nhận một tùy chọn :shallow giúp rút ngắn các URL khi có thể. Mục tiêu là bỏ qua các đoạn URL của bộ sưu tập cha (parent collection) khi chúng không cần thiết. Kết quả cuối cùng là chỉ các tuyến lồng nhau cho các hành động :index :create, và :new được tạo ra.
    resources :auctions, shallow: true do
    resources :bids do
    resources :comments
    end
    end
    Routing Concerns
    Một trong những nguyên tắc cơ bản mà các lập trình viên Rails tuân theo là Don’t Repeat Yourself (DRY - Đừng lặp lại chính mình). Tuy nhiên, file config/routes.rb vẫn có thể dễ bị lặp lại dưới dạng các tuyến lồng nhau được chia sẻ giữa nhiều tài nguyên. Ví dụ, giả sử trong ví dụ liên tục của chúng ta, cả auctions và bids đều có thể có các comments liên kết với chúng.
    resources :auctions do
    resources :comments
    end
    resources :bids do
    resources :comments
    end
    Rails cung cấp một cách để tránh lặp lại bằng cách sử dụng concerns. Bạn có thể định nghĩa các tuyến chia sẻ trong một concern và sau đó bao gồm concern đó trong các tài nguyên khác nhau.
    Đầu tiên, bạn định nghĩa một concern cho các comments:
    concern :commentable do
    resources :comments
    end
    Sau đó, bạn có thể bao gồm concern đó trong các tài nguyên khác nhau:
    resources :auctions, concerns: :commentable
    resources :bids, concerns: :commentable
    Bằng cách này, bạn tránh được sự lặp lại và giữ cho file config/routes.rb của bạn gọn gàng và dễ bảo trì hơn.
    RESTful Route Customizations
    Có hai loại hành động tùy chỉnh: hành động thành viên (member actions) và hành động tập hợp (collection actions).
    Menber actions: ví dụ đoạn code sau
    resources :auctions do
    member do
    post 'mark_as_ended'
    end
    end
    Giải thích:
    member do ... end: Khối member được sử dụng để định nghĩa các đường dẫn tùy chỉnh cho một thành viên cụ thể của tài nguyên. Các đường dẫn được tạo ra sẽ chứa id của tài nguyên trong URL.
    post 'mark_as_ended': Định nghĩa một route tùy chỉnh mark_as_ended sử dụng phương thức HTTP POST.Đường dẫn này sẽ có dạng /auctions/:id/mark_as_ended, nơi :id là id của phiên đấu giá cụ thể mà bạn muốn đánh dấu là đã kết thúc.
    Collection actions: ví dụ đoạn code sau
    resources :auctions do
    collection do
    get 'search'
    end
    end
    Giải thích:
    collection do ... end: Khối collection được sử dụng để định nghĩa các đường dẫn tùy chỉnh cho toàn bộ bộ sưu tập của tài nguyên. Các đường dẫn này không yêu cầu id của một phiên đấu giá cụ thể trong URL.
    get 'search': Định nghĩa một route tùy chỉnh search sử dụng phương thức HTTP GET.Đường dẫn này sẽ có dạng /auctions/search, không cần id của phiên đấu giá cụ thể.
    Controller-Only Resources
    một tài nguyên REST không nhất thiết phải ánh xạ trực tiếp vào một controller. Bạn có thể, nếu muốn, cung cấp các dịch vụ REST mà các định danh công khai của chúng (URI) không khớp với tên của các controller của bạn.
    The RESTful Rails Action Set
    Index: Hiển thị danh sách các tài nguyên.
    Show: Hiển thị thông tin chi tiết về một tài nguyên cụ thể.
    Create: Hiển thị form để tạo mới một tài nguyên.
    New: Tạo mới một đối tượng tài nguyên (không phải là hành động RESTful riêng biệt nhưng thường đi kèm với Create).
    Edit: Hiển thị form để chỉnh sửa thông tin của một tài nguyên.
    Update: Cập nhật thông tin của một tài nguyên sau khi chỉnh sửa.
    Destroy: Xóa bỏ một tài nguyên khỏi hệ thống.
### Chapter 4: Working with Controllers
========================================
    Action Dispatch
    Action Dispatch là 1 mô-đun con của mô-đun Action Pack.
    Action Dispatch xử lý routing các request, nó xử lý các request và xử lý một vài quá trình liên quan đến giao thức HTTP như xử lý cookies, session…
    Phương thức draw trong file config/routes.rb gọi tới ActionDispatch::Routing::RouteSet
    Khi Rails app khởi động, RouteSet sẽ được khởi tạo. Nó build cái routes từ file routes.rb. Và nó cũng chính là thành phần đầu tiên nhận request từ client sau khi đi qua Rack và các middleware khác.
    RouteSet::Dispatcher
    Dispatcher là 1 class nhỏ chịu trách nhiệm khởi tạo controller và có nhiệm vụ chuyển request tới đúng controller để xử lý và trả về response.
    get "demo" => "demo#index"
    route trên sử dụng dispatcher. Ta có thể bỏ qua dispatcher bằng cách viết
    get "demo" => DemoController.action(:index)
    Ví dụ (A):
    Tạo democontroller trong app/controllers/demo_controller.rb với code sau:
    class DemoController < ActionController::Metal
    def index
    self.response_body = "Hello World!"
    end
    end
    Xong thêm code sau vào config/routes.rb:
    get "demo" => "demo#index"
    Xong truy cập vào localhost:3000/demo Web sẽ hiển thị “Hello World!”.
    Parameters
    Rails thu thập tham số từ 3 vị trí:
    Đường dẫn (the path)
    Truy vấn (the query)
    Thân (the body)
    Tất cả chúng đều được hợp nhất và cung cấp thông qua phương thức params dưới dạng một thể hiện của ActionController::Parameters.
    Tham số Query được thiết lập bằng cách gửi một biểu mẫu HTML với yêu cầu GET
    Tham số Body được thiết lập bằng cách gửi biểu mẫu qua POST và các phương thức khác vì chúng được giả lập bởi một yêu cầu POST.
    Tham số cũng có thể được cung cấp bởi một request gửi qua JavaScript hoặc một client API.
    Trong biểu mẫu (forms), danh sách các tham số là một danh sách phẳng của các cặp khóa/giá trị (key/value pairs):
    a=12&b=text&c=Hello
    Rails sẽ cung cấp cho chúng ta các tham số tương đương với hash sau:
    {"a"=>"12", "b"=>"text", "c"=>"Hello"}
    Rails hiện áp dụng hai quy tắc bổ sung để cho phép mảng và hash. Để đánh dấu một tham số là một mảng, đặt tham số trong “[]” , bằng cách này có thể mô tả mục bằng nhiều tham số
    countries[]=AT&countries[]=CH&countries[]=DE
    tương đương với:
    {"countries"=>["AT", "CH", "DE"]}
    Quy tắc còn lại là 1 tham số được mô tả bằng 1 hash gồm nhiều tham số
    person[name]=obie&person[occupation]=dj
    tương đương với:
    {"person"=>{"name"=>"obie", "occupation"=>"dj"}}
    Strong parameters
    Là các tham số được cung cấp dưới dạng một thể hiện của ActionController::Parameters có thuộc tính permitted được đặt là false
    params = ActionController::Parameters.new( person: { name: "obie", occupation: "dj" } )
    #=> ActionController::Parameters{"person"={
    "name"=>"obie","occupation"=>"dj"}} permitted: false>
    Để ngăn chặn người dùng đặt bất kỳ tham số nào họ muốn (ví dụ: tự thăng cấp lên vai trò quản trị viên), việc cung cấp một unpermitted cho một lần gán hàng loạt của một mô hình ActiveRecord sẽ gây ra một ngoại lệ:
    Person.new(params)
    #=> Exception raised: ActiveModel::ForbiddenAttributesError
    Bạn có hai lựa chọn: handpick hoặc permit attributes
    Render onto View…
    Mục tiêu của hành động controller điển hình trong một ứng dụng web truyền thống là để render một view template — tức là, điền vào template và giao kết quả, thường là một tài liệu HTML, lại cho máy chủ để chuyển đến trình duyệt.
    Trong app/views/demo/index.html.erb thêm mã sau:
    Hello World!
    Sau đó yêu cầu controller render template đó:
    class DemoController < ApplicationController
    def index
        render "demo/index"
    end
    end
    So sánh với ví dụ (A) ta thấy DemoController không còn kế thừa từ ActionController::Metal mà từ ApplicationController vì render là một tính năng được cung cấp bởi ActionController::Base. Nếu bạn truy cập lại trang, bạn vẫn sẽ được chào đón với “Hello World!”.
    When in Doubt, Render
    Ở cuối mỗi hành động của controller, nếu không có gì khác được chỉ định, hành vi mặc định là render template có tên khớp với tên của controller và action, trong trường hợp này nghĩa là app/views/demo/index.html.erb. Vì vậy, hãy thử mã sau:
    class DemoController < ApplicationController
    def index
    end
    end
    không cần phải gọi render rõ ràng, vì Rails giả định đó là điều bạn muốn.
    Nói cách khác, mỗi hành động controller có một lệnh render ngầm trong đó.
    Vào app/controller/demo_controller.rb, và xóa hành động index để file trông trống rỗng, như sau:
    class DemoController < ApplicationController
    end
    Rails biết rằng khi nó nhận được một yêu cầu cho hành động index của demo controller, điều thực sự quan trọng là gửi lại điều gì đó cho máy chủ. Vì vậy, nếu không có hành động index trong file controller, Rails chỉ đơn giản cho rằng, “Chà, hãy giả định rằng nếu có một hành động index, nó cũng sẽ trống thôi, và tôi chỉ cần render index.html.erb. Vậy đó là điều tôi sẽ làm.”
    Render Template của Hành Động Khác
    Một lý do phổ biến để render một template hoàn toàn khác là để hiển thị lại một form, khi nó được gửi với dữ liệu không hợp lệ và cần phải chỉnh sửa. Trong những trường hợp như vậy, chiến lược web thông thường là hiển thị lại form với dữ liệu đã gửi và đồng thời hiển thị một số thông tin lỗi, để người dùng có thể chỉnh sửa form và gửi lại.
    Lý do quá trình này liên quan đến việc render một template khác là vì hành động xử lý form và hành động hiển thị form thường là — khác nhau. Do đó, hành động xử lý form cần một cách để hiển thị lại template gốc (form), thay vì coi việc gửi form là thành công và chuyển sang màn hình tiếp theo.
    class EventController < ActionController::Base
    def new
    end
    def create
        @event = Event.new(params[:event])
        if @event.save
            redirect_to dashboard_path, notice: "Event created!"
        else
            render action: 'new' 
        end
    end
    end
    Giải thích :
    Phương thức new chỉ đơn giản là render ra template new.html.erb mà không có xử lý logic nào khác. Template này thường chứa biểu mẫu để người dùng nhập thông tin về sự kiện mới.
    Phương thức create xử lý dữ liệu từ biểu mẫu nhập liệu (new.html.erb). Dữ liệu nhập vào từ người dùng được lưu trong params, và thông tin của sự kiện mới được khởi tạo thông qua Event.new(params[:event]).
    Nếu sự kiện được lưu thành công (@event.save trả về true), người dùng sẽ được chuyển hướng đến dashboard_path và hiển thị thông báo "Event created!".
    Nếu không thành công (@event.save trả về false), phương thức render sẽ render lại action new. Điều này cho phép hiển thị lại biểu mẫu với thông báo lỗi hoặc trạng thái trước đó mà không cần gọi lại phương thức new.
    Lưu ý rằng template tự nó không “biết” rằng nó đã được render bởi hành động create thay vì hành động new. Nó chỉ làm công việc của nó: Nó điền, mở rộng và chèn, dựa trên các hướng dẫn nó chứa và dữ liệu (trong trường hợp này là @event) mà controller đã truyền cho nó.
    Render Một Template Hoàn Toàn Khác
    Tương tự như vậy, nếu bạn muốn render một template cho một hành động khác, bạn có thể render bất kỳ template nào trong ứng dụng của bạn bằng cách gọi phương thức render với một chuỗi chỉ đến tệp template mong muốn.
    render template: "/products/index.html.erb"
    Trong những trường hợp hiếm, bạn có thể sử dụng phương thức render để truy cập vào các template hoàn toàn nằm ngoài ứng dụng của bạn bằng cách sử dụng :file
    render file: "/u/apps/warehouse_app/current/app/views/products/show"
    Rendering a Partial Template: render 1 phần giao diên người dùng
    sử dụng tùy chọn :partial
    render partial: "product"
    Trong Rails, partials là các file view nhỏ được sử dụng để tái sử dụng các phần giao diện giống nhau trong các trang khác nhau của ứng dụng. Khi gọi render partial: "product", Rails sẽ tìm và render nội dung từ file _product.html.erb trong thư mục app/views/products/.
    Rendering HTML
    sử dụng tùy chọn :html
    render html: "Not Found".html_safe
    Rendering Inline Template Code
    sử dụng tùy chọn inline:
    render inline: "%span.foo #{@foo.name}", type: "haml"
    Rendering Text
    sử dụng tùy chọn plain:
    render plain: "Submission accepted"
    Rendering raw body output
    sử dụng tùy chọn body:
    render body: "Something very raw"
    Rendering Other Types of Structured Data
    :json
    render json: @record
    :xml
    render xml: @record
    Chính sách mặc định của việc render
    Trong mọi trường hợp, nếu Rails không thể tìm thấy template để render, hãy nhớ rằng nó sẽ phản hồi yêu cầu với "204 No Content". Điều khó khăn với mã trạng thái này là nhiều trình duyệt sẽ đơn giản bỏ qua nó và không làm gì cả. Nếu bạn không để ý đến những gì đang xảy ra (đặc biệt là trên cửa sổ console của Rails), bạn có thể nghĩ rằng trang trước đã được render lại, đặc biệt là nếu nó chứa một thông báo hoặc mã trạng thái lỗi.
    Rendering Options
    :content_type
    Options định dạng nội dung
    :layout
    Options cho phép chọn layout render ra
    :status
    Giao thức HTTP bao gồm nhiều mã trạng thái tiêu chuẩn chỉ ra nhiều điều kiện khác nhau trong phản hồi của máy chủ đối với yêu cầu của khách hàng. Rails sẽ tự động sử dụng trạng thái phù hợp cho hầu hết các trường hợp thông thường, chẳng hạn như 200 OK cho một yêu cầu thành công.
    Additional Layout Options
    có thể chỉ định các tùy chọn layout ở cấp lớp controller nếu bạn muốn tái sử dụng các layout cho nhiều hành động khác nhau.
    class EventController < ActionController::Base
    layout "events", only: [:index, :new]
    layout "global", except: [:index, :new]
    end
    Phương thức layout có thể chấp nhận một chuỗi (String), ký hiệu (Symbol), hoặc Boolean (kiểu dữ liệu biểu diễn hai giá trị: đúng (true) và sai (false)), kèm theo một hash các đối số.
    Redirecting
    Vòng đời của một ứng dụng Rails được chia thành các yêu cầu. Việc render một template, dù là template mặc định hay một template thay thế, hoặc render một partial hay một đoạn văn bản nào đó, là bước cuối cùng trong việc xử lý một yêu cầu. Tuy nhiên, việc chuyển hướng (redirect) có nghĩa là kết thúc yêu cầu hiện tại và yêu cầu khách hàng khởi tạo một yêu cầu mới
    Hãy xem ví dụ về phương thức xử lý form create sau đây:
    def create
    if @event.save
        redirect_to :index, notice: "Event created!"
    else
        render :new
    end
    end
    if @event.save: Kiểm tra xem việc lưu đối tượng @event vào cơ sở dữ liệu có thành công hay không.
    redirect_to, notice: "Event created!": Nếu việc lưu thành công, chuyển hướng đến hành động index và hiển thị thông báo "Sự kiện đã được tạo!".
    Trong trường hợp này, đó là hành động index. Logic ở đây là nếu bản ghi Event mới được lưu, bước tiếp theo là đưa người dùng trở lại top-level view
    Lý do chính để chuyển hướng thay vì chỉ render một template sau khi tạo hoặc chỉnh sửa một tài nguyên (thực sự là một hành động POST) liên quan đến hành vi tải lại của trình duyệt. Nếu bạn không chuyển hướng, người dùng sẽ được nhắc nhở để nộp lại biểu mẫu nếu họ nhấn nút quay lại hoặc tải lại trang.
    Phương thức redirect_to nhận hai tham số:
    redirect_to(target, response_status = {})
    Tham số target có thể nhận một trong các dạng sau:
    Hash: URL sẽ được tạo bằng cách gọi url_for với đối số được cung cấp.
    Đối tượng Active Record: URL sẽ được tạo bằng cách gọi url_for với đối tượng được cung cấp, đối tượng này nên tạo một URL có tên cho bản ghi đó.
    Chuỗi bắt đầu bằng giao thức như http://: Được sử dụng trực tiếp làm URL đích để chuyển hướng.
    Chuỗi không chứa giao thức: Giao thức và host hiện tại được thêm vào trước đối số và được sử dụng cho việc chuyển hướng.
    Proc: Một proc sẽ được thực thi trong ngữ cảnh của controller. Proc này nên trả về bất kỳ lựa chọn mục tiêu nào ở trên.
    Phương thức redirect_back
    Bạn có thể sử dụng phương thức redirect_back để chuyển hướng người dùng quay lại trang mà họ vừa đến từ, một kỹ thuật rất hữu ích trong các ứng dụng web truyền thống. Địa chỉ để "quay lại" được lấy từ tiêu đề HTTP_REFERER. Vì không đảm bảo rằng nó sẽ được thiết lập bởi trình duyệt, bạn phải cung cấp tham số fallback_location như một giá trị mặc định khi không có địa chỉ trước đó.
    redirect_back fallback_location: root_path
    Mặc định, phương thức redirect_back cho phép chuyển hướng đến một máy chủ khác. Tuy nhiên, bạn có thể ngăn chặn điều này bằng cách thiết lập tùy chọn allow_other_host thành false.
    redirect_back fallback_location: root_path, allow_other_host: false
    Điều này sẽ chỉ cho phép chuyển hướng quay lại cùng một máy chủ mà không chuyển đến các máy chủ khác.
    Controller/View Communication
    Khi một template view được render, thường nó sẽ sử dụng dữ liệu mà controller đã lấy từ cơ sở dữ liệu. Nói cách khác, controller nhận những gì nó cần từ tầng model và chuyển giao cho view.
    Rails thực hiện việc truyền dữ liệu từ controller tới view thông qua các biến instance. Thông thường, một hành động của controller khởi tạo một hoặc nhiều biến instance. Những biến instance đó sau đó có thể được sử dụng bởi view. Điều này được thực hiện thông qua hash assigns được cung cấp cho view khi khởi tạo.
    assigns hash là một biến instance trong controller được sử dụng để lưu trữ các giá trị mà bạn muốn truyền từ controller tới view. Đây là một phần của cơ chế kết hợp giữa controller và view trong Rails để truyền dữ liệu từ phía server (controller) sang phía client (view).
    Cách hoạt động của assigns hash:
    Định nghĩa và gán giá trị: Trên controller, bạn có thể gán các giá trị cho assigns thông qua các biến instance. Ví dụ:
    class PostsController < ApplicationController
    def show
        @post = Post.find(params[:id])
        @comments = @post.comments
        @tags = @post.tags
    # Các giá trị này sẽ được tự động lưu vào assigns hash
    end
    end
    Truyền dữ liệu cho view: Khi một view được render, Rails sẽ tự động chuyển assigns hash vào phạm vi của view template. Điều này cho phép bạn truy cập các giá trị này từ view một cách dễ dàng.
    Ví dụ, trong view show.html.erb:
    <%= @post.title %>
    Comments:
    <% @comments.each do |comment| %>
    <%= comment.body %>
    <% end %>
    Tags:
    <% @tags.each do |tag| %>
    <%= tag.name %>
    <% end %>
    Trong đoạn mã này, @post, @comments, và @tags đều được truy cập thông qua assigns hash.
    Tính chất của assigns: assigns là một hash bao gồm các cặp key-value, trong đó key là tên biến instance (có thể là symbol hoặc string) và value là giá trị của biến instance đó. Rails tự động tạo và quản lý assigns trong quá trình xử lý một request và gửi dữ liệu này sang view tương ứng.
    Gem "Decent Exposure" trong Rails là một thư viện giúp đơn giản hóa việc xử lý dữ liệu (data handling) trong các controller của ứng dụng Rails. Thư viện này được sử dụng để giảm thiểu việc lặp lại code và tăng tính tổ chức của mã nguồn bằng cách tự động tạo ra các phương thức truy cập dữ liệu cho các biến instance trong controller.
    Các tính năng chính của Decent Exposure:
    Tự động tạo các phương thức truy cập: Decent Exposure cho phép bạn định nghĩa các biến instance trong controller mà không cần viết nhiều code. Thay vì phải viết từng getter và setter cho từng biến instance, bạn chỉ cần định nghĩa một lần và Decent Exposure sẽ tự động tạo ra các phương thức truy cập tương ứng.
    Tăng tính mạnh mẽ và bảo mật: Thay vì trực tiếp sử dụng biến instance, Decent Exposure thúc đẩy việc sử dụng các phương thức để truy cập và thay đổi dữ liệu, giúp tăng tính bảo mật và quản lý dữ liệu.
    Hỗ trợ cấu hình linh hoạt: Decent Exposure cho phép bạn tùy chỉnh cách thức hoạt động thông qua các tùy chọn cấu hình, cho phép bạn điều chỉnh hành vi mặc định của gem theo yêu cầu cụ thể của ứng dụng.
    Action Callbacks
    Các action callbacks cho phép các controller chạy mã xử lý chung trước và sau khi thực hiện các hành động của họ. Những callback này có thể được sử dụng để thực hiện xác thực, caching hoặc kiểm định trước khi hành động dự định được thực hiện. Các khai báo callback là các phương thức lớp kiểu macro, tức là chúng xuất hiện ở đầu phương thức của controller, trong ngữ cảnh lớp, trước định nghĩa của phương thức. Vi du
    before_action :require_authentication
    Streaming
    là khả năng hỗ trợ gửi nội dung về cho client yêu cầu một cách liên tục (streaming), thay vì đợi cho đến khi toàn bộ nội dung được hoàn thành mới gửi đi. Điều này giúp cải thiện hiệu suất và trải nghiệm người dùng trong các trường hợp cần xử lý và truyền tải dữ liệu lớn hoặc dữ liệu đang được sinh ra theo thời gian thực.
    Streaming Templates
    Mặc định, khi một view được render trong Rails, nó trước tiên render template và sau đó là layout của view. Khi trả về một response cho client, tất cả các truy vấn Active Record cần thiết được thực thi và toàn bộ template được render một lần duy nhất.
    Tuy nhiên, Rails cũng hỗ trợ streaming các view đến client. Điều này cho phép các view được render và truyền dần dần khi chúng được xử lý, bao gồm chỉ thực thi các truy vấn Active Record cụ thể khi chúng cần thiết. Để làm được điều này, Rails đảo ngược thứ tự render bình thường. Layout được render trước thay vì sau, sau đó từng phần của template được xử lý.
    Để kích hoạt streaming view, bạn có thể truyền tuỳ chọn stream vào phương thức render
    class EventController < ActionController::Base
    def index
        @events = Events.all
        render stream: true
    end
    end
    Rails cũng hỗ trợ gửi các bộ đệm (buffers) và tập tin bằng hai phương thức trong module ActionController::DataStreaming: send_data và send_file.
    The respond_to Method
    Trong HTTP, định dạng của phản hồi được thương lượng giữa máy khách và máy chủ. Máy khách cung cấp cho máy chủ một danh sách các định dạng mà nó hiểu được theo độ ưu tiên thông qua tiêu đề Accept. Sau đó, máy chủ chọn một định dạng từ danh sách được cung cấp hoặc từ chối yêu cầu. Trong lệnh curl, chúng ta có thể thiết lập một tiêu đề yêu cầu bằng tùy chọn -H:
    $ curl http://localhost:3000/auctions/1 -H "Accept: application/json" -v
    GET /auctions/1 HTTP/1.1
    Host: localhost:3000
    User-Agent: curl/7.58.0
    Accept: application/json
    Điều này cho biết với máy chủ của chúng ta rằng máy khách chỉ chấp nhận JSON.
    $ curl http://localhost:3000/auctions/1.json -v
    GET /auctions/1 HTTP/1.1
    Host: localhost:3000
    User-Agent: curl/7.58.0
    Accept: /
    / Accept header chấp nhận bất kỳ định dạng nào.
    Trong Rails, controller có trách nhiệm phản ứng với định dạng yêu cầu từ phía máy khách. Phương thức respond_to cho phép bạn viết các hành động của mình sao cho nó sẽ trả về kết quả khác nhau dựa trên định dạng yêu cầu từ máy khách. Dưới đây là một ví dụ về hành động show cho controller products, cung cấp cả HTML và JSON:
    def show
    @auction = Auction.find(params[:id])
    respond_to do |format|
        format.html
        format.json { render json: @auction.to_json }
    end
    end
    Khối respond_to trong ví dụ này có hai điều kiện. Điều kiện HTML đơn giản chỉ có format.html. Yêu cầu cho HTML sẽ được xử lý bằng cách hiển thị một view template như thường lệ. Điều kiện JSON bao gồm một khối mã; nếu yêu cầu JSON, khối mã sẽ được thực thi và kết quả của nó sẽ được trả về cho máy khách.
    Nếu yêu cầu một định dạng mà không được định nghĩa trong khối respond_to, Rails sẽ không sinh ra một ngoại lệ. Thay vào đó, nó sẽ trả về mã trạng thái 406 Not Acceptable, để chỉ ra rằng nó không thể xử lý yêu cầu.
    Nếu bạn muốn thiết lập một điều kiện else cho khối respond_to của mình, bạn có thể sử dụng phương thức any, cho Rails biết để bắt các định dạng khác không được định nghĩa một cách rõ ràng:
    def show
    @product = Product.find(params[:id])
    respond_to do |format|
        format.html
        format.json { render json: @product.to_json }
        format.any
    end
    end
    Hãy chắc chắn rằng bạn đã chỉ định rõ ràng cho any cách xử lý yêu cầu hoặc có các view template tương ứng với các định dạng bạn mong đợi. Nếu không, bạn sẽ nhận được một ngoại lệ MissingTemplate.
    ActionView::MissingTemplate (Missing template products/show,
    application/show with {:locale=>[:en], :formats=>[:xml],
    :handlers=>[:erb, :builder, :raw, :ruby, :jbuilder, :coffee]}.)
    Variants
    Variants là một tính năng trong Rails cho phép render các template HTML, JSON, và XML khác nhau dựa trên một số tiêu chí nhất định.
### Chapter 5: Cookies, Session Management and the Flash
    Cookies
    Cookie là một mẩu thông tin mà máy chủ yêu cầu trình duyệt lưu trữ. Từ thời điểm đó, trình duyệt sẽ bao gồm cookie trong mọi yêu cầu nó gửi đến máy chủ. Chúng ta có thể sử dụng cookies để lưu trữ các tùy chọn của người dùng, chẳng hạn. Hãy lưu ý rằng cookies sẽ làm tăng kích thước của mỗi yêu cầu đến máy chủ của bạn.
    Bộ chứa cookie, như được biết đến, trông giống như một hash. Bạn có thể truy cập nó thông qua phương thức cookies trong phạm vi của các controller, view template và helper.
    Lưu ý rằng cookie được đọc theo giá trị, vì vậy bạn sẽ không nhận lại đối tượng cookie mà chỉ là giá trị nó giữ dưới dạng chuỗi (hoặc dưới dạng một mảng chuỗi nếu nó giữ nhiều giá trị).
    Để tạo hoặc cập nhật cookies, bạn gán giá trị sử dụng toán tử dấu ngoặc vuông. Bạn có thể gán một giá trị chuỗi đơn hoặc một hash chứa các tùy chọn. 
    Chúng ta có thể ghi cookie như sau:
    cookies[:list_mode] = "false"
    Chúng ta có thể đọc cookie như sau:
    cookies[:list_mode] # "false"
    Chúng ta cũng có thể xóa một cookie:
    cookies.delete(:list_mode)
    Một cookie có thời gian sống mặc định là "session". Thời gian sống của "session" được định nghĩa bởi trình duyệt, nhưng trong hầu hết các trường hợp, cookie này được lưu trữ cho đến khi bạn thoát trình duyệt. Nếu bạn muốn giữ nó lâu hơn, bạn có thể đặt thuộc tính "Expires". Trong Rails, chúng ta sử dụng tùy chọn expires, nhận một số giây làm đối số, để xác định khi nào cookie nên bị xóa bởi trình duyệt. 
    cookies[:recheck] = { value: "false", expires: 5.minutes.from_now }
    Bạn cũng có thể đặt nó thành một timestamp:
    cookies[:contact] = { value: "false", expires: Time.utc(2063, 4, 5) }
    Một trường hợp sử dụng phổ biến là đặt một cookie "vĩnh viễn". Vì điều này không phải là một phần của tiêu chuẩn HTTP, Rails định nghĩa vĩnh viễn là "20 năm". Bạn có thể đặt một cookie "permanent" như sau:
    cookies.permanent[:answer] = "42"
    Mỗi cookie đều có một phạm vi. Phạm vi bao gồm hai thuộc tính: domain và path.
    Theo mặc định, thuộc tính domain là nil. Điều này báo hiệu cho trình duyệt rằng cookie chỉ nên được gửi đến chính xác tên miền mà nó xuất phát từ đó, cụ thể là không bao gồm các tên miền con.
    Nếu bạn đặt tùy chọn :domain thành :all cookie sẽ được gửi cho các yêu cầu đối với tên miền và tất cả các tên miền con của nó
    cookies[:login] = {
        value: @user.security_token,
        domain: :all
    }
    Tuỳ chọn :path rất hữu ích vì nó cho phép bạn đặt các tùy chọn cụ thể cho các phần cụ thể hoặc thậm chí các bản ghi cụ thể của ứng dụng của bạn. Đường dẫn mô tả một tiền tố, vì vậy /ideas cũng áp dụng cho /ideas/1. Nó mặc định là /, do đó khớp với tất cả các đường dẫn.
    Cookie cũng có thuộc tính SameSite. Nó có ba giá trị khả thi:
    - Strict: Cookie chỉ được gửi đến cùng tên miền mà nó xuất phát từ đó.
    - Lax: Tương tự, ngoại trừ khi người dùng điều hướng đến một URL từ một trang web bên ngoài, chẳng hạn như khi theo dõi một liên kết.
    - None: Không có hạn chế về các yêu cầu chéo trang web.
    Cookie Access
    Chúng ta có thể hạn chế thêm quyền truy cập vào cookie:
    cookies[:account_number] = { value: @account.number, secure: true, httponly: true }
    với tuỳ chọn :httponly được đặt là true (mặc định là false), cookie sẽ không thể truy cập từ JavaScript.
    Với tuỳ chọn :secure được đặt là true, cookie chỉ được truyền qua kết nối HTTPS. Mặc định phụ thuộc vào cấu hình force_ssl của bạn: Nếu bạn bắt buộc sử dụng HTTPS, Rails cũng sẽ mặc định sử dụng cookie bảo mật. Mặc định điều này có nghĩa là bạn có :secure được bật trong môi trường production, nhưng không phải trong môi trường development hoặc testing.
    Signed and Encrypted Cookies
    Vì cookie được lưu trữ trong trình duyệt của người dùng, họ có thể đọc và thay đổi nó. Điều này có thể chấp nhận được đối với một số cookie. Đối với các cookie khác (như cookie phiên làm việc mà chúng ta sẽ tìm hiểu tiếp theo), điều này có thể không ổn.
    Nếu bạn muốn người dùng có thể đọc cookie nhưng không thể thay đổi nó, bạn có thể sử dụng signed cookie:
    cookies.signed[:user_id] = current_user.id
    Nếu người dùng thay đổi giá trị của cookie, Rails sẽ kiểm tra chữ ký và loại bỏ cookie. Tuy nhiên, vẫn có thể đọc cookie ở phía client. Điều này có thể hữu ích nếu bạn muốn một client JavaScript đọc cookie nhưng không thể sửa đổi nó. Nó hữu ích để bảo vệ thông tin nhạy cảm khỏi bị thay đổi mà không cần mã hóa.
    Nếu bạn muốn đảm bảo rằng cookie không thể đọc hoặc thay đổi, bạn có thể mã hóa nó:
    cookies.encrypted[:user_id] = current_user.id
    Session
    Bất cứ khi nào một người dùng mới truy cập vào ứng dụng Rails của chúng ta, một phiên làm việc mới sẽ được tự động tạo. Sử dụng phiên này, chúng ta có thể duy trì một lượng trạng thái phía máy chủ đủ để làm cho lập trình web trở nên dễ dàng hơn đáng kể.
    Accessing the Session
    API của phiên làm việc là một tập con của API Hash:
    # write to the session
    session[:current_user] = "Bob"
    # read from the session
    session[:current_user] # "Bob"
    # delete a key
    session.delete(:current_user) # "Bob"
    Ngoài ra, bạn có thể sử dụng phương thức reset_session được định nghĩa trong ActionController::Metal để đặt lại phiên làm việc:
    reset_session
    Storage Mechanisms
    Phiên làm việc được xác định bởi một session id duy nhất, một chuỗi 32 ký tự của các số hex ngẫu nhiên. Bất kể cơ chế lưu trữ được chọn là gì, session id này được lưu trữ trong một cookie. Như đã giải thích trong phần về cookie bảo mật, tất cả các cookie được đặt thành bảo mật khi bạn bật force_ssl. Hơn nữa, cookie phiên làm việc mặc định là httponly. Bạn có thể truy cập session id như sau:
    session[:session_id]
    Cache Store
    Tùy chọn lưu trữ phiên cache cho phép bạn sử dụng bất kỳ cái gì bạn đã cấu hình làm bộ nhớ cache làm kho dữ liệu phiên. Ngoài việc dễ dàng để cấu hình và vận hành, điều này cũng rất tiện lợi vì nó tích hợp sẵn hết hạn, có nghĩa là bạn không cần phải tự động hết hạn các phiên cũ.
    Để cấu hình nó, hãy tạo một tập tin khởi tạo trong config/initializers session_store.rb:
    Rails.application.config.
        session_store ActionDispatch::Session::CacheStore
    Hoặc, để cấu hình nó khác nhau cho mỗi môi trường, đặt nó trong tệp cấu hình tương ứng trong config/environments:
    config.session_store ActionDispatch::Session::CacheStore
    The Flash
    sử dụng flash[:notice] để lưu trữ các thông báo thông thường và flash[:alert] để truyền thông tin quan trọng hơn. Những thông điệp này sau đó được hiển thị ở một phần nổi bật trên HTML được render.
    Rails cho phép bạn thiết lập chúng như các tham số tùy chọn trong phương thức redirect_to:
    def create
        if user.try(:authorize, params[:user][:password])
        redirect_to home_url, notice: "Welcome, #{user.first_name}!"
        else
        redirect_to :new, alert: "Login invalid."
        end
    end
    Đôi khi bạn muốn hiển thị một thông báo flash cho người dùng nhưng chỉ trong yêu cầu hiện tại. Thực tế, một lỗi lập trình phổ biến của các lập trình viên mới vào Rails là đặt thông báo flash mà không chuyển hướng, dẫn đến hiển thị thông báo flash sai trong yêu cầu tiếp theo.
    Để làm cho flash hoạt động cùng với lệnh render, bạn có thể sử dụng phương thức flash.now
    class ReportController < ActionController::Base
        def create
        if report.save
            redirect_to report_path(report), notice: "#{report.title} has been created."
        else
            flash.now.alert = "#{report.title} could not be created."
            render :new
        end
        end
    end
    Đối tượng flash.now cũng có các truy cập vào notice và alert, tương tự như đối tượng flash truyền thống của nó. Phương thức flash.now được sử dụng để hiển thị thông báo flash ngay trong quá trình render hiện tại, thay vì chờ cho đến khi chuyển hướng đến yêu cầu tiếp theo.
### Chapter 6: Action View & Haml
=================================