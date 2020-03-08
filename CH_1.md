# CH-1. **MOSTLY STATIC PAGES**



*이 문서는 Michael Hartl 의 The Ruby on Rails Tutorial 4판을 참고해서 작성하였습니다.*



1. 레일즈 시작하기

   Rails 와 Ruby 가 모두 깔려있다고 가정하고 시작하겠습니다.

   먼저 터미널에 Rails <버전> new <이름> 을 적어주세요.

   예)

   ```bash
   rails _6.0.2.1_ new rails_app
   ```

   단, rails를 처음 시작할 때는 sqlite 기반이므로, 만약 postgresql 기반으로 하시고 싶으면

   ``` bash
   rails _6.0.2.1_ new rails_app --database=postgresql 
   ```

   커맨드라인에 위와같이 쳐주시면 됩니다.

   그 다음 db를 생성하고, 서버를 켜주고, 127.0.0.1:3000 로 들어가면, 레일즈 초기 화면이 보입니다.

   ```bash
   rake db:create && rake db:migrate
   rails s
   ```

   

   cd **rails_app** 을 쳐서 **rails_app** 폴더에 들어갑니다. 

   

   application_controller.rb 파일에 처음 들어가면 

   ```ruby
   class ApplicationController < ActionController::Base
   end
   ```

   이런식으로 코드가 적혀있는 것을 볼 수 있을 것 입니다.

   

   application_controller.rb 파일의 이 부분을 다음과 같이 바꿔줍니다.

   ```ruby 
   class ApplicationController < ActionController::Base 
    protect_from_forgery with: :exception
     
    def hello
     render html: "hello, world!"
    end 
   end
   ```

   

   그다음 

   ```ruby
   rails s
   ```

   를 커맨드라인에 쳐주고, 나온 화면을 확인합니다.

   

   아마도 그대로일 것 입니다.

   이 부분은 '컨트롤러' 로 hello 라는 액션을 실행시켰을때, "hello, world!" 라는 html을 띄워주라는 의미입니다.

   그렇기 때문에 라우트 파일에서 hello 라는 액션의 라우트를 정해주면 됩니다.

   

   config/routes.rb 파일에 들어가셔서 아래와 같이 작성합니다.

   ```Ruby
   Rails.application.routes.draw do 
    root 'application#hello'
   end
   ```

   여기서 root 가 앞에 붙는 이유는 우리가 127.0.0.1:3000 에 접속했을 때 가장 처음에 띄우라는 이야기입니다.

   참고로 원래 root 는 application.html.erb 입니다.

   그 다음 다시 서버를 켜주시면 hello, world! 가 적혀있는 페이지를 볼 수 있죠.

   

   이제 controller를 직접 만들어보겠습니다. 

   ```ruby
   rails generate controller StaticPages home help
   ```

   이렇게 쳐주면, \<StaticPages> controller를 만들고, 그 안에 home 과 help 액션을 추가하라는 의미입니다.

   만약 컨트롤러를 만들었는데, 지우고 싶으면 어떻게 할 수 있을까요?

   ```Ruby
   rails destroy controller StaticPages home help
   ```

   이렇게 하면 controller와 연관된 모든것이 삭제됩니다.

   

   자 일단 controller 를 만들어봅시다.

   그러면 controller 폴더 안에 static_pages_controller 가 생긴것을 볼 수 있습니다.

   어떻게 생겼나요?

   ```ruby
   class StaticPagesController < ApplicationController
     def home
     end
   
     def help
     end
   end
   ```

   이렇게 생겼습니다.

   

   ApplicationController 라는 부모클래스를 상속받는 StaticPagesController 자식클래스가 생긴 것입니다.

   super 키워드를 이용하면 자식클래스에서 부모클래스의 메소드에 직접 접근할 수 있습니다.

   

   config/routes 를 들여다 봅시다.

   

   ```ruby
   Rails.application.routes.draw do
     get 'static_pages/home'
     get 'static_pages/help'
     root 'application#hello'
   end
   ```

   이런식으로 되어있습니다.

   보면, application#hello 이고, static_pages/home 이렇게 되어있어서 헷갈릴 수 있습니다.

   라우터는 (get/put/patch 등등) (사용자가 접근할 url) (원하는 액션) 이렇게 이루어져 있습니다.

   예를 들면

   ```ruby
   get 'static_pages/home'
   get 'static_pages/home', to: 'static_pages#home'
   ```

   위와 아래는 서로 같습니다.

   위의 표현이 더 축약한 것이죠.

   

   그러면 root로 되어있는 경우는 어떤 의미일까요?

   ```ruby
   root 'application#hello'
   get '/', to: 'application#hello'
   ```

   위처럼, 가장 기본 url을 나타내는 / 가 root 를 나타내는 것이죠.

   마찬가지로 둘 다 같은 표현입니다만, 위의 표현이 더 간단하고 좋습니다.

   

   이제 rails s 명령어를 치고, 127.0.0.1:3000/static_pages/home 으로 가봅시다.

   라우트에 'static_pages/home' 이라는 url 을 추가했고, 그 url은 home 액션을 나타내므로, home 액션이 나타나겠네요.

   

   한가지 설명을 빠뜨렸네요.

   컨트롤러에 있는 액션은 뷰를 한개씩 가질 수 있습니다.

   app/views/static_pages 에 들어가보면, home.html.erb , help.html.erb 두가지 html.erb 형태의 뷰가 만들어진 것을 볼 수 있죠. 한마디로, url을 들어갔을때 컨트롤러의 home 액션이 실행되면서 home.html.erb라는 뷰가 보이게 됩니다.

   모든 액션이 뷰를 통해 보여질 필요는 없죠. 뷰와 뷰 사이를 전환하는 사이의 액션일수도 있습니다.

   그럴 때에는 뷰의 <액션>.html.erb 파일을 꼭 만들 필요는 없습니다.

   

   이제 뷰를 바꿔보겠습니다.

   app/views/static_pages/home.html.erb 를 다음과 같이 바꿔봅시다.

   ```ruby
   <h1>Sample App</h1> 
   <p>
    This is the home page for the
    <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a> 
    sample application.
   </p>
   ```

   

   마찬가지로

   app/views/static_pages/help.html.erb 도 다음과 같이 바꿔봅시다.

   ```Ruby
   <h1>Help</h1> 
   <p>Get help on the Ruby on Rails Tutorial at the
     <a href="https://www.railstutorial.org/help">Rails Tutorial help page</a>. To get help on this sample app, see the
     <a href="https://www.railstutorial.org/book"><em>Ruby on Rails Tutorial</em>book</a>.
   </p>
   ```



그 다음 127.0.0.1:3000/static_pages/home, 127.0.0.1:3000/static_pages/help 에 들어가보면 뷰가 나와있는 것을 볼 수 있습니다.

자 그다음은 TDD에 관한 내용입니다. 

TDD 는 test-driven development 의 줄임말입니다.

TDD는 말그대로 제대로 동작하는지 테스트 하는 것인데요, 직접 서버를 켜서 확인하지 않아도, rails test 라는 명령어를 통해 올바르게 작동하는지 확인할 수 있습니다. 

test 종류는 

1. controller test
2. model test
3. Integration test

이렇게 3가지가 있습니다. integration test 가 가장 강력하고 좋지만, TDD의 쉬운 접근을 위해 controller test로 시작해보겠습니다.

먼저,

```bash
test/controllers/
```

안에있는 static_pages_controller_test.rb 파일을 엽니다.

그 다음

*test/controllers/static_pages_controller_test.rb*

```ruby
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest
  test "should get home" do
    get static_pages_home_url
    assert_response :success
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
  end

end
```

이렇게 바꿉니다.

```bash
rails generate controller
```

이 명령어를 쳤을 때, 테스트파일이 만들어집니다. 그래서 위와 같은 테스트 파일이 존재하는 것 입니다.

static_pages_controller에 home과 help를 만들었죠? 위의 테스트는 home과 help의 url이 정상적으로 작동하는지 확인하는 코드입니다.

그 다음

```bash
rails test
```

를 쳐서 실행시켜봅니다.

```bash
$ rails test
2 tests, 2 assertions, 0 failures, 0 errors, 0 skips
```

이렇게 나오면 성공입니다.

자 테스트 파일을 

```ruby
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest
  test "should get home" do
    get static_pages_home_url
    assert_response :success
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
  end
  
  test "should get about" do 
    get static_pages_about_url 
    assert_response :success
  end
end
```

이렇게 바꿔봅시다.

맨 마지막에 about의 url test 구문을 넣어보았습니다. 현재 우리는 about 컨트롤러가 없어서 오류가 나올 것 입니다. 한번 rails test 를 해서 확인해보죠.

```bash
$ rails test
3 tests, 2 assertions, 0 failures, 1 errors, 0 skips
```

이런식으로 나오고, static_pages_about_url 이 없다고 나옵니다.

이 에러메시지는 About page의 URL을 위한 rails 코드가 없다는 이야기이고, 라우트 파일에 추가하기를 원한다는 힌트이죠. 그렇기 때문에 라우트파일에 about url을 추가해보겠습니다.

*config/routes.rb*

```ruby
Rails.application.routes.draw do
  get 'static_pages/home'
  get 'static_pages/help'
  get 'static_pages/about'
  get '/', to: 'application#hello'
end
```

이렇게 추가를 했습니다.

이제 다시 rails test를 해볼까요?

또 오류가 날 것 입니다.

```bash
AbstractController::ActionNotFound: The action 'about' could not be found for StaticPagesController
```

오류메시지는 위와 같은데, about 액션이 없다고 하네요?

그렇다면 컨트롤러에 가서 about 액션을 추가해야겠네요!

*app/controllers/static_pages_controller.rb*

```ruby
class StaticPagesController < ApplicationController
  def home
  end

  def help
  end

  def about
  end  
end
```

이렇게 하고 rails test를 돌려봅시다.

돌려봐도 또 오류가 납니다.

```bash
ActionController::MissingExactTemplate: StaticPagesController#about is missing a template for request formats: text/html
```

이렇다는군요.

레일즈에서 템플릿이 없다는 말은 뷰가 없다는 말과 동일합니다. 

예를들어 home 이라는 컨트롤러는 home.html.erb 파일을 가지고 있죠. 그렇지만 about은 그 파일을 가지고 있지 않습니다.

그래서 app/views/static_pages 에 about.html.erb 파일을 만들어 줍시다.

*app/views/static_pages/about.html.erb*

```html
<h1>About</h1> 
<p>
 The 
 <a href="https://www.railstutorial.org/"><em>Ruby on Rails Tutorial</em></a> is a
 <a href="https://www.railstutorial.org/book">book</a> and
 <a href="http://screencasts.railstutorial.org/">screencast series</a> to teach web development with
 <a href="http://rubyonrails.org/">Ruby on Rails</a>.
 This is the sample application for the tutorial. 
</p>
```

그리고 그 안에 about view에 대한 코드를 넣어줍니다.

test도 해보고 브라우저에서 직접 확인해봐도 오류가 나지 않는다는 것을 알 수 있습니다.

테스트파일에 assert_select ~~~~ 이 부분을 추가해봅시다.

assert_select는 view에 있는 html에서 태그와 그 내용이 실제로 있는지 확인하는 것 입니다.

```ruby
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest
  test "should get home" do
    get static_pages_home_url
    assert_response :success
    assert_select "title", "Home | Ruby on Rails Tutorial Sample App"
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
    assert_select "title", "Help | Ruby on Rails Tutorial Sample App"
  end
  
  test "should get about" do 
    get static_pages_about_url 
    assert_response :success
    assert_select "title", "About | Ruby on Rails Tutorial Sample App"
  end
end
```

지금 rails test를 하면 오류가 뜰 것 입니다. 왜냐하면 view 파일에 title이 저렇게 설정되어있지 않기 때문이죠.

그렇기 때문에 html 파일을 바꿔봅시다.

*app/views/static_pages/home.html.erb*

```html
<!DOCTYPE html>
<html> 
 <head>
  <title>Home | Ruby on Rails Tutorial Sample App</title> 
 </head>

 <body>
  <h1>Sample App</h1> 
  <p>
  This is the home page for the
  <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a> 
  sample application.
  </p> 
 </body>
</html>
```

*app/views/static_pages/about.html.erb*

```html
<!DOCTYPE html>
<html> 
  <head>
    <title>About | Ruby on Rails Tutorial Sample App</title> 
  </head>
  
  <body> 
    <h1>About</h1> 
    <p>
    The <a href="https://www.railstutorial.org/"><em>Ruby on Rails Tutorial</em></a> is a
    <a href="https://www.railstutorial.org/book">book</a> and
    <a href="http://screencasts.railstutorial.org/">screencast series</a> to teach web development with
    <a href="http://rubyonrails.org/">Ruby on Rails</a>.
    This is the sample application for the tutorial. 
    </p>
  </body> 
</html>
```

*app/views/static_pages/help.html.erb*

```html
<!DOCTYPE html>
<html> 
  <head>
    <title>Help | Ruby on Rails Tutorial Sample App</title> 
  </head>
  
  <body> 
  <h1>Help</h1> 
  <p>
  Get help on the Ruby on Rails Tutorial at the
  <a href="https://www.railstutorial.org/help">Rails Tutorial help page</a>.
  To get help on this sample app, see the
  <a href="https://www.railstutorial.org/book"><em>Ruby on Rails Tutorial</em> book</a>.
  </p> 
  </body>
</html>
```

이렇게 3개의 view를 모두 바꿔줍시다.

그렇게 하고 rails test 를 하면 오류없이 잘 돌아가는 것을 볼 수 있습니다.

test 상의 오류는 없는데, 서버를 키고 직접 확인해보니까 title이 안바뀝니다.

그래서 application.html에서 title을 주석처리하고 해보니 잘 작동하는 것을 볼 수 있었습니다.

어차피 나중에 잘 동작되게 만들거라서 상관없습니다.



자, 이제 반복되는 부분을 줄여봅시다.

*test/controllers/static_pages_controller_test.rb*

```ruby
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest

  def setup
    @base_title = "Ruby on Rails Tutorial Sample App"
  end    

  test "should get home" do
    get static_pages_home_url
    assert_response :success
    assert_select "title", "Home | #{@base_title}"
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
    assert_select "title", "Help | #{@base_title}"
  end
  
  test "should get about" do 
    get static_pages_about_url 
    assert_response :success
    assert_select "title", "About | #{@base_title}"
  end
end
```

위와 같이 코드를 바꿔줍시다. 뭐냐하면, base_title이라는 변수에 ruby on rails tutorial sample app 이라는 반복되는 부분을 넣어줍니다. 그리고 그 문장을 변수를 이용해서 다시 써준겁니다.

참고로 변수는 @variable 형태로 만들고, 이 변수를 쌍따옴표 안에서 사용할 때에는 #{@variable}과 같은 형태로 사용합니다. 그냥 @base_title 형태로는 사용이 불가능합니다. 루비에서 @는 instance variable 입니다.

자, 이제 rails test 와 서버를 켜서 확인해보면 그대로임을 확인 할 수 있죠.

*app/views/static_pages/home.html.erb*

```html
<% provide(:title, "Home") %>
<!DOCTYPE html>
<html> 
  <head>
    <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title> 
  </head>

  <body>
    <h1>Sample App</h1> 
    <p>
    This is the home page for the
    <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a> 
    sample application.
    </p> 
  </body>
</html>
```

위와같이 코드를 또 바꿔봅시다.

: 는 Symbol을 의미하는 것 인데,  이 심볼은 스트링과 비슷한 타입이고, 차이점은 메모리 관리가 효율적입니다.

:title 에 "Home" 이라는 스트링을 provide 함수로 넣고, 이 것을 title에 적용한 것 입니다.

저는 제일 헷갈렸던게, provide 함수에서 :title 이라는 변수안에 "Home" 을 넣는 줄 알고 착각했는데, :는 그저 심볼을 뜻하는 것이었습니다. :title 대신 "title"로 바꿔서 해봐도 똑같이 동작합니다.

```ruby
<% provide(:title, "Home") %>
```

이 코드는 title이라는 라벨이 붙은 Home 스트링이라고 생각하면 됩니다. 즉, title이라는 변수에 Home을 넣은것처럼 동작한다는 의미이죠. 변수는 아니지만 변수처럼 동작한다라고 이해하시면 편할 것 같습니다.

자 그리고, html.erb 파일에서 ruby 코드를 넣을 때 <% %> 를 이용해서 넣습니다. 그런데 이 코드는 실제로 view에 보여지지 않습니다. 만약 어떤 변수를 view에서 표현하고 싶다고 하면, <%= %>를 이용하면 됩니다. 그래서 위 코드에서 하나는 <% %>, 나머지 하나는 <%= %>라고 적어준 것이죠.

그리고 yield(:title)는 :title 이라는 라벨이 붙은 스트링을 나타내라! 라고 이해하시면 될 것 같습니다. 만약 그냥 :title로 사용하게되면 home 이 아니라 title 이 나오게 됩니다. 

또, yield(:title) 은 yield :title 로 괄호를 생략 할 수 있습니다.

about과 help도 마찬가지로 바꿔주면 되지만, 아직 더 줄일 수 있기 때문에 바로 다음 설명으로 넘어가겠습니다.

우리는 application.html.erb 파일이 나머지 뷰 파일의 조상이라는 점을 알고있어야 합니다. 

*app/views/layouts/application.html.erb*

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- <title>RailsApps</title> -->
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

원래 application.html.erb 파일이 이런식으로 되어있죠? 보시면 html의 head 부분이 있고, body 부분은 <%= yield %>로 생략되어있는 것이 보일 것 입니다. 이 생략된 부분에 바로 home, about, help의 view 파일이 들어갑니다.

그런데 home, help, about 파일을 살펴보면 head 와 body가 모두 들어가있네요? 그렇게 되면 body 안에 title이 들어가는 등 겹치게 됩니다. 이런것을 방지하기 위해서 다음과 같이 바꿔보겠습니다.

*app/views/static_pages/home.html.erb*

```html
<% provide(:title, "Home") %>
<h1>Sample App</h1> 
<p>
  This is the home page for the
  <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a> 
  sample application.
</p>
```

*app/views/static_pages/about.html.erb*

```html
<% provide(:title, "Help") %>
<h1>About</h1> 
<p>
  The <a href="https://www.railstutorial.org/"><em>Ruby on Rails Tutorial</em></a> is a
  <a href="https://www.railstutorial.org/book">book</a> and
  <a href="http://screencasts.railstutorial.org/">screencast series</a> to teach web development with
  <a href="http://rubyonrails.org/">Ruby on Rails</a>.
  This is the sample application for the tutorial. 
</p>
```

*app/views/static_pages/help.html.erb*

```html
<% provide(:title, "Help") %>
<h1>Help</h1> 
  <p>
  Get help on the Ruby on Rails Tutorial at the
  <a href="https://www.railstutorial.org/help">Rails Tutorial help page</a>.
  To get help on this sample app, see the
  <a href="https://www.railstutorial.org/book"><em>Ruby on Rails Tutorial</em> book</a>.
</p> 

```

자 그리고, application.html.erb 파일도 다음과 같이 바꿔줍니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

자 이렇게 하면, title이 home, about, help view 마다 그것에 맞는 title로 바뀌어서 나오겠죠?

rails test를 해봅시다.

오류없이 잘 돌아가네요. 이제 서버를 켜서 확인해봅시다.

실제로도 오류없이 잘 돌아갑니다.

자, 그다음에는 root 를 home 으로 바꿔봅시다. 

*config/routes.rb*

```ruby
Rails.application.routes.draw do
  root 'static_pages#home'
  get 'static_pages/home'
  get 'static_pages/help'
  get 'static_pages/about'
end
```

그 다음 서버를 켜봅시다. http://127.0.0.1:3000/ 으로 들어갔을 때 home 페이지가 제일 먼저 나옴을 확인 할 수 있습니다.

자 이제 첫번째 챕터가 끝났습니다.

다음 챕터에서 뵙겠습니다.















 







