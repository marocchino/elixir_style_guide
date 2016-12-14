# [The Elixir Style Guide][Elixir Style Guide]

**역주**:
[7418d2a](https://github.com/christopheradams/elixir_style_guide/blob/7418d2a6535f89b8b062f3be80416567516b7b91/README.md)을
기준으로 번역했습니다.

## 목차

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
  * [Structs](#structs)
  * [Exceptions](#exceptions)
  * _Collections_
  * [Strings](#strings)
  * _Regular Expressions_
  * [Metaprogramming](#metaprogramming)
  * [Testing](#testing)
  * [Suggested Alternatives](#suggested-alternatives)
  * [Style Guides](#style-guides)
  * [Tools](#tools)
* __[Getting Involved](#getting-involved)__
  * [Contributing](#contributing)
  * [Spread the Word](#spread-the-word)
* __[Copying](#copying)__
  * [License](#license)
  * [Attribution](#attribution)

## Prelude

> Liquid architecture. It's like jazz — you improvise, you work together, you
> play off each other, you make something, they make something.
>
> —Frank Gehry

스타일은 중요합니다.
[Elixir]는 많은 스타일을 가지고 있지만, 다른 언어와 마찬가지로 무시할 수
있습니다.
스타일을 무시하지 마세요.

## The Guide

여기에 [Elixir 프로그래밍 언어][Elixir]의 커뮤니티 스타일 가이드를 만들려고
합니다.
편하게 풀 리퀘스트를 던지거나 기여를 해주세요.
정말로 훨씬 긴 역사를 가진 다른 언어보다 더 활발한 커뮤니티를 Elixir가 가졌으면
합니다.

혹시 다른 프로젝트에 기여하고 싶으시면, [Hex 패키지 관리자 사이트][Hex]를
참고하세요.

### Source Code Layout

<!-- TODO: Add crafty quote here -->

* <a name="spaces-indentation"></a>
  들여쓰기로 2개의 **공백**을 사용하세요.
  하드 탭을 사용하지 않습니다.
  <sup>[[link](#spaces-indentation)]</sup>

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

* <a name="line-endings"></a>
  유닉스 스타일 개행을 사용합니다. (\*BSD/Solaris/Linux/OSX 사용자는 기본값으로
  커버됩니다만, Windows 사용자는 조심하실 필요가 있습니다.)
  <sup>[[link](#line-endings)]</sup>

* <a name="autocrlf"></a>
  Git을 사용하신다면, 다음 설정을 프로젝트에 추가해 Windows 개행으로부터
  프로젝트를 보호할 수 있습니다.
  <sup>[[link](#autocrlf)]</sup>

  ```sh
  git config --global core.autocrlf true
  ```

* <a name="spaces"></a>
  연산자 주위와 쉼표, 콜론, 세미콜론의 뒤에 공백을 넣으세요.
  중괄호나 대괄호같은 매치된 쌍 주변에는 공백을 넣지 않습니다.
  공백은 (대부분의 경우) Elixir 실행 시에 문제 되지 않지만, 읽을 수 있는 코드를
  작성하는데 중요하기에 권장됩니다.
  <sup>[[link](#spaces)]</sup>

  ```elixir
  sum = 1 + 2
  {a, b} = {2, 3}
  [first | rest] = [1, 2, 3]
  Enum.map(["one", <<"two">>, "three"], fn num -> IO.puts num end)
  ```

* <a name="no-spaces"></a>
  하나의 인자를 가지는 단어가 아닌 연산자 뒤에 스페이스를 사용하지 마세요.
  레인지 연산자에도 사용하지 마세요.
  <sup>[[link](#no-spaces)]</sup>

  ```elixir
  0 - 1 == -1
  ^pinned = some_func()
  5 in 1..10
  ```

* <a name="def-spacing"></a>
  `def`사이에 빈 줄을 넣어 함수의 끝을 논리적인 단락으로 만드세요.
  <sup>[[link](#def-spacing)]</sup>

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

* <a name="single-line-defs"></a>
  ...하지만 같은 함수의 한 줄 `def`는 붙여 씁니다.
  <sup>[[link](#single-line-defs)]</sup>

  ```elixir
  def some_function(nil), do: {:err, "No Value"}
  def some_function([]), do: :ok
  def some_function([first | rest]) do
    some_function(rest)
  end
  ```

* <a name="long-dos"></a>
  함수 내용이 길어지는 곳에서 `do:` 구문을 사용한다면, 개행 한 후에
  들여 쓰기를 한 번 더 하고 `do:`를 넣으세요.
  <sup>[[link](#long-dos)]</sup>

  ```elixir
  def some_function(args),
    do: Enum.map(args, fn(arg) -> arg <> " is on a very long line!" end)
  ```

  한 개 이상의 함수 절에 `do:` 구문을 사용하실 때 위에 있는 관습을 사용하시려면,
  각 함수 절마다 개행을 한 다음 `do:`를 넣으세요.

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

* <a name="multiple-function-defs"></a>
  여러 줄을 사용하는 `def`가 한 개 이상 있다면 한 줄 `def`를 사용하지 마세요.
  <sup>[[link](#multiple-function-defs)]</sup>

  ```elixir
  def some_function(nil) do
    {:err, "No Value"}
  end

  def some_function([]) do
    :ok
  end

  def some_function([first | rest]) do
    some_function(rest)
  end

  def some_function([first | rest], opts) do
    some_function(rest, opts)
  end
  ```

* <a name="pipe-operator"></a>
  함수를 연결할 때 파이프라인 연산자(`|>`)를 사용하세요.
  <sup>[[link](#pipe-operator)]</sup>

  ```elixir
  # 권장하지 않음
  String.strip(String.downcase(some_string))

  # 권장함
  some_string |> String.downcase |> String.strip

  # 여러줄 파이프라인의 경우 더 들여 쓰기 하지 않음
  some_string
  |> String.downcase
  |> String.strip

  # 패턴 매칭의 오른쪽 편의 여러 줄 파이프라인은 반드시 새 줄에 맞춰 개행함
  sanitized_string =
    some_string
    |> String.downcase
    |> String.strip
  ```

  메서드에서는 이 방법이 권장되지만, 여러 줄 파이프라인을 IEx로 복사
  붙여넣기하면 문법 에러가 되는 것에 주의하세요. IEx는 다음 줄에 파이프라인이
  있다는 걸 눈치채지 못한 채 첫 줄을 평가합니다.

* <a name="avoid-single-pipelines"></a>
  파이프 연산자를 한번만 쓰는 것을 피하세요.
  <sup>[[link](#avoid-single-pipelines)]</sup>

  ```elixir
  # 권장하지 않음
  some_string |> String.downcase

  # 권장함
  String.downcase(some_string)
  ```

* <a name="bare-variables"></a>
  함수 체인의 시작은 _그냥_ 변수로 하세요.
  <sup>[[link](#bare-variables)]</sup>

  ```elixir
  # 최악!
  # 실제로는 String.strip("nope" |> String.downcase)로 해석됨
  String.strip "nope" |> String.downcase

  # 권장하지 않음
  String.strip(some_string) |> String.downcase |> String.codepoints

  # 권장함
  some_string |> String.strip |> String.downcase |> String.codepoints
  ```

* <a name="trailing-whitespace"></a>
  줄 끝의 공백은 피하세요.
  <sup>[[link](#trailing-whitespace)]</sup>

* <a name="newline-eof"></a>
  파일의 끝에 개행을 넣으세요.
  <sup>[[link](#newline-eof)]</sup>



### Syntax

* <a name="parentheses"></a>
  `def`에 인자가 있을 때 괄호를 사용하고, 인자가 없다면 괄호를 사용하지 마세요.
  <sup>[[link](#parentheses)]</sup>

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

* <a name="do-with-multi-line-if-unless"></a>
  여러 줄 `if/unless`에 `do:`를 사용하지 마세요.
  <sup>[[link](#do-with-multi-line-if-unless)]</sup>

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

* <a name="do-with-single-line-if-unless"></a>
  한 줄 `if/unless` 구문에 `do:`를 사용하세요.
  <sup>[[link](#do-with-single-line-if-unless)]</sup>

  ```elixir
  # 권장함
  if some_condition, do: # 어떤 코드
  ```

* <a name="unless-with-else"></a>
  `unless`와 `else`를 같이 쓰지 마세요.
  긍정적인 경우가 먼저 오게 다시 작성하세요.
  <sup>[[link](#unless-with-else)]</sup>

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

* <a name="true-as-last-condition"></a>
  항상 `cond` 구문의 마지막 조건으로 `true`를 사용하세요.
  <sup>[[link](#true-as-last-condition)]</sup>

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

* <a name="function-names-with-parentheses"></a>
  함수 이름과 괄호의 시작 사이에 공백을 넣지 마세요.
  <sup>[[link](#function-names-with-parentheses)]</sup>

  ```elixir
  # 권장하지 않음
  f (3 + 2) + 1

  # 권장함
  f(3 + 2) + 1
  ```

* <a name="function-calls-and-parentheses"></a>
  함수 호출에 괄호를 사용하세요. 특히 파이프라인의 안쪽에서는 더 필요합니다.
  <sup>[[link](#function-calls-and-parentheses)]</sup>

  ```elixir
  # 권장하지 않음
  f 3

  # 권장함
  f(3)

  # 권장하지 않고 아마 원하지 않았을 rem(2, (3 |> g))로 해석됨
  2 |> rem 3 |> g

  # 권장함
  2 |> rem(3) |> g
  ```

* <a name="macro-calls-and-parentheses"></a>
  매크로에서 do 블록이 넘겨진 경우 괄호를 생략하세요.
  <sup>[[link](#macro-calls-and-parentheses)]</sup>

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

* <a name="parentheses-and-function-expressions"></a>
  파이프라인의 밖에서는 마지막 인자가 함수라면 선택적으로 함수 호출에 괄호를
  생략하세요.
  <sup>[[link](#parentheses-and-function-expressions)]</sup>

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

* <a name="parentheses-and-functions-with-zero-arity"></a>
  인자 개수가 0인 함수를 호출할 경우 괄호를 사용해 변수와 구분하세요.
  <sup>[[link](#parentheses-and-functions-with-zero-arity)]</sup>

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

* <a name="with-clauses"></a>
  `with` 절 안의 구문을 들여쓰기하고 정렬하세요.
  `do:` 인자는 새 줄에 평범하게 들여쓰게 하세요.
  <sup>[[link](#with-clauses)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, bar} <- fetch(opts, :bar),
    do: {:ok, foo, bar}
  ```

* <a name="with-else"></a>
  `with` 표현식이 두 줄이상의 `do` 블록을 가지거나 `else` 옵션을 가진다면,
  여러 줄 구문을 사용하세요.
  <sup>[[link](#with-else)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, bar} <- fetch(opts, :bar) do
    {:ok, foo, bar}
  else
    :error ->
      {:error, :bad_arg}
  end
  ```

### Naming

* <a name="snake-case"></a>
  애텀, 함수, 변수에 `snake_case`를 사용하세요.
  <sup>[[link](#snake-case)]</sup>

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

* <a name="camel-case"></a>
  모듈에 `CamelCase`를 사용하세요. (HTTP, RFC, XML 같은 약어는 대문자로
  유지합니다.)
  <sup>[[link](#camel-case)]</sup>

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

* <a name="predicate-macro-names-with-guards"></a>
  _가드 안에서 사용할 수 있는_ 선언적(predicate) 매크로(부울값을 반환하는 컴파일
  시 생성되는 함수)의 이름은 접두사 `is_`로 시작해야 합니다.
  허용된 표현식의 목록은 [Guard][Guard Expressions] 문서를 확인하세요.
  <sup>[[link](#predicate-macro-names-with-guards)]</sup>

  ```elixir
  defmacro is_cool(var) do
    quote do: unquote(var) == "cool"
  end
  ```

* <a name="predicate-macro-names-no-guards"></a>
  _가드 안에서 사용할 수 없는_ 선언적 함수의 이름은 `is_`나 그와 비슷한 접두사
  대신 끝에 물음표(`?`)를 붙여야 합니다.
  <sup>[[link](#predicate-macro-names-no-guards)]</sup>

  ```elixir
  def cool?(var) do
    # 순수 함수 안에서 사용할 수 없는 값이 cool인지 확인하는 복잡한 코드
  end
  ```

* <a name="private-functions-with-same-name-as-public"></a>
  같은 이름의 public 함수가 있는 private 함수는 `do_`로 시작해야 합니다.
  <sup>[[link](#private-functions-with-same-name-as-public)]</sup>

  ```elixir
  def sum(list), do: do_sum(list, 0)

  # private 함수
  defp do_sum([], total), do: total
  defp do_sum([head | tail], total), do: do_sum(tail, head + total)
  ```

### Comments

* <a name="expressive-code"></a>
  스스로 설명할 수 있는 코드를 작성하고 제어 흐름, 구조 및 이름 지정을 통해
  프로그램의 의도를 전달하십시오.
  <sup>[[link](#expressive-code)]</sup>

* <a name="comment-leading-spaces"></a>
  주석의 앞의 `#` 문자와 주석의 내용 사이에 공백 하나를 사용합니다.
  <sup>[[link](#comment-leading-spaces)]</sup>

* <a name="comment-spacing"></a>
  한 단어보다 긴 주석은 대문자로 시작하고 문장 부호를 사용합니다.
  문단의 뒤에 [공백 하나][Sentence Spacing]를 사용합니다.
  <sup>[[link](#comment-spacing)]</sup>

  ```elixir
  # not preferred
  String.upcase(some_string) # Capitalize string.
  ```

#### Comment Annotations

* <a name="annotations"></a>
  어노테이션은 보통 해당 코드의 바로 위에 적어야 합니다.
  <sup>[[link](#annotations)]</sup>

* <a name="annotation-keyword"></a>
  어노테이션 키워드 뒤에는 콜론과 한 개의 공백을 넣고 문제점을 적습니다.
  <sup>[[link](#annotation-keyword)]</sup>

* <a name="multiple-line-annotations"></a>
  문제를 설명하는 데 여러 줄이 필요하면 첫 줄을 제외하고 `#` 뒤에 두 개의
  공백으로 들여쓰기를 합니다.
  <sup>[[link](#multiple-line-annotations)]</sup>

* <a name="exceptions-to-annotations"></a>
  문제가 너무 뻔해 문서화가 필요 없을 때에는, 어노테이션을 해당 줄의 뒤에 달고
  내용을 생략합니다.
  이 사용법은 예외이며 룰의 일부가 아닙니다.
  <sup>[[link](#exceptions-to-annotations)]</sup>

* <a name="todo-notes"></a>
  `TODO`는 나중에 추가해야 하는 없는 기능을 설명할 때 사용합니다.
  <sup>[[link](#todo-notes)]</sup>

* <a name="fixme-notes"></a>
  `FIXME`는 고쳐야 하는 망가진 코드를 설명할 때 사용합니다.
  <sup>[[link](#fixme-notes)]</sup>

* <a name="optimize-notes"></a>
  `OPTIMIZE`는 성능 문제가 될 수 있는 느리거나 비효율적인 코드를 설명할 때
  사용합니다.
  <sup>[[link](#optimize-notes)]</sup>

* <a name="hack-notes"></a>
  `HACK`은 리팩터링해야 할 의문이 드는 코딩 관습이 사용된 코드 스멜을 설명할 때
  사용합니다.
  <sup>[[link](#hack-notes)]</sup>

* <a name="review-notes"></a>
  `REVIEW`는 의도대로 동작하는지 확인이 필요할 때 사용합니다.
  예: `REVIEW: 지금 이걸로 클라이언트가 X를 올바르게 수행할 수 있나요?`
  <sup>[[link](#review-notes)]</sup>

* <a name="custom-keywords"></a>
  필요하다고 생각할 때 다른 사용자 정의 어노테이션 키워드를 사용하세요.
  사용하셨으면 프로젝트의 `README`같은 곳에 문서화하세요.
  <sup>[[link](#custom-keywords)]</sup>

### Modules

* <a name="one-module-per-file"></a>
  테스트처럼 모듈이 다른 모듈 안에서만 사용되지 않는 이상 파일당 한 모듈만
  사용하세요.
  <sup>[[link](#one-module-per-file)]</sup>

* <a name="underscored-filenames"></a>
  `CamelCase` 모듈 이름에 대해 snake_case 파일 이름을 사용하세요.
  <sup>[[link](#underscored-filenames)]</sup>

  ```elixir
  # some_module.ex 파일

  defmodule SomeModule do
  end
  ```

* <a name="module-name-nesting"></a>
  모듈 이름 중첩의 각 단계로 디렉터리를 표현하세요.
  <sup>[[link](#module-name-nesting)]</sup>

  ```elixir
  # parser/core/xml_parser.ex 파일

  defmodule Parser.Core.XMLParser do
  end
  ```

* <a name="defmodule-spacing"></a>
  `defmodule` 뒤에 빈줄을 넣지 않습니다.
  <sup>[[link](#defmodule-spacing)]</sup>

* <a name="module-block-spacing"></a>
  module 수준 코드 블록 뒤에 빈줄을 넣으세요.
  <sup>[[link](#module-block-spacing)]</sup>

* <a name="module-attribute-ordering"></a>
  모듈 속성과 디렉티브는 다음 순서로 나열하세요.
  <sup>[[link](#module-attribute-ordering)]</sup>

  1. `@moduledoc`
  1. `@behaviour`
  1. `use`
  1. `import`
  1. `alias`
  1. `require`
  1. `defstruct`
  1. `@type`
  1. `@module_attribute`

  그리고 각 그룹 사이에 빈줄을 넣고 (모듈 이름같은) 용어를 알파벳순으로
  정렬하세요.
  여기에 모듈 안을 어떻게 정렬해야 하는지 전반적으로 보여주는 예제가 있습니다.

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

    defstruct name: nil, params: []

    @type params :: [{binary, binary}]

    @module_attribute :foo
    @other_attribute 100

    ...
  end
  ```

* <a name="module-pseudo-variable"></a>
  모듈에서 자신을 참조할 때 `__MODULE__` 수도 변수를 사용하세요. 이렇게하면
  모듈 이름이 바뀌었을 때 자신을 참조한 부분의 갱신을 피할 수 있습니다.
  <sup>[[link](#module-pseudo-variable)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    defstruct [:name]

    def name(%__MODULE__{name: name}), do: name
  end
  ```

* <a name="alias-self-referencing-modules"></a>
  모듈의 자기 참조에 읽기 편한 이름을 사용하고 싶다면, 알리아스를 사용하세요.
  <sup>[[link](#alias-self-referencing-modules)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    alias __MODULE__, as: SomeModule

    defstruct [:name]

    def name(%SomeModule{name: name}), do: name
  end
  ```


### Documentation

Elixir에서 문서화는(`iex`에서 `h`로 읽거나 [ExDoc]으로 생성할 때)
[Module Attributes] `@moduledoc`과 `@doc`을 사용합니다.

* <a name="moduledocs"></a>
  항상 `@moduledoc` 속성을 모듈 안의 `defmodule` 다음 줄에 넣으세요.
  <sup>[[link](#moduledocs)]</sup>

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

* <a name="moduledoc-false"></a>
  모듈을 문서화할 생각이 없다면 `@moduledoc false`을 사용하세요.
  <sup>[[link](#moduledoc-false)]</sup>

  ```elixir
  defmodule SomeModule do
    @moduledoc false
    ...
  end
  ```

* <a name="moduledoc-spacing"></a>
  `@moduledoc` 다음의 코드는 개행을 한 줄 넣어 구분하세요.
  <sup>[[link](#moduledoc-spacing)]</sup>

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

* <a name="heredocs"></a>
  문서화에 마크다운을 사용한 히어독을 사용하세요.
  <sup>[[link](#heredocs)]</sup>

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

타입 스펙은 문서화나 정적 분석기 Dialyzer를 위한 타입과 스펙을 정의하기 위한
표기법입니다.

사용자 정의 타입은 다른 디렉티브와 함께 모듈의 제일 위에 정의되어야 합니다.
([Modules](#modules)를 확인하세요.)

* <a name="typedocs"></a>
  `@typedoc`과 `@type` 정의를 함께 두고 각 쌍을 빈 줄로 구분하세요.
  <sup>[[link](#typedocs)]</sup>

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

* <a name="union-types"></a>
  유니온 타입이 한 줄에 들어가기에는 너무 길다면, 개행을 하고 타입에 맞춰
  들여 쓰기를 합니다.
  <sup>[[link](#union-types)]</sup>

  ```elixir
  # 권장하지 않음 - 들여 쓰기 없음
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

* <a name="naming-main-types"></a>
  모듈의 메인 타입 이름은 `t`로 합니다. 예를 들어, 구조체를 위한 타입 사양은
  이런식으로 할 수 있습니다.
  <sup>[[link](#naming-main-types)]</sup>

  ```elixir
  defstruct name: nil, params: []

  @type t :: %__MODULE__{
    name: String.t,
    params: Keyword.t
  }
  ```

* <a name="spec-spacing"></a>
  스펙은 함수 선언의 직전에 빈줄을 사이에 넣지 않고 넣습니다.
  <sup>[[link](#spec-spacing)]</sup>

  ```elixir
  @spec some_function(term) :: result
  def some_function(some_data) do
    {:ok, some_data}
  end
  ```

### Structs

* <a name="nil-struct-field-defaults"></a>
  모든 구조체 필드의 기본값이 nil이라면 애텀의 리스트로 필드를 넣습니다.
  <sup>[[link](#nil-struct-field-defaults)]</sup>

  ```elixir
  # 권장하지 않음
  defstruct name: nil, params: nil

  # 권장함
  defstruct [:name, :params]
  ```

* <a name="additional-struct-def-lines"></a>
  구조체 선언이 한 줄을 넘어 갈 때, 들여쓰기는 첫 번째 키에 맞추세요.
  <sup>[[link](#additional-struct-def-lines)]</sup>

  ```elixir
  defstruct foo: "test", bar: true, baz: false,
            qux: false, quux: nil
  ```


### Exceptions

* <a name="exception-names"></a>
  예외 이름 뒤에 `Error`를 붙이세요.
  <sup>[[link](#exception-names)]</sup>

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

* <a name="lowercase-error-messages"></a>
  예외를 발생시킬 때는 뒤에 문장 부호를 붙이지 않은 소문자로 시작하는 에러
  메세지를 사용하세요.
  <sup>[[link](#lowercase-error-messages)]</sup>

  ```elixir
  # 권장하지 않음
  raise ArgumentError, "This is not valid."

  # 권장됨
  raise ArgumentError, "this is not valid"
  ```

### Collections

_콜렉션에 관한 가이드라인은 아직 추가되지 않았습니다._

### Strings

* <a name="strings-matching-with-concatenator"></a>
  문자열은 바이너리 패턴보다 문자열 연결 연산자를 이용해 매칭하세요.
  <sup>[[link](#strings-matching-with-concatenator)]</sup>

  ```elixir
  # 권장하지 않음
  <<"my"::utf8, _rest>> = "my string"

  # 권장됨
  "my" <> _rest = "my string"
  ```

### Regular Expressions

_정규 표현식에 관한 가이드라인은 아직 추가되지 않았습니다._

### Metaprogramming

* <a name="avoid-metaprogramming"></a>
  불필요한 메타 프로그래밍을 피하세요.
  <sup>[[link](#avoid-metaprogramming)]</sup>

### Testing

* <a name="testing-assert-order"></a>
  [ExUnit]의 단언문을 적을 때에는, 테스트 안의 expected와 actual의 순서에
  일관성을 유지하세요.
  단언문이 패턴 매칭이 아니라면, expected 결과를 오른쪽에 두는 것을 권장합니다.
  <sup>[[link](#testing-assert-order)]</sup>

  ```elixir
  # preferred - expected result on the right
  assert actual_function(1) == true
  assert actual_function(2) == false

  # not preferred - inconsistent order
  assert actual_function(1) == true
  assert false == actual_function(2)

  # required - the assertion is a pattern match
  assert {:ok, expected} = actual_function(3)
  ```

### Suggested Alternatives

제시된 대안은 아직 커뮤니티에서 많이 보이진 않지만
가치가 있을 스타일들입니다.

#### Cond

* <a name="atom-conditions"></a>
  `cond` 안에서 애텀은 트루시(truthy) 값과 동일하기 때문에 다 잡는
  표현식으로 사용할 수 있습니다.
  추천하는 애텀은 `:else`나 `:otherwise`입니다.
  <sup>[[link](#atom-conditions)]</sup>

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

### Style Guides

Check [Awesome Elixir][Style Guides] for a list of alternative style guides.

### Tools

Refer to [Awesome Elixir][Code Analysis] for libraries and tools that can help
with code analysis and style linting.

## Getting Involved

### Contributing

It's our hope that this will become a central hub for community discussion on
best practices in Elixir.
Feel free to open tickets or send pull requests with improvements.
Thanks in advance for your help!

Check the [contributing guidelines](CONTRIBUTING.md)
and [code of conduct](CODE_OF_CONDUCT.md) for more information.

### Spread the Word

A community style guide is meaningless without the community's support. Please
tweet, [star][Stargazers], and let any Elixir programmer know
about [this guide][Elixir Style Guide] so they can contribute.

## Copying

### License

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a
[Creative Commons Attribution 3.0 Unported License][License]

### Attribution

The structure of this guide, bits of example code, and many of the initial
points made in this document were borrowed from the [Ruby community style guide].
A lot of things were applicable to Elixir and allowed us to get _some_ document
out quicker to start the conversation.

Here's the [list of people who has kindly contributed][Contributors] to this
project.

<!-- Links -->
[Code Analysis]: https://github.com/h4cc/awesome-elixir#code-analysis
[Contributors]: https://github.com/christopheradams/elixir_style_guide/graphs/contributors
[Elixir Style Guide]: https://github.com/christopheradams/elixir_style_guide
[Elixir]: http://elixir-lang.org
[ExDoc]: https://github.com/elixir-lang/ex_doc
[ExUnit]: https://hexdocs.pm/ex_unit/ExUnit.html
[Guard Expressions]: http://elixir-lang.org/getting-started/case-cond-and-if.html#expressions-in-guard-clauses
[Hex]: https://hex.pm/packages
[License]: http://creativecommons.org/licenses/by/3.0/deed.en_US
[Module Attributes]: http://elixir-lang.org/getting-started/module-attributes.html#as-annotations
[Ruby community style guide]: https://github.com/bbatsov/ruby-style-guide
[Sentence Spacing]: http://en.wikipedia.org/wiki/Sentence_spacing
[Stargazers]: https://github.com/christopheradams/elixir_style_guide/stargazers
[Style Guides]: https://github.com/h4cc/awesome-elixir#styleguides
