# [The Elixir Style Guide][Elixir Style Guide]

**역주**:
[029d78a](https://github.com/levionessa/elixir_style_guide/blob/029d78a6f963e780f9b470da81dc7284a5ceb5ff/README.md)을
기준으로 번역했습니다.

### 목차

* __[Prelude](#prelude)__
* __[The Guide](#the-guide)__
    * [Source Code Layout](#source-code-layout)
    * [Syntax](#syntax)
    * [Naming](#naming)
    * [Comments](#comments)
        * [Comment Annotations](#comment-annotations)
    * [Modules](#modules)
    * [Documentation](#documentation)
    * [Typespecs](#typespecs)
    * [Exceptions](#exceptions)
    * _Collections_
    * [Strings](#strings)
    * _Regular Expressions_
    * [Metaprogramming](#metaprogramming)
    * [Suggested Alternatives](#suggested-alternatives)
    * _Tools_
* __[Getting Involved](#getting-involved)__
    * [Contributing](#contributing)
    * [Spread the Word](#spread-the-word)
* __[Copying](#copying)__
    * [License](#license)
    * [Attribution](#attribution)


## Prelude

> Liquid architecture. It's like jazz — you improvise, you work together, you
> play off each other, you make something, they make something. <br/>
> —Frank Gehry

스타일 중요합니다.
[Elixir]는 많은 스타일을 가지고 있지만, 다른 언어와 마찬가지로 무시할 수
있습니다.
스타일을 무시하지 마세요.

**주의**: PR을 던지고 그게 머지되면 자동으로 협업자에 추가됩니다. 추가를 원하지
않으시면 제출할 때 말씀해주세요.
PR이 머지되신 분은 협업자에 추가됩니다.


## The Guide

여기에 [Elixir 프로그레밍 언어][Elixir]의 커뮤니티 스타일 가이드를 만드려고
합니다.
편하게 풀리퀘스트를 던지거나 기여를 해주세요.
정말로 훨씬 긴 역사를 가진 다른 언어보다 더 활발한 커뮤니티를 엘릭서가 가졌으면
합니다.

혹시 다른 프로젝트에 기여하고 싶으시면, [Hex 패키지 관리자 사이트][Hex]를
참고하세요.


### Source Code Layout

<!-- TODO: Add crafty quote here -->

* 들여쓰기로 2개의 **공백**을 사용하세요.
  하드 텝을 사용하지 않습니다.

  ```elixir
  # 4 스페이스 - 권장하지 않음
  def some_function do
      do_something
  end

  # 권장함
  def some_function do
    do_something
  end
  ```

* 유닉스 스타일 개행을 사용합니다. (\*BSD/Solaris/Linux/OSX 사용자는 기본값으로
  커버됩니다만, Windows 사용자는 조심하실 필요가 있습니다.)

* Git을 사용하신다면, 다음 설정을 프로젝트에 추가해 윈도우 개행으로 부터
  프로젝트를 보호할 수 있습니다.

  ```sh
  $ git config --global core.autocrlf true
  ```

* 연산자 주위와 쉼표, 콜론, 세미콜론의 뒤에 공백을 넣으세요.
  중괄호나 대괄호같은 매치된 쌍주변에 공백을 넣지 않습니다.
  공백은 (대부분의 경우) Elixir 실행시에 문제되지 않지만, 읽을 수 있는 코드를
  작성하는데 중요하기에 권장됩니다.

  ```elixir
  sum = 1 + 2
  {a, b} = {2, 3}
  Enum.map(["one", <<"two">>, "three"], fn num -> IO.puts num end)
  ```

* `def`사이에 빈줄을 넣어 함수의 끝을 논리적인 단락으로 만드세요.

  ```elixir
  def some_function(some_data) do
    altered_data = Module.function(data)
  end

  def some_function do
    result
  end

  def some_other_function do
    another_result
  end

  def a_longer_function do
    one
    two

    three
    four
  end
  ```

* ...하지만 같은 함수의 한 줄 `def`는 붙여씁니다.

  ```elixir
  def some_function(nil), do: {:err, "No Value"}
  def some_function([]), do: :ok
  def some_function([first|rest]) do
    some_function(rest)
  end
  ```

* 함수 내용이 길어지는 곳에서 `do:` 구문을 사용한다면, 개행 한 후에
  들여쓰기를 한 번 더 하고 `do:`를 넣으세요.

```elixir
def some_function(args),
  do: Enum.map(args, fn(arg) -> arg <> " is on a very long line!" end)
```

한 개이상의 함수 클로져에 `do:` 구문을 사용하시고 위에 있는 관습을 사용하시려면,
각 함수 클로져 마다 개행을 한 다음 `do:`를 넣으세요.

```elixir
# 권장하지 않음
def some_function([]), do: :empty
def some_function(_),
  do: :very_long_line_here

# 권장함
def some_function([]),
  do: :empty
def some_function(_),
  do: :very_long_line_here
```

* 한 개 이상 여러 줄 `def`가 있다면 한 줄 `def`를 사용하지 마세요.

  ```elixir
  def some_function(nil) do
    {:err, "No Value"}
  end

  def some_function([]) do
    :ok
  end

  def some_function([first|rest]) do
    some_function(rest)
  end

  def some_function([first|rest], opts) do
    some_function(rest, opts)
  end
  ```

* 함수를 연결할 때 파이프라인 연산자(`|>`)를 사용하세요.

  ```elixir
  # 권장하지 않음
  String.strip(String.downcase(some_string))

  # 권장함
  some_string |> String.downcase |> String.strip

  # 여러줄 파이프라인의 경우 더 들여쓰기 하지 않음
  some_string
  |> String.downcase
  |> String.strip

  # 패턴 매칭의 오른 쪽 편의 여러줄 파이프라인은 반드시 새줄에 맞춰 개행 함
  sanitized_string =
    some_string
    |> String.downcase
    |> String.strip
  ```

  메서드에서는 이 방법이 권장되지만, 여러 줄 파이프라인을 IEx로 복사
  붙여놓기하면 문법 에러가 되는 것에 주의하세요. IEx는 다음 줄에 파이프라인이
  있다는 걸 눈치채지 못한 체 첫 줄을 평가합니다.

* 함수 체인의 시작은 _그냥_ 변수로 하세요.

  ```elixir
  # 최악!
  # 실제로는 String.strip("nope" |> String.downcase)로 해석됨
  String.strip "nope" |> String.downcase

  # 권장하지 않음
  String.strip(some_string) |> String.downcase |> String.codepoints

  # 권장함
  some_string |> String.strip |> String.downcase |> String.codepoints
  ```

* 줄 끝의 공백은 피하세요.


### Syntax

* 인자가 있을 때 괄호를 사용하고, 인자가 없다면 괄호를 사용하지 마세요.

  ```elixir
  # 권장하지 않음
  def some_function arg1, arg2 do
    # 내용 생략
  end

  def some_function() do
    # 내용 생략
  end

  # 권장함
  def some_function(arg1, arg2) do
    # 내용 생략
  end

  def some_function do
    # 내용 생략
  end
  ```

* 여러 줄 `if/unless`에 `do:`를 사용하지 마세요.

  ```elixir
  # 권장하지 않음
  if some_condition, do:
    # 코드 한줄
    # 코드 한줄 더
    # 주의: 이 블록에서 끝나지 않음

  # 권장함
  if some_condition do
    # 여러
    # 줄
    # 코드
  end
  ```

* 한 줄 `if/unless` 구문에 `do:`를 사용하세요.

  ```elixir
  # 권장함
  if some_condition, do: # 어떤 코드
  ```

* `unless`와 `else`를 같이 쓰지 마세요.
  긍적적인 경우가 먼저 오게 다시 작성 하세요.

  ```elixir
  # 권장하지 않음
  unless success? do
    IO.puts 'failure'
  else
    IO.puts 'success'
  end

  # 권장함
  if success? do
    IO.puts 'success'
  else
    IO.puts 'failure'
  end
  ```

* 항상 `cond` 구문의 마지막 조건으로 `true`를 사용하세요.

  ```elixir
  cond do
    1 + 2 == 5 ->
      "Nope"
    1 + 3 == 5 ->
      "Uh, uh"
    true ->
      "OK"
  end
  ```

* 함수 이름과 괄호의 시작 사이에 공백을 넣지 마세요.

  ```elixir
  # 권장하지 않음
  f (3 + 2) + 1

  # 권장함
  f(3 + 2) + 1
  ```

* 함수 호출에 괄호를 사용하세요. 특히 파이프라인의 안쪽에서는 더 필요합니다.

  ```elixir
  # 권장하지 않음
  f 3

  # 권장함
  f(3)

  # 권장하지 않고 아마 원하지 않을 rem(2, (3 |> g))로 해석됨
  2 |> rem 3 |> g

  # 권장함
  2 |> rem(3) |> g
  ```

* 메크로에서 do 블럭이 넘겨진 경우 괄호를 생략하세요.

  ```elixir
  # 권장하지 않음
  quote(do
    foo
  end)

  # 권장함
  quote do
    foo
  end
  ```

* 파이프라인의 밖에서는 마지막 인자가 함수라면 선택적으로 함수 호출에 괄호를
  생략 하세요.

  ```elixir
  # 권장함
  Enum.reduce(1..10, 0, fn x, acc ->
    x + acc
  end)

  # 역시 권장함
  Enum.reduce 1..10, 0, fn x, acc ->
    x + acc
  end
  ```

* 인자 갯 수가 0인 함수를 호출할 경우 괄호를 사용해 변수와 구분하세요.

  ```elixir
  defp do_stuff, do: ...

  # 권장하지 않음
  def my_func do
    do_stuff # 변수일 수도 함수 호출일 수도 있음
  end

  # 권장함
  def my_func do
    do_stuff() # 명백하게 함수 호출
  end
  ```


### Naming

* 에텀, 함수, 변수에 `snake_case`를 사용하세요.

  ```elixir
  # 권장하지 않음
  :"some atom"
  :SomeAtom
  :someAtom

  someVar = 5

  def someFunction do
    ...
  end

  def SomeFunction do
    ...
  end

  # 권장함
  :some_atom

  some_var = 5

  def some_function do
    ...
  end
  ```

* 모듈에 `CamelCase`를 사용하세요. (HTTP, RFC, XML 같은 약어는 대문자로
  유지합니다.)

  ```elixir
  # 권장하지 않음
  defmodule Somemodule do
    ...
  end

  defmodule Some_Module do
    ...
  end

  defmodule SomeXml do
    ...
  end

  # 권장함
  defmodule SomeModule do
    ...
  end

  defmodule SomeXML do
    ...
  end
  ```

* _가드 안에서 사용할 수 있는_ 선언적(predicate) 메크로(부울값을 반환하는 컴파일
  시 생성되는 함수)의 이름은 전치사로 `is_`로 시작해야 합니다.
  허용된 표현식의 목록은
  [Expressions in Guard Clauses](http://elixir-lang.org/getting-started/case-cond-and-if.html#expressions-in-guard-clauses)를
  확인하세요.

  ```elixir
  defmacro is_cool(var) do
    quote do: unquote(var) == "cool"
  end
  ```

* _가드 안에서 사용할 수 없는_ 선언적 함수의 이름은 `is_`나 그와 비슷한 전치사
  대신 끝에 물음표(`?`)를 붙여야 합니다.

  ```elixir
  def cool?(var) do
    # 순수 함수 안에서 사용할 수 없는 값이 cool인지 확인하는 복잡한 코드
  end
  ```

* 같은 이름의 public 함수가 있는 private 함수는 `do_`로 시작해야 합니다.

  ```elixir
  def sum(list), do: do_sum(list, 0)

  # private 함수
  defp do_sum([], total), do: total
  defp do_sum([head|tail], total), do: do_sum(tail, head + total)
  ```


### Comments

* 스스로 설명되는 코드를 적고 이 절을 무시하세요.
  진지합니다!

* 주석의 앞의 `#` 문자와 주석의 내용 사이에 공백 하나를 사용합니다.

* 한 단어보다 긴 주석은 대문자로 시작하고 문장 부호를 사용합니다.
  문단의 뒤에 [한 개의 공백](http://en.wikipedia.org/wiki/Sentence_spacing)을
  사용합니다.

  ```elixir
  # not preferred
  String.upcase(some_string) # Capitalize string.
  ```

#### Comment Annotations

* 어노테이션은 보통 해당 코드의 바로 위에 적어야 합니다.

* 어노테이션 키워드뒤에는 콜론과 한개의 공백을 넣고, 문제점을 적습니다.

* 문제를 설명하는데 여러 줄이 필요하면, 첫 줄을 재외하고 `#` 뒤에 두개의
  공백으로 들여쓰기를 합니다.

* 문제가 너무 뻔해 문서화가 필요 없을 때에는, 어노테이션을 해당 줄의 뒤에 달고
  내용을 생략합니다.
  이 사용법은 예외이며 룰의 일부가 아닙니다.

* `TODO`는 나중에 추가해야 하는 없는 기능을 설명할 때 사용합니다.

* `FIXME`는 고쳐야하는 망가진 코드를 설명할 때 사용합니다.

* `OPTIMIZE`는 성능 문제가 될 수 있는 느리거나 비효율적인 코드를 설명할 때
  사용합니다.

* `HACK`은 리팩터링해야 할 의문이 드는 코딩 관습이 사용된 코드 스멜을 설명할 때
  사용합니다.

* `REVIEW`는 의도대로 동작하는지 확인이 필요할 때 사용합니다.
  예: `REVIEW: 지금 이걸로 클라이언트가 X를 올바르게 수행할 수 있나요?`

* 필요하다고 생각할 때 다른 사용자 정의 어노테이션 키워드를 사용하세요.
  사용하셨으면 프로젝트의 `README`같은 곳에 문서화하세요.


### Modules

* 테스트 처럼 모듈이 다른 모듈안에서만 사용되지 않는 이상 파일당 한 모듈만
  사용하세요.

* `CamelCase` 모듈 이름에 대해 snake_case 파일 이름을 사용하세요.

  ```elixir
  # some_module.ex 파일

  defmodule SomeModule do
  end
  ```

* 모듈 이름 중첩의 각 단계로 디렉토리를 표현하세요.

  ```elixir
  # parser/core/xml_parser.ex 파일

  defmodule Parser.Core.XMLParser do
  end
  ```

* defmodule 뒤에 개행을 하지 않습니다.

* 첫번째 함수 def 앞에 개행을 하지 않습니다.

* "module-level-code-blocks" 뒤에 개행 하세요.

* 모듈 속성과 디렉티브는 다음 순서로 나열하세요.

    1. `@moduledoc`
    1. `@behaviour`
    1. `use`
    1. `import`
    1. `alias`
    1. `require`
    1. `@type`
    1. `@module_attribute`

  그리고 완벽주의자라면 각각 알파벳순으로 정렬하세요. 여기에 모듈안을 어떻게
  정렬해야 하는지 전반적으로 보여주는 예제가 있습니다.

  ```elixir
  defmodule MyModule do
    @moduledoc """
    An example module
    """

    @behaviour MyBehaviour

    use GenServer
    import Something
    import SomethingElse
    alias My.Long.Module.Name
    alias My.Other.Module.Name
    require Integer

    @type params :: [{binary, binary}]

    @module_attribute :foo
    @other_attribute 100

    ...
  end
  ```


### Documentation

Elixir에서 문서화는(`iex`에서 `h`로 읽거나
[ExDoc](https://github.com/elixir-lang/ex_doc)으로 생성할 때)
[Module Attributes](http://elixir-lang.org/getting-started/module-attributes.html#as-annotations)
`@moduledoc`과 `@doc`을 사용합니다.

* 항상 `@moduledoc` 속성을 모듈안의 `defmodule` 다음 줄에 넣으세요.

  ```elixir
  # 권장하지 않음

  defmodule SomeModule do

    @moduledoc """
    About the module
    """
    ...
  end

  defmodule AnotherModule do
    use SomeModule
    @moduledoc """
    About the module
    """
    ...
  end

  # 권장함

  defmodule SomeModule do
    @moduledoc """
    About the module
    """
    ...
  end
  ```

* 모듈을 문서화 할 생각이 없다면 `@moduledoc false`을 사용하세요.

  ```elixir
  defmodule SomeModule do
    @moduledoc false
    ...
  end
  ```

* `@moduledoc` 다음의 코드는 개행을 한 줄 넣어 구분하세요.

  ```elixir
  # 권장하지 않음

  defmodule SomeModule do
    @moduledoc """
    About the module
    """
    use AnotherModule
  end

  # 권장함
  defmodule SomeModule do
    @moduledoc """
    About the module
    """

    use AnotherModule
  end
  ```

* 문서화에 마크다운을 사용한 히어독을 사용하세요.

  ```elixir
  # 권장하지 않음

  defmodule SomeModule do
    @moduledoc "About the module"
  end

  defmodule SomeModule do
    @moduledoc """
    About the module

    Examples:
    iex> SomeModule.some_function
    :result
    """
  end

  # 권장함
  defmodule SomeModule do
    @moduledoc """
    About the module

    ## Examples

        iex> SomeModule.some_function
        :result
    """
  end
  ```


### Typespecs

타입 스팩은 문서화나 정적 분석기 Dialyzer를 위한 타입과 스팩을 정의하기 위한
표기법입니다.

사용자 정의 타입은 다른 디렉티브와 함께 모듈의 제일 위에 정의되어야 합니다.
([Modules](#modules)를 확인하세요.)

* `@typedoc`과 `@type` 정의를 함꼐 두고 각 쌍을 빈 줄로 구분하세요.

  ```elixir
  defmodule SomeModule do
    @moduledoc false

    @typedoc "The name"
    @type name :: atom

    @typedoc "The result"
    @type result :: {:ok, term} | {:error, term}

    ...
  end
  ```

* 유니온 타입이 한줄에 들어가기에는 너무 길다면, 개행을 하고 타입에 맞춰
  들여쓰기를 합니다.

  ```elixir
  # 권장하지 않음 - 들여쓰기 없음
  @type long_union_type :: some_type | another_type | some_other_type
  | a_final_type

  # 권장함
  @type long_union_type :: some_type | another_type | some_other_type
                         | a_final_type

  # 역시 권장함 - 타입 당 한번씩 개행
  @type long_union_type :: some_type
                         | another_type
                         | some_other_type
                         | a_final_type
  ```

* 스팩은 함수 선언의 직전에 개행으로 구분해 두세요.

  ```elixir
  @spec some_function(term) :: result
  def some_function(some_data) do
    {:ok, some_data}
  end
  ```


### Exceptions

* 예외 이름 뒤에 `Error`를 붙이세요.

  ```elixir
  # 권장하지 않음
  defmodule BadHTTPCode do
    defexception [:message]
  end

  defmodule BadHTTPCodeException do
    defexception [:message]
  end

  # 권장됨
  defmodule BadHTTPCodeError do
    defexception [:message]
  end
  ```

* 예외를 발생 시킬 때는 뒤에 문장 부호를 붙이지 않은 소문자로 시작하는 에러
  메세지를 사용하세요.

  ```elixir
  # 권장하지 않음
  raise ArgumentError, "This is not valid."

  # 권장됨
  raise ArgumentError, "this is not valid"
  ```


### Collections

_콜랙션에 관한 가이드라인은 아직 추가되지 않았습니다._


### Strings

* 문자열은 바이너리 패턴 보다 문자열 연결 연산자를 이용해 매칭하세요.

  ```elixir
  # 권장하지 않음
  <<"my"::utf8, _rest>> = "my string"

  # 권장됨
  "my" <> _rest = "my string"
  ```


### Regular Expressions

_정규 표현식에 관한 가이드라인은 아직 추가되지 않았습니다._


### Metaprogramming

* 불필요한 메타프로그래밍을 피하세요.


### Suggested Alternatives

대안 제시는 아직 커뮤니티에서 많이 보진 못했지만, 가치가 있을 겁니다.

#### Cond

* `cond` 안에서 에텀은 트루시(truthy) 값과 동일 하기 때문에 다 잡는
  표현식으로 사용할 수 있습니다.
  추천하는 에텀은 `:else`나 `:otherwise`입니다.

  ```elixir
  cond do
    1 + 2 == 5 ->
      "Nope"
    1 + 3 == 5 ->
      "Uh, uh"
    :else ->
      "OK"
  end

  # 이것과 동일
  cond do
    1 + 2 == 5 ->
      "Nope"
    1 + 3 == 5 ->
      "Uh, uh"
    true ->
      "OK"
  end
  ```


### Tools

_아직 툴은 추가되지 않았습니다._


## Getting Involved


### Contributing

It's our hope that this will become a central hub for community discussion on
best practices in Elixir.
Feel free to open tickets or send pull requests with improvements.
Thanks in advance for your help!


### Spread the Word

A community style guide is meaningless without the community's support. Please
tweet, [star](https://github.com/niftyn8/elixir_style_guide/stargazers), and let
any Elixir programmer know about [this guide][Elixir Style Guide] so they can
contribute.


## Copying


### License

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a
[Creative Commons Attribution 3.0 Unported License][license]


### Attribution

The structure of this guide, bits of example code, and many of the initial
points made in this document were borrowed from the [Ruby community style guide].
A lot of things were applicable to Elixir and allowed us to get _some_ document
out quicker to start the conversation.

Here's the
[list of people who has kindly contributed](https://github.com/niftyn8/elixir_style_guide/graphs/contributors)
to this project.

<!-- Links -->
[Elixir Style Guide]: https://github.com/niftyn8/elixir_style_guide
[Elixir]: http://elixir-lang.org
[Hex]: https://hex.pm/packages
[license]: http://creativecommons.org/licenses/by/3.0/deed.en_US
[Ruby community style guide]: https://github.com/bbatsov/ruby-style-guide
