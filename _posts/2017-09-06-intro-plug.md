---
title: elixir的plug入門教學 
date: 2017-09-06 06:32:00
tags: [ elixir, plug ]
---
先瞭解基本的 http
=================


要知道 elixir 的 plug 是幹麼的，可能要先知道 http 在幹麼的。這裡可以使用 chrome 的[inspector ](https://developer.chrome.com/devtools)可以來學習 http。用鄉民最喜歡的[ Ptt 網頁版 ](https://www.ptt.cc/index.html)來說明好了。如果你按 F12 就會看到 inspector，在按"Network"，在"index.html"裡，你就會看到 http 的 response header 了。以本頁來說，我們有 32 個 http request, request 有"status"和"method", 常見的 method 就有 GET, POST, PUT, DELETE。

<!--more-->
![Imgur](https://i.imgur.com/wU24abY.png)


![Imgur](https://i.imgur.com/nua99YT.png)

如果你在你的 terminal 裡輸入： `curl -X GET "https://www.ptt.cc/" -m 30 -v`


``` bash
  # request line
  > GET / HTTP/1.1

  # request header
  > Host: www.ptt.cc
  > User-Agent: curl/7.52.1
  > Accept: */*

  # request body
  ...

  #status line
  < HTTP/1.1 302 oved Temporarily

  #response header
  < Date: Wed, 06 Sep 2017 00:39:15 GMT
  < Content-Type: text/html
  < Transfer-Encoding: chunked
  < Connection: keep-alive
  < Set-Cookie: __cfduid=d899c202a6797f2894b0a1a7eccecd1081504658355; expires=Thu, 06-Sep-18 00:39:15 GMT; path=/; domain=.ptt.cc; HttpOnly
  < Location: https://www.ptt.cc/index.html
  < Alternate-Protocol: 443:npn-spdy/3
  < Strict-Transport-Security: max-age=0
  < X-Content-Type-Options: nosniff
  < Server: cloudflare-nginx
  < CF-RAY: 399d5ebfae8484c0-HKG

  # response body
  #...
```


Plug 家庭套餐
=============

我是按照官方 plug [ documentation ](https://hexdocs.pm/plug/readme.html) 的說明來架的。如果不想寫 code，可以到我的 [github](https://github.com/poligen/my_plug) 上來 git clone 下來玩。

Hello, Plug
-----------

如果進入我的 `my_plug~裡, ~mix deps.get`, `iex -S mix run`

到 `localhost:4000`, 你就可以看到, `Hello world` 。 太神奇啦！以下是最主要的： 斯斯有三種，但 plug 有兩種，哪兩種？ `Functional plugs` 和 `Module plugs`, 我們以下的例子是 module plugs, 會有一個 `call/2` function, 和一個 `init/1` function。 `init/1` 會在 compiling time 執行，而 `call/2` 會在 run time。(`call/2` 的功能像是 functional plug), functional plug 就是會接受 `connection` ，再返回一個新的 `connection` (因為 elixir 的 state 都是 immutable 的，你可以重新指定回本來的 `conn`) 那什麼是 `conn`? 我們晚點再細談，這裡你可以先想說，我們把 http 的 request 用 `Plug.Conn` 裡的 Elixir Struct，把 request 的 key-value 包進去[ %Plug.Conn{}](https://github.com/elixir-plug/plug/blob/v1.4.3/lib/plug/conn.ex#L3), 所以你在 `Phoenix Framework` 裡看到的 `conn` 大多數都可以想成是一個 %Plug.Conn{}的 struct。

``` elixir
  # in the /lib/my_plug.ex
  defmodule MyPlug do
    import Plug.Conn

    def init(options) do
      # initialize options

      options
    end

    def call(conn, _opts) do
      conn
      |> put_resp_content_type("text/plain")
      |> send_resp(200, "Hello world")
    end
  end
```

cowboy webserver
----------------

這裡我們沒有要搭建自己的 server, 也不是本文的重點，所以先把 webserver 交給[Cowboy](https://github.com/ninenines/cowboy), 一個 Erlang/OTP based 的 web server。Plug 內有 adapter 可以直接使用 cowboy. 我直接在 `Application`裡，用兩個 `children` 。一個來測試基本的 plug, 另外一個用來瞭解 Adapters

``` elixir
  def start(_type, _args) do
    # List all child processes to be supervised
    children = [
      Plug.Adapters.Cowboy.child_spec(:http, MyRouter, [], [port: 4001]),
      Plug.Adapters.Cowboy.child_spec(:http, MyPlug, [], [port: 4000])
      # Starts a worker by calling: MyPlug.Worker.start_link(arg)
      # {MyPlug.Worker, arg},
    ]

    # See https://hexdocs.pm/elixir/Supervisor.html
    # for other strategies and supported options
    opts = [strategy: :one_for_one, name: MyPlug.Supervisor]
    Supervisor.start_link(children, opts)
  end
```

Plug.Conn
---------

如果你打開 `Plug.Conn` 的 source code 來看的話，一開始就會定義這個 `__MODULE__` 的 struct, 就是我們常常在 `Phoenix` 裡看到的 `conn`：

``` elixir
    @type t :: %__MODULE__{
                adapter:         adapter,
                assigns:         assigns,
                before_send:     before_send,
                body_params:     params | Unfetched.t,
  #...
  }
```

這裡就是放我們 http 的 request/response 等。所以 `Plug.Conn` 裡有 `Request fields`, `Fetchable fiels`, `Response fields` 等…… 裡面還有一些常用的 function，我們在這個例子用的 `send_resp/1` 或是 `put_resp_content_type/3` 都是 `Plug.Conn` 裡的 function。

Plug.Builder
------------

### Pipeline

再講 `Plug.Router` 之前，要先說一下 `Plug.Builder`, 如果你把 plug 想成是樂高積木，或是串聯的插頭/座系列，你就可以用這個組合成你要 pipeline，譬如你想要對某一個 http request，做一系列的變化，最後生出你要的 `conn`, 裡面有一個 `plug` function, 可以吃 你定義好的 `plug function` 或是 `plug module` 譬如：

``` elixir
  defmodule MyApp do
    use Plug.Builder

    plug Plug.Logger                 # plug module
    plug :hello, upper: true         # plug function

    # A function from another module can be plugged too, provided it's
    # imported into the current module first.
    import AnotherModule, only: [interesting_plug: 2]
    plug :interesting_plug

    def hello(conn, opts) do
      body = if opts[:upper], do: "WORLD", else: "world"
      send_resp(conn, 200, body)
    end
  end
```

> Internally, Plug.Builder implements the Plug behaviour, which means both the init/1 and call/2 functions are defined.

`Plug.Builder` 有幫你定義好 init/1 和 call/2，所以可以不用自己再重寫一次。

譬如這個你自己蓋了一個 `:hello` function, 你就可以直接使用，在上述的例子裡，就是指：

> Multiple plugs can be defined with the plug/2 macro, forming a pipeline. The plugs in the pipeline will be executed in the order

好幾個 `plugs`，可以組合在一起，然後被按照順序執行。如果你常在 `Phoenix` 裡看到 `pipeline` function, 注意, 這是 `phoenix`的 DSL，不是 ~Plug` 的， `Plug` 裡沒有這個 function！

不過如果你讀 phoenix 的 source code，就可以看到 `pipeline/2` 有使用 `Plug.Builder` 的方式：

``` elixir
  defmacro pipeline(plug, do: block) do
    block =
      quote do
        plug = unquote(plug)
        @phoenix_pipeline []
        unquote(block)
      end

    compiler =
      quote unquote: false do
        Scope.pipeline(__MODULE__, plug)
        {conn, body} = Plug.Builder.compile(__ENV__, @phoenix_pipeline, [])
        def unquote(plug)(unquote(conn), _) do
          try do
            unquote(body)
          catch
            :error, reason ->
              Plug.Conn.WrapperError.reraise(unquote(conn), :error, reason)
          end
        end
        @phoenix_pipeline nil
      end

    quote do
      try do
        unquote(block)
        unquote(compiler)
      after
        :ok
      end
    end
  end
```

Plug.Router
-----------

> Each route needs to return a connection, as per the Plug spec. A catch-all match is recommended to be defined as in the example above, otherwise routing fails with a function clause error.

你可以把 `Plug.Router` 當成是一個 module plug, 所以在我們的例子裡面，你要在 `Application` 裡使用時，呼叫的方式是和其他 module plug 是一樣的。 `Plug.Adapters.Cowboy.child_spec(:http, MyRouter, [], [port: 4001])`

``` elixir
  defmodule MyRouter do
    use Plug.Router

    plug :match
    plug :dispatch

    get "/hello" do
      send_resp(conn, 200, "world")
    end

    forward "/users", to: "/"

    match _ do
      send_resp(conn, 404, "oops")
    end
  end
```

我們用 `match` 和 `dispatch` 加入 plug pipeline 裡讓我們使用，如果你去讀 Plug.Router 裡，其實 `get/post/patch/delete` 都是 `match/3` 的延伸，通過 `:via` 這個 options, 使用 `compile` 。可以讀一讀[Match](https://github.com/elixir-plug/plug/blob/3d48af2b97d58c183a7b8390abc42ac5367b0770/lib/plug/router.ex#L230) 這一段的 source code。

如果有興趣在深入，想要自己蓋個簡單版的 web server, 而不用 plug，可以看看 Ole Michaelis 在 2017 ElixirConf.EU 的演講 [ Just Enough Plug to Build a Web server ](https://youtu.be/ESzZCaaCzyg)。
