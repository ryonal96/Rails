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

