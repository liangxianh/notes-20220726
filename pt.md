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


### 4. vue 动态添加参数并v-model到select上面，该select内容很多（3200条model数据）导致页面非常卡顿

想到

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

```



    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
