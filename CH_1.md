# CH-1. 레일즈 시작하기



*이 문서는 Michael Hartl 의 The Ruby on Rails Tutorial 4판을 참고해서 작성하였습니다.*



1. 레일즈 시작하기

   Rails 와 Ruby 가 모두 깔려있다고 가정하고 시작하겠습니다.

   먼저 터미널에 Rails <버전> new <이름> 을 적어주세요.

   예)

   ```
   rails _6.0.2.1\_ new rails_app
   ```

   단, rails를 처음 시작할 때는 sqlite 기반이므로, 만약 postgresql 기반으로 하시고 싶으면

   ``` 
   rails _6.0.2.1\_ new rails_app --database=postgresql 
   ```

   커맨드라인에 위와같이 쳐주시면 됩니다.

   그 다음 db를 생성하고, 서버를 켜주고, 127.0.0.1:3000 로 들어가면, 레일즈 초기 화면이 보입니다.

   ```
   rake db:create
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
     <a href="https://www.railstutorial.org/help">Rails Tutorial help page</a>. To 	get help on this sample app, see the
     <a href="https://www.railstutorial.org/book"><em>Ruby on Rails Tutorial</em> 		book</a>.
   </p>
   ```



그 다음 127.0.0.1:3000/static_pages/home, 127.0.0.1:3000/static_pages/help 에 들어가보면 뷰가 나와있는 것을 볼 수 있습니다.

자 그다음은 TDD에 관한 내용입니다. 

TDD 는 test-driven development 의 줄임말입니다.

TDD는 말그대로 제대로 동작하는지 테스트 하는 것인데요, 직접 서버를 켜서 확인하지 않아도, rails test 라는 명령어를 통해 올바르게 작동하는지 확인할 수 있습니다. 

test 종류는 

 







