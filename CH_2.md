

# CH-2. **Rails-flavored Ruby**

저번 챕터에 title을 만들어 보았죠?

application.html.erb 파일에 다음과 같이 작성되어 있고,

```ruby
<%= yield(:title) %> | Ruby on Rails Tutorial Sample App
```

나머지 home, about, help 에는 다음과 같이 작성되어 있을 것 입니다. (밑의 예시는 home)

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

home, about, help의 공통점은 provide 함수를 통해서 title을 제공합니다. 그렇기 때문에 "Home | Ruby on Rails Tutorial Sample App" 와 같은 제목이 보이게 되죠.

그런데 웹페이지를 만들다보면, provide 함수를 통해서 title을 제공하지 않는 페이지도 분명 있을 것 입니다. 그런 페이지의 title은

" | Ruby on Rails Tutorial Sample App" 이렇게 나오게 되겠죠? 앞의 바가 걸리적 거립니다.

그래서 우리는 페이지 타이틀이 없을 경우를 대비해서 full_title 이라는 custom helper 를 만들 것 입니다.

여기서 full_title은 우리가 만들 헬퍼 이름입니다. 다른 이름으로 해도 상관 없습니다.

헬퍼 설계는 다음과 같습니다.

- 뷰의 provide 함수를 통해 제공되는 타이틀 이름이 없는 경우 "Ruby on Rails Tutorial Sample App" 를 base title로 한다.
- 만약 제공되는 타이틀 이름이 있는 경우 "타이틀 이름 | Ruby on Rails Tutorial Sample App" 로 한다.

이렇게 헬퍼를 만들어 보겠습니다.

helper 는 application helper, static_page helper 가 있을 텐데, title 관련한 부분은 applicaiton.html.erb 파일이 관여하므로 application_helper에서 만들도록 하겠습니다.

*app/helpers/application_helper.rb*

```ruby
module ApplicationHelper
  # Returns the full title on a per-page basis.
  def full_title(page_title = '')
    base_title = "Ruby on Rails Tutorial Sample App" 
    if page_title.empty?
      base_title
    else
      page_title + " | " + base_title 
    end
  end 
end
```

보면 위의 조건에 맞는 helper 가 완성됐죠? 아, 그리고 헷갈릴만한게 full_title(page_title = '') 이 부분인데, page_title 의 default 값을 '' 로 설정한다는 의미입니다.

root 인 Home 의 title이 provide 함수를 통해 제공되지 않게 코드를 바꾸고, test 파일도 바꿔보겠습니다.

*rails_app/app/views/static_pages/home.html.erb*

```ruby
<h1>Sample App</h1> 
<p>
  This is the home page for the
  <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a> 
  sample application.
</p>
```

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
    assert_select "title", "#{@base_title}"
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

그 다음 rails test 를 한번 해보죠. 지금 home 의 타이틀은 "Ruby on Rails Tutorial Sample App" 이렇게 나와야 합니다.

test 가 통과됐군요. 잘 됩니다.



그 다음에는 rails console 을 사용해서 string 과 method의 특징에 대해서 알아보죠.

```bash
$ rails console
>> 17 + 42 # Integer addition 
=> 59
```

숫자는 integer 로 인식해서 덧셈 연산이 됩니다.

```bash
$ rails console
>> "" # An empty string 
=> ""
>> "foo" # A nonempty string 
=> "foo"
```

string은 쌍따옴표를 양쪽에 붙여서 나타냅니다.

```bash
>> "foo" + "bar" # String concatenation 
=> "foobar"
```

string은 + 연산이 가능합니다.

```bash
>> first_name = "Michael" # Variable assignment 
=> "Michael"
>> "#{first_name} Hartl" # String interpolation 
=> "Michael Hartl" 
```

string 안에 변수를 사용하고 싶으면 "#{변수}" 와 같은 형식으로 사용합니다.

```bash
>> first_name = "Michael" 
=> "Michael"
>> last_name = "Hartl"
=> "Hartl"
>> first_name + " " + last_name # Concatenation, with a space in between
=> "Michael Hartl"
>> "#{first_name} #{last_name}" # The equivalent interpolation
=> "Michael Hartl"
```

위와 같이 여러가지 방법으로 concate 할 수 있습니다.

```bash
>> puts "foo" # put string 
foo
=> nil
>> print "foo" # print string without extra line 
foo=> nil
>> print "foo\n" # Same as puts "foo" 
foo
=> nil
```

puts는 뒤에 개행문자가 포함되어 문장끼리 띄어쓰기가 가능하지만, print는 문장뒤에 개행문자가 없다. 그렇기 때문에 문장을 띄어쓰려면 개행문자를 사용해야 한다.

레일즈에서는 쌍따옴표와 그냥 따옴표끼리 차이가 있습니다. 아래 예시를 보시죠.

```bash
>> '#{foo} bar' # Single-quoted strings don't allow interpolation 
=> "\#{foo} bar"
```

위에 쌍따옴표가 했던것처럼 변수를 사용할 수 가 없습니다. string 그 자체이죠.



















