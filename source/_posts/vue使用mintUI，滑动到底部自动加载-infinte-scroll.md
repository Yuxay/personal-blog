---
title: vue使用mintUI，滑动到底部自动加载(infinte-scroll)
date: 2019-04-23 20:13:01
tags: 
    - vue
    - mint-ui
categories: vue
cover: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=313184173,121254158&fm=26&gp=0.jpg
keywords: 'vuye,mint,mint-ui'
aplayer: true
---
**做此项目与官网的区别在于，数据总数，页码总数是由后台返回，而非前端设置**
```html
<ul v-infinite-scroll="loadMore" infinite-scroll-disabled="loading" infinite-scroll-distance="10">
    <li v-for="(item,index) in records" :key="index">	
    	<span class="nickname">{{item.nickName}}</span>
		<span class="date">{{item.createTime}}</span>
    </li>
</ul>
```
```json
res返回的数据格式如下
res:{
	data:{
		pages:2,
		tota;:35,
		records:[{
				nickname:'Tony',
				createTIme:'1568738766'
				},
				{
				nickname:'James',
				createTIme:'1536746827'
				}],
		}
}
```
```javascript
data() {
    return {
      paramId:'2e78542asd812e1', // 数据在数据库中存放的ID
      current: 0, // 查询评论的页码数
      pages: "", //总评论页码数
      loading:false
    };
 },
methods:{
 	loadMore(){
        this.listForCom();
    },
    listForCom() {
      // 获取评论列表
      let self = this;
      // 发请求所需要的参数
      var commentParams = { current: this.current, sectionId: this.paramId };
      // 请求的配置
      var commentOptions = {
        method: "POST",
        headers: { "content-type": "application/x-www-form-urlencoded" },
        data: qs.stringify(commentParams),
        url: "/share/comment/list"
      };
      // 发送请求
      this.$axios(commentOptions)
        .then(res => {
          if (res.data) {
            self.pages = res.data.pages;//将从后台获取的页码总数存入data中
            if (res.data.records) {
              // 将每一次获取到的数据拼接在已有的数据后面
              self.records = self.records.concat(res.data.records);
            }
            ++this.current; // 递增页码
            // 当获取到的数据量等于总的数据量时，禁用加载
            if(self.records.length == res.data.total){
                self.loading = true;
            }
          } 
        })
        .catch(err => console.log(err));
    },
}
```