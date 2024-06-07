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
ROUTING
    -Các routes được định nghĩa trong tập tin config/routes.rb
    -Hai mục đích của Routing:
        +Mapping yêu cầu đến các phương thức action của controller
        +Tạo URL động: tạo các URL động để sử dụng trong các phương thức như 'link_to' và 'redirect_to'
        
    
# root to: "static_pages#index"
    Định nghĩa route gốc của ứng dụng. Khi người dùng truy cập vào URL gốc ("/"), action index của controller static_pages sẽ được gọi.

# get "/signup", to: "users#new"
    Định nghĩa route để hiển thị form đăng ký người dùng. Khi người dùng truy cập vào URL "/signup", action new của controller users sẽ được gọi.

# get "/login", to: "sessions#new"
    Hiển thị form đăng nhập. Khi người dùng truy cập vào URL "/login" bằng phương thức GET, action new của controller sessions sẽ được gọi.

# post "/login", to: "sessions#create"
    Xử lý dữ liệu đăng nhập khi người dùng gửi form. Khi người dùng gửi form đăng nhập (bằng phương thức POST), action create của controller sessions sẽ được gọi.

# delete "/logout", to: "sessions#destroy"
    Định nghĩa route để đăng xuất người dùng. Khi người dùng gửi yêu cầu DELETE tới URL "/logout", action destroy của controller sessions sẽ được gọi để thực hiện quá trình đăng xuất

# resources :shared_urls, only: %i(new create)
    Định nghĩa các routes RESTful cho tài nguyên shared_urls, nhưng chỉ tạo các routes cho các action new và create.
        new: Hiển thị form tạo mới shared_url.
        create: Xử lý việc tạo mới shared_url.

# resources :users, only: %i(create new show)
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
        -Constraints cơ bản
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
        -Constraints nâng cao với Custom Constraints
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

        -Constraints với Regex
            Có thể sử dụng biểu thức chính quy (regex) để giới hạn các giá trị của tham số trong tuyến đường.

            Ví dụ về Regex Constraint:
            Rails.application.routes.draw do
            get 'products/:id', to: 'products#show', constraints: { id: /\d+/ }
            end
            Trong ví dụ này, chỉ những giá trị số (chỉ chứa các ký tự số) mới hợp lệ cho tham số :id.

        -Constraints với Lambda
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
==========
class DateFormatConstraint 
    def self.matches?(request)
        request.params[:date] =~ /\A\d{4}-\d\d-\d\d\z/ # YYYY-MM-DD 
    end
end
# in routes.rb
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

# in routes.rb
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
