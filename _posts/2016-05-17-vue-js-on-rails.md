---
layout: post
title: "在rails上使用vue.js"
date:   2016-05-17 11:05:19 +0800
categories: programming
tag: [rails, vue.js]
---

前一陣子看了好幾篇vue.js的介紹文，本來是想學react的，不過react之前看了好幾個tutorial，可能愚鈍的我當時沒有弄得很懂，而vue的tutorial看起來親民很多，一邊想要把vuejs裝到rails上,一邊也學習著如何在rails上使用ajax和json來和前端搭配。

本篇文章快速用vue.js 在rails 上用呈現to do list。

<!-- more -->
### vue.js
在[官網](http://vuejs.org/),
有很不錯的tutorial。不過後來我找的tutorial是在github上的[awesome-vue](https://github.com/vuejs/awesome-vue)看到的。

### rails
有[vuejs-rails](https://github.com/adambutler/vuejs-rails)的gem。

#### 安裝流程
1. `gem 'vuejs-rails'`

2. 在application.js裡頭加入
```javascript
    //= require vue
    //= require vue-resource
```

3. 把turbolinks相關的地方都關起來
  - remove `gem "turbolinks"` in Gemfile
  - remove `//=require turbolinks` in application.js
  - remove 'data-turbolinks-track' in layout/applicaiton.html.erb
4. 在`app/view/layout/applicaiton.html.erb`裡，把這一行
 `<%= javascript_include_tag 'application' %> `,
移到`</body>`前面，把javascript_include_tag裡頭的turbolinks選項都刪除。


這樣就可以準備上路了！

### TODO list
1. 先新建一個to do list 的app
``` ruby
    rails new todolist
    cd todolist
    rails g scaffold Todolist item:string
    rake db:migrate
```
 2. 在 app/controllers/todolists_controller 裡
 ``` ruby
    def index
      @todolists = Todolist.all
      respond_to do |format| 
        format.html
        format.json { render :json => @todolists }
      end
    end
 ```
3.在view/todolists/index.html.erb裡
``` html

    <div class="container">
      <my-tasks></my-tasks>
    </div>


    <template id="my-template">
    <h1>My tasks</h1>

    <table>
      <thead>
        <tr>

          <th>Item</th>
        </tr>
      </thead>

      <tbody>
          <tr v-for="task in list">
            <td>{{ task.item }}</td>
          </tr>
      </tbody>
    </table>
    </template>

    <br>
    <%= link_to 'New Todolist', new_todolist_path %>
```
在這裡使用`v-for`, 還有template，等等要用

4. 在app/asset/javascripts/todolists.js裡
``` javascript
    Vue.component('my-tasks',{
      template:'#my-template',
      data: function() {
        return {
          list: []
        };
      },
      
      created: function() {
        this.fetchTaskList();
      },

      methods: {
        fetchTaskList: function() {
          var resource = this.$resource('/todolists.json/{ id }');
          resource.get().then(function(response){
            this.list = response.data;
          });
        }
      } 
    });
     
    new Vue({
      el: 'div'
    });
```
如果不要用creating resources，可以精簡成http get:
``` javascript

fetchTaskList = function() {
this.$http('/todolists.json').then(function (response) {
  this.list = response.data;
  }
}
```

這裡使用的是vue的componet，vue有個很好用的[vue-resource](https://github.com/vuejs/vue-resource), 可以讓你
不用使用jquery來讀取ajax的request。把json的檔案讀取後，直接傳給component，
就可以顯示出來to do list了！
