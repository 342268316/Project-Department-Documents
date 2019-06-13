首先在vue中下载axios插件，然后在要使用异步请求的页面中写入axios
<script>
import axios from "axios";


2.在页面上面写前端代码
//<form @submit="insertData">
//       <input type="text" v-model="name">
//       <button>添加</button>
//     </form>
//     <ul>
//       <li v-for="item,index in clazzList">
//         {{item.name}}
//         <span @click="del(item.id)">删除</span>
//       </li>
//     </ul>


3.写增删查功能

export default {
  name: "home",

  data() {
    return {
      clazzList: [],
      name: ""
    };
  },
  methods: {
    getList() {
      axios
        .get("http://127.0.0.1:7001/hell")
        .then(res => {
          console.log(res);
          this.clazzList = res.data;
        })
        .catch(err => {
          console.log(err);
        });
    },
    del(id) {
      axios
        .delete("http://127.0.0.1:7001/delhell/" + id)
        .then(response => {
          console.log(response);
          // this.clazzList = res.data;
          // this.getCollect();
          // this.ctx.redirect("/hell");
        })
        .catch(error => {
          console.log(error);
        });
    },

    insertData() {
      axios
        .post("http://127.0.0.1:7001/addhell", {
          name: this.name
        })
        .then(res => {
          this.clazzList = res.data;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created() {
    this.getList();
  },
  components: { menulist }
};
/script

4.在egg项目里面跨域

https://www.jianshu.com/p/0ddd84f4063c

5.在egg项目里连接数据库
https://blog.csdn.net/weixin_34351321/article/details/87072545


6.controller的home.js
'use strict';

const Controller = require('egg').Controller;

class HomeController extends Controller {
  async index() {
    await this.ctx.render("login.html")
  }
  async hell() {
    const clazzList = await this.app.model.Clazz.findAll();//查询班级列表
    this.ctx.body = clazzList
  }
  async addhell() {
      const namee = this.ctx.request.body.name
      const clazz = {
          name:namee
      }
      await this.app.model.Clazz.create(clazz);
      this.ctx.body = clazzList;

  }
  async delhell() {
    // const id = this.ctx.request.body.id;
    // // let id = this.ctx.params.id;
    // const clazz = await this.app.model.Clazz.findOne({
    //     where: {
    //         id: id
    //     }
    // });
    // clazz.destroy();

    // // clazzList.splice(id,1); //删除数据
    // this.ctx.body = clazzList;


    // // this.ctx.redirect("/clazz")


    // const id = this.ctx.request.body.clazz_id;
    // const {ctx} = this;
        const id = this.ctx.params.id;
    const clazzs = await this.app.model.Clazz.findOne({
        where: {
            id: id
        }
    });
    clazzs.destroy(id);

    //     ctx.body = await ctx.service.clazz.destroy(id)

}
}


module.exports = HomeController;


6.model的clazz.js


module.exports = app => {
    const {
        STRING
    } = app.Sequelize;

    const Clazz = app.model.define('clazz', {  //sequelize会自动创建主键
        name: STRING,
    })

    return Clazz;
}

7.model的students.js

module.exports = app => {
    const {
        STRING
    } = app.Sequelize;

    const Students = app.model.define('students', {
        name: STRING,
    })

    Students.associate = function () {
        app.model.Students.belongsTo(app.model.Clazz, {  //设置外键
            foreignKey: 'clazz_id',
            as: 'clazz'
        })
    }

    return Students;
}
