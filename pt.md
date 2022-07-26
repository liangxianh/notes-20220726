### 1 布尔型数据存储到cookie或者session中会变为字符串类型的数据，需要将内容进行转换一下在使用

* js常常用到将数据存储到<b>cookie</b>和<b>session</b>中，或者从<b>客户端的方法中获取数据</b>，一定注意获取的对象或者数据是字符串类型的，使用之前记得进行转换，可以使用JSON.parse（）

```
    Cookies.set('t', false)
    const tttt = Cookies.get('t')
    console.log('tttt', tttt, tttt && true, JSON.parse(tttt) && true)

    localStorage.setItem('tt', false)
    const lsVars = localStorage.getItem('tt')
    console.log('lsVars', lsVars, lsVars && true, JSON.parse(lsVars) && true)

    // tttt false true false
    // lsVars false true false
```


### 2 浏览器突然就不能newwork和console了，什么内容都看不到

* 可能原因是newwork被过滤了，删除过滤条件即可
* 但是console的内容即使恢复设置也不行，还没找到具体的原因？？？


### 3 vue 动态新增字段v-model到input上，输入一个字符后就离焦了
源代码如下：
```
<el-form-item v-else
  v-for="(item, index) in newObj.list"
  :key="item.pars2"
  class="item-wrapper">
  <div class="flex-left">
    <el-form-item label="参数1"
      label-width="194px"
      :prop="'list.' + index + '.pars1'"
      :rules="{ required: true, message: '不能为空', trigger: 'change' }">
      <el-select v-model="item.pars1"
        filterable
        no-data-text="No Data"
        clearable
        placeholder="参数1"
        size="small">
        <el-option v-for="item in pars1List"
          :key="item.label"
          :label="item.label"
          :value="item.value" />
      </el-select>
    </el-form-item>
    <el-form-item label="参数2"
      label-width="194px"
      :prop="'list.' + index + '.pars2'"
      :rules="{ required: true, message: '不能为空', trigger: 'blur' }">
      <el-input v-model="item.pars2"
        maxlength="50"
        type="textarea"></el-input>
    </el-form-item>
  </div>
  <div class="flex-right">
    <el-button type="primary"
      circle
      size="mini"
      icon="el-icon-minus"
      @click="delPars(index)"></el-button>
    <el-button v-if="index === newObj.list.length-1 && newObj.list.length < 50"
      type="primary"
      circle
      size="mini"
      icon="el-icon-plus"
      @click="addPars()"></el-button>
  </div>
</el-form-item>
```
问题的根源是:key="item.pars2"， 这个值绑定了动态输入的内容，内容输入后相当于key值变化了 div就会被重新渲染导致了离焦的效果，更新:key="index"即可


### 4. vue 动态添加参数（最多50条）并v-model到select上面，该select内容很多（近3200条model数据）导致页面非常卡顿（很多个select）

这样如过不做任何优化的话，页面将会存在最多50 * 3200 = 160000 16万条options数据， 这样页面会很卡顿；

网上查看了有一下几种方案：

* 使用[vue-virtual-scroll-list](https://www.npmjs.com/package/vue-virtual-scroll-list) 的

* 使用[vue-virtual-scroller](http://www.wjhsh.net/lst619247-p-14580686.html)

* 还有一种使用[elementui和vue-virtual-scroll-list](https://www.pudn.com/news/628bc32c16e0ca714148954c.html)可以解决一个select下拉选项过多的问题；
* 使用懒加载[vue懒加载指令](https://juejin.cn/post/6979186001411473444)

但是这几种种方案都不满足我项目的需求，多是一个select，下拉列表超级多的情况，于是自己想出了两种优化方案：

* 1 针对下拉列表进行点击后渲染列表数据，离焦后清空数据（或只保留当前选中的数据）；

这种情况需要有多少条动态数据就创建多少个options变量，牺牲了内存来换取页面的流畅；不能共用一个options，否则点击会全部同时渲染，也同样的会卡顿；

这种离焦清空数据，在点击option选项时就会被触发，直接就被清空，数据值没有不能被点选上；针对这种进行了优化，在select聚焦时就对当前下拉列表进行渲染，同时对所在的其他参数进行数据清空（或者只保留当前所选项）

* 2 针对下拉列表提供搜索功能直接过滤一批（这样内容量会较小）

针对第一种方法实现如下；(并且做了如下优化)
1 非index值作为key，因为下拉数据会存在去重复处理当各个节点的index多数会变化，若变化的是第一个就会所有的都销毁在重新生成；这样成本比较大[详细原理参见](https://wubin.work/blog/articles/223)

2 使用了elementui自带的虚拟列表（应用于大数据量的情况，可达10w条）。使用virtual-scroll开启，然后传入items数组即可，暂不支持创建条目的功能。
```
<!-- html -->
    <el-form ref="form"
      :model="newObj"
      :rules="rules"
      class="form-cell"
      label-width="135px">

      <el-form-item class="search-item"
        label="参数"
        style="margin-bottom: 16px;"
       >
        <el-select v-model="newObj.pars"
          :disabled="newOrEdit === 'edit'"
          size="small"
          filterable>
          <el-option v-for="item in parsOptions"
            :key="getKey(item.parsName, item.parsId)"
            :label="item.parsName"
            :value="item.parsId">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item v-if="newObj.list.length === 0">
        <el-button type="primary"
          icon="el-icon-plus"
          circle
          @click="addPars()"
          size="small"></el-button>
      </el-form-item>
      <el-form-item v-else
        style="margin-bottom: 0"
        v-for="(item, index) in newObj.list"
        :key="index"
        class="item-wrapper">
        <div class="flex-left">
          <el-form-item :prop="'list.' + index + '.pars1'"
            :rules="{ required: true, message: '不能为空', trigger: 'change' }">
            <el-select v-model="item.pars1"
              filterable
              no-data-text="No Data"
              clearable
              :virtual-scroll="true"
              placeholder="参数list-pars1"
              @focus="updateFilter(index)"
              size="small">
              <!--  -->
              <el-option v-for="inner in item.filterOptions"
                :key="inner.desc"
                :label="inner.desc"
                :value="inner.desc" />
            </el-select>
          </el-form-item>
          <el-form-item :prop="'list.' + index + '.pars2'"
            :rules="{ required: true, message: '参数list-pars2不能为空', trigger: 'change' }">
            <el-input v-model="item.pars2"
              maxlength="50"
              placeholder="参数list-pars2"
              type="text"></el-input>
          </el-form-item>
        </div>
        <div class="flex-right">
          <el-button type="primary"
            circle
            size="mini"
            icon="el-icon-minus"
            @click="delPars(index)"></el-button>
          <el-button v-if="index === newObj.list.length-1 && newObj.list.length < 50"
            type="primary"
            circle
            size="mini"
            icon="el-icon-plus"
            @click="addPars()"></el-button>
        </div>
      </el-form-item>
      <el-form-item class="submit-box">
        <el-button size="small"
          @click="handleClose">
          取消
        </el-button>
        <el-button type="primary"
          :loading="submit_loading"
          size="small"
          @click="submit('form')">
          保存
        </el-button>
      </el-form-item>
    </el-form>
    
<!--  js    -->

// 优化内容，当聚焦时更新下拉列表，同时其他下拉列表（删除内容，只保留被选中的）
    updateFilter(idx) {
      const len = this.newObj.list.length
      for (let i = 0; i < len; i++) {
        if (i === idx) {
          this.updateFilterAdd(i)
        } else {
          this.updateFilterDel(i)
          console.log(i)
        }
      }
    },
    updateFilterAdd(idx) {
      this.$nextTick(() => {
        // filterList下拉列表参数不可重复，此为过滤后的数据
        this.$set(this.newObj.list[idx], 'filterOptions', JSON.parse(JSON.stringify(this.filterList)))
      })
    },
    updateFilterDel(idx) {
      const model = this.newObj.list[idx].model
      if (model) {
        let eleSelected = []
        // 注意：此处为全量列表，非过滤后的；
        this.modelList.forEach((item) => {
          if (item.desc === model) {
            eleSelected.push(item)
          }
        })
        this.$set(this.newObj.list[idx], 'filterOptions', eleSelected)
      } else {
        this.$set(this.newObj.list[idx], 'filterOptions', [])
      }
    }
```


### 5 正式环境报错 Failed to load module script: expected a javascript module script but the server responded with a mime type of text/hmtl, strict mime type checking is enforced for module scripts per html spec
测试预发环境都是好的，但是正式环境偶尔会出现上面的错误（现象是某一台电脑登陆某一个账号导致几乎所有页面访问不了，其他账号或多或少可以访问；）
![image](https://user-images.githubusercontent.com/31762176/196906691-fa9af7d0-cdb0-47ea-adf7-6ebc6429c415.png)
```
发现是env prod.js 配置baseurl出现问题；
// 有问题的代码
window.BASE_URL = "https://yourdomain/yourpubulicpath/ ";

// 更新后的代码
window.BASE_URL = "https://yourdomain/yourpubulicpath";
```
baseurl后面多加入了‘/ ’导致报出该错误

参考文档：
[URL中的双斜杠是什么意思？](http://129.226.226.195/post/15658.html)
[类似问题1](https://github.com/vitejs/vite/discussions/9332)
[类似问题2](https://github.com/nuxt/framework/issues/7285)
[类似问题3](https://github.com/vitejs/vite/issues/8429)
[类似问题4](https://stackoverflow.com/questions/72070748/failed-to-load-module-script-expected-a-javascript-module-script-but-the-server)
    
    
    
![image](https://user-images.githubusercontent.com/31762176/206077879-59e94b8e-e238-453c-8f9c-065f19ad931d.png)

查找了一些资源 发现可能是因为vite.config.js里面的base配置问题,删掉base 利用nginx 配置；
base 配置公共基础路径；
```
现有的 
base: '/my-project/',

利用docker部署的conf资源修改 fail
由
location / {
    root /usr/share/nginx/html;
    ndex  index.html index.htm;
    try_files $uri $uri/ /index.html;
}
修改为如下的location：
server {
    server_name www.baidu.com;

    server_tokens off;

    location /my-api {
        proxy_pass https://www.api.com/;
    }
 
    location /my-project/ {
        root /usr/share/nginx/html/;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
}

```
观察情况一段时间
    
    
    
    
    
    
    
    
    
    
    
    
