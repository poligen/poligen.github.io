---
title: elixir的ecto入門教學 
date: 2017-09-04 09:41:54
tags: [ elixir, ecto ]
---

因為 elixir 的資料庫 ecto 的中文教學實在是太少了。（根本沒有…）。自己也正好要學習，順手寫了這個教學文。但這教學文也不是我原創的。看了幾個教學，感覺[ Geoffrey Lessel](https://twitter.com/geolessel)的最好，這是 Lonstar ElixirConf 2017, 的一個四十分鐘的[演講](https://youtu.be/PWJpSRUJDwk)(Using Ecto Outside of phoenix)，練習上，我也覺得直接用 terminal, 不需要和 phoenix 框架一起練習。有一些教學，也喜歡把 ecto 這個 DSL 另外放在外頭，不過這是設計上的後話了。這裡沒有 elixir 的教學，有一些東西，我省略沒有打。之後有機會也可以再來寫的 elixir 的教學。建議可以一邊看影片，一邊實做。我把他的程式碼和實做的過程都記錄在這裡：

我覺得開始前也可以把`hex.pm` 上，ecto的[ documentation ](https://hexdocs.pm/ecto/Ecto.html)看一下：第一段最重要。Ecto四要素：Repo, Schema, Changeset, Query
<!--more-->

建立新的 repo
=============

``` elixir
  mix new todo --sup
```

``` elixir
  # in todo/mix.exs
    defp deps do
      [
        {:ecto, "~>2.1"},
        {:postgrex, "~>0.13"}
      ]
    end
```

``` bash
  mix deps.get
  mix compile
```

如果你輸入 `mix hep |grep ecto`, 你就會看到 ecto 常用的功能：

``` elixir
  mix ecto               # Prints Ecto help information
  mix ecto.create        # Creates the repository storage
  mix ecto.drop          # Drops the repository storage
  mix ecto.dump          # Dumps the repository database structure
  mix ecto.gen.migration # Generates a new migration for the repo
  mix ecto.gen.repo      # Generates a new repository
  mix ecto.load          # Loads previously dumped database structure
  mix ecto.migrate       # Runs the repository migrations
  mix ecto.migrations    # Displays the repository migration status
  mix ecto.rollback      # Rolls back the repository migrations
  mix phx.new.ecto       # Creates a new Ecto project within an umbrella project
```

所以當你試著輸入 `mix.ecto.gen.repo`, 會報出錯誤：

``` bash
  You can avoid this warning by passing the -r flag or by setting the
  repositories managed by those applications in your config/config.exs:

      config :todo, ecto_repos: [...]

   ,** (Mix) ecto.gen.repo expects the repository to be given as -r MyApp.Repo
```

所以我們照著 compile 的指示做。elixir 的 complie 都有相當好的指示，可以讓人知道到底哪裡出了問題。

`mix ecto.gen.repo -r Todo.Repo`

這時你的 terminal 指示器又說話了：

``` bash
  Don't forget to add your new repo to your supervision tree
  (typically in lib/todo/application.ex):

      supervisor(Todo.Repo, [])

  And to add it to the list of ecto repositories in your
  configuration files (so Ecto tasks work as expected):

      config :todo,
        ecto_repos: [Todo.Repo]
```

所以我們照著做，把 `supervisor(Todo.Repo, [])` 放到指定位置，也在 config/config.exs 裡，把 `config :todos` 這一行放到裡頭。

在 `config/config.exs` 裡要設定你聯接到你的 database 的資料庫，這裡預設內建是使用 Postgrex, 如果是其他的可能要去 ecto 的 documentation 看如何設定：

``` elixir
  config :todo, Todo.Repo,
    adapter: Ecto.Adapters.Postgres,
    database: "todo_repo",
    username: "你的帳號",
    password: "你的密碼",
    hostname: "localhost"
```

`mix ecto.create` 耶，我們的 repo 就建立好了，也有 database 了。

建立 item module
================

``` elixir
  # ~/lib/todo/item.ex
  defmodule Todo.Item do
    use Ecto.Schema

    schema "items" do
      field :title, :string
      field :completed, :boolean
      timestamps()
    end
  end
```

我們定義一個 schema，但我們還沒有告訴 database 要建立這個 items table，所以還要再寫一個 migration，去告訴 database，我們現在要建立一個 `items table`.

-   `mix ecto.gen.migration create_items`

``` elixir
  # ~/priv/repo/migrations/20170904XXXX_create_itemx.exs

  defmodule Todo.Repo.Migrations.CreateItems do
    use Ecto.Migration

    def change do
      create table(:items) do
        add :title, :string
        add :completed, :boolean
        timestamps()
      end

    end
  end
```

-   `mix ecto.migrate`

現在我們可以來試試這個 item 能不能用了！

`iex -S mix`

進入 ~iex~的環境裡面：

`alias Todo.{Repo, Item}`

你可以試著呼叫%Item{}的 struct:

      %Todo.Item{__meta__: #Ecto.Schema.Metadata<:built, "items">, completed: nil,
       id: nil, inserted_at: nil, title: nil, updated_at: nil}

`Repo.insert %Item{title: "my first item"}` 時， 會給你{:ok, ...}，這時就可以用~pattern match~的方式， `{:ok, item } =Repo.instert %Item{...}`

但 `completed` 是 nil 怪怪的，在 `todo/item.ex` 裡，改： `field :completed, :boolean, default: false`

      item2 = Repo.insert! %Item{title: "my second"}

      %Todo.Item{__meta__: #Ecto.Schema.Metadata<:loaded, "items">, completed: false,
       id: 2, inserted_at: ~N[2017-09-04 08:00:54.351209], title: "my second",
       updated_at: ~N[2017-09-04 08:00:54.351218]}

耶，這樣就有 default value 了。

再來，我們回去改第一個 item:

``` elixir
  item =  Repo.get(Item, 1) #Repo.get_by(Item, %{title: "my first item"})

  %{item | completed: false} # 在 elixir 裡面，可以打開 map 結構，再用 pipe operator 去改你想要改的東西

  Repo.update(%{item | completed: false})
```

耶完成了？結果竟然報錯？！

``` bash
  (ArgumentError) giving a struct to Todo.Repo.update/2 is not supported. Ecto is unable to properly track changes when a struct is given, an Ecto.Changeset must be given instead
```

要用 `Ecto.Changeset`... ，什麼是 changeset?

Changeset
=========

> Changesets allow filtering, casting, validation and definition of constraints when manipulating structs.

``` elixir
  ...
    import Ecto.Changeset
    def changeset(item, params \\ %{}) do
      item
      |> cast(params, [:title, :completed])
    end
```

在這個 params 裡，default value 是一個 map, 我只打算讓 `:title` 和 `:completed` 通過，只讓這兩個可以被改變。

``` bash
   Item.changeset(%Item{}, %{title: "something"})

  #Ecto.Changeset<action: nil, changes: %{title: "something"}, errors: [], data: #Todo.Item<>, valid?: true>
```

如果你在 iex 直接打這個的話，你就可以看到~changeset~的作用了，這裡是有顯示，有沒有 error, 是要放到哪個 dataset 裡，有 valid 過了嗎？

``` bash
  Item.changeset(item, %{completed: false}) |> Repo.update
```

這樣就可以 update `item` 了。

Relationship
============

現在來建立 user：

``` elixir

  defmodule Todo.User do
    use Ecto.Schema
    import Ecto.Changeset

    schema "users" do
      field :name, :string
      has_many :items, Todo.Item
      timestamps()
    end
  end
```

在 item.ex 裡加入 `belongs_to :user, Todo.User`

`mix ecto.gen.migration create_users`

``` elixir
  defmodule Todo.Repo.Migrations.CreateUsers do
    use Ecto.Migration

    def change do
      create table(:users) do
        add :name, :string
        timestamps()
      end

      alter table(:items) do
        add :user_id, references(:users)
      end

    end
  end
```

run `mix ecto.migrate`

回到 iex 裡：

-   `alias Todo.User`
-   `Repo.insert! %User{name: "Jonny"}`
-   耶~~~
-   要刪除的話： `Repo.get(User, 1) |> Repo.delete`

``` bash
  jonny = Repo.insert! %User{name: "Jonny"}
  # 可以試試 jonny.id, jonny.name
  # 那 jonny.items 呢？
```

``` bash
  Ecto.Association.NotLoaded<association :items is not loaded>
```

Association.NotLoaded? use `preload`: Preloads the associations into the given struct.

``` elixir
  jonny = Repo.preload(jonny, :items)
  jonny.items #[]
```

Build association
-----------------

``` elixir
  Item.changeset(item, %{user_id: 2}) |> Repo.update!

  %Todo.Item{__meta__: #Ecto.Schema.Metadata<:loaded, "items">, completed: false,
   id: 1, inserted_at: ~N[2017-09-04 07:57:04.769910], title: "my first item",
   updated_at: ~N[2017-09-04 08:25:10.196799],
   user: #Ecto.Association.NotLoaded<association :user is not loaded>,
   user_id: nil}
```

user<sub>id</sub> 沒有改變？！因為我們在 Item 裡的 changeset 並沒有讓 user<sub>id</sub> 可以改，如果要，就要回去 Item 裡的 changeset 更改。

``` elixir
  def changeset(item, params \\ %{}) do
    item
    |> cast(params, [:title, :completed, :user_id])
  end
```

      %Todo.Item{__meta__: #Ecto.Schema.Metadata<:loaded, "items">, completed: false,
       id: 1, inserted_at: ~N[2017-09-04 07:57:04.769910], title: "my first item",
       updated_at: ~N[2017-09-04 08:52:53.058675],
       user: #Ecto.Association.NotLoaded<association :user is not loaded>, user_id: 2}

耶~~

如果要讀出 user，就可以用：

``` elixir
  Repo.preload(item, :user) 

  16:56:24.728 [debug] QUERY OK source="users" db=0.6ms
  SELECT u0."id", u0."name", u0."inserted_at", u0."updated_at", u0."id" FROM "users" AS u0 WHERE (u0."id" = $1) [2]
  %Todo.Item{__meta__: #Ecto.Schema.Metadata<:loaded, "items">, completed: false,
   id: 1, inserted_at: ~N[2017-09-04 07:57:04.769910], title: "my first item",
   updated_at: ~N[2017-09-04 08:52:53.058675],
   user: %Todo.User{__meta__: #Ecto.Schema.Metadata<:loaded, "users">, id: 2,
    inserted_at: ~N[2017-09-04 08:38:01.865437],
    items: #Ecto.Association.NotLoaded<association :items is not loaded>,
    name: "Jonny", updated_at: ~N[2017-09-04 08:38:01.865442]}, user_id: 2}
```

``` elixir
  Ecto.build_assoc(jonny, :items, %{title: "hello world"})

  %Todo.Item{__meta__: #Ecto.Schema.Metadata<:built, "items">, completed: false,
   id: nil, inserted_at: nil, title: "hello world", updated_at: nil,
   user: #Ecto.Association.NotLoaded<association :user is not loaded>, user_id: 2}
```

Ecto.Query
==========

``` elixir
  1..75 |>
  Enum.each(fn(i) ->
  %Item{title: "Todo number #{i}", estimated_minutes: :random.uniform(120), user_id: Enum.random([2,3]), completed: Enum.random([true, false])} |> 
  Repo.insert!
  end)
```

以下是幾個 query 的練習：

``` elixir
  import Ecto.Query

  Repo.one from i in Item, select: count(i.id)

  Repo.one from i in Item, join: u in User, on: i.user_id == u.id, where: u.name == "Max", select: count(i.id)

  Repo.all from i in Item, join: u in User, on: i.user_id == u.id, group_by: u.id, select: %{count: count(i.id), name: u.name}

  #[%{count: 46, name: "Max"}, %{count: 30, name: "Jonny"}]
  query = from i in Item, join: u in User, on: i.user_id == u.id

  Repo.all from [i, u] in query, where: i.estimated_minutes < 10, order_by: i.estimated_minutes, select: %{todo: i.title, time: i.estimated_minutes, user: u.name}

  [%{time: 1, todo: "Todo number 60", user: "Max"},
   %{time: 1, todo: "Todo number 17", user: "Jonny"},
   %{time: 3, todo: "Todo number 35", user: "Max"},
   %{time: 3, todo: "Todo number 66", user: "Max"},
   %{time: 4, todo: "Todo number 36", user: "Max"},
   %{time: 5, todo: "Todo number 28", user: "Max"},
   %{time: 6, todo: "Todo number 37", user: "Max"},
   %{time: 7, todo: "Todo number 38", user: "Jonny"},
   %{time: 8, todo: "Todo number 22", user: "Jonny"},
   %{time: 9, todo: "Todo number 57", user: "Max"},
   %{time: 9, todo: "Todo number 52", user: "Jonny"}]

  Repo.all from [item, user] in query, group_by: user.id, select: %{user: user.name, todo: count(item.id), avg_min: avg(item.estimated_minutes) }

  SELECT u1."name", count(i0."id"), avg(i0."estimated_minutes") FROM "items" AS i0 INNER JOIN "users" AS u1 ON i0."user_id" = u1."id" GROUP BY u1."id" []
  [%{avg_min: #Decimal<58.4565217391304348>, todo: 46, user: "Max"},
   %{avg_min: #Decimal<62.6896551724137931>, todo: 30, user: "Jonny"}]
```
