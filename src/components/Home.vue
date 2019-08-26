<template>
  <div class="m-home" style="padding-top: 46px;">
    <div class="header-bg"></div>
	<card>
      <div slot="content" class="card-box">
        <div class="card-header vux-1px-b">
            <div class="headerimg">
                <img src="http://zf.lanseemy.com/Public/Shop/images/avatar/avatar0.png" alt="用户头像">
            </div>
            <div class="user-mid">
                <router-link to="/UserEdit">{{ user.username || '请设置昵称' }}</router-link>
                <span :style="user.group > 1 ? 'color: #409EFF;' : ''">{{ user.group_text || '--' }}<router-link to="/Upgrade" class="upgrade">{{ user.group_max === 1 ? '续费' : '升级' }}</router-link></span>
                <div class="exp" v-if="user.have_text === '长期有效'" style="width: 105px;">有效期：{{ user.have_text }}</div>
                <div class="exp" v-else style="width: 125px;">有效期：{{ user && user.have_text.split(' ')[0] }}</div>
                <span class="user">会员名&nbsp;{{ user.nickname || '--' }}</span>
            </div>            
            <router-link class="pay" to="/Pay">充值</router-link>
        </div>
        <div class="card-flex card-footer">
            <div class="vux-1px-b item" style="padding-bottom: 5px;">
                <span class="money">{{ parseInt(user.money)/100 || 0 }}</span>
                <br/>
                账户余额（元）
            </div>
            <div class="item vux-1px-b" style="padding-bottom: 5px;">
                <span class="money">{{ parseInt(balance)/100 || 0 }}</span>
                <br/>
                已消费金额（元）
            </div>
            <div class="item" style="padding-top: 4px;">
                <span class="money">{{ parseInt(balance_all)/100 || 0 }}</span>
                <br/>
                充值总金额（元）
            </div>
            <div class="item" style="padding-top: 4px;">
                <span class="money">{{ balance_frozen/100 || 0 }}</span>
                <br/>
                冻结金额（元）
            </div>            
        </div>
      </div>
    </card>
    <div class="intro">             
    <!--   
        <x-icon type="ios-information-outline" size="40"></x-icon>
        <div class="tips">说明：系统按充值金额的10%计收服务费，（例如：充值1000元，实际可用金额为900元）</div>
    -->
    </div>
    <div class="m-grids">
        <grid :show-lr-borders="false" :show-vertical-dividers="false">
            <grid-item link="/Article" label="文章列表">
                <img slot="icon" src="../assets/article0.png">
            </grid-item>
            <grid-item link="/ShareArticle" label="已分享文章">
                <img slot="icon" src="../assets/article2.png">
            </grid-item>
            <grid-item link="/CommentDetail">
                <img slot="icon" src="../assets/article1.png">
                <span slot="label">文章评论</span>
            </grid-item>
        </grid>
        <grid :show-lr-borders="false" :show-vertical-dividers="false">
            <grid-item link="/Envelope" label="红包明细">
                <img slot="icon" src="../assets/envelope.png">
            </grid-item>
            <grid-item link="/Transaction" label="交易明细">
                <img slot="icon" src="../assets/tran_detail.png">
            </grid-item>
            <grid-item label="联系客服" @on-item-click="serverEve">
                <img slot="icon" src="../assets/server.png">
            </grid-item>
        </grid>
        <grid :show-lr-borders="false" :show-vertical-dividers="false">
            <grid-item link="/articlelist" label="文章库">
                <img slot="icon" src="../assets/article.png">
            </grid-item>
            <grid-item link="/artTransform" label="文章转换">
                <img slot="icon" src="../assets/article.png">
            </grid-item>            
            <grid-item class="placeholder">
            </grid-item>               
        </grid>
    </div>

    <div v-transfer-dom>
      <x-dialog v-model="showDialogSer" hide-on-blur :dialog-style="{'max-width': '100%', width: '100%', height: '50%', 'background-color': 'transparent'}">
        <p style="color:#fff;text-align:center;" @click="showDialogSer = false">
          <span style="font-size:30px; display: block;">联系客服</span>
          <img :src="wx_img" art="" style="width: 60%; height: auto;" />
          <br>
          <br>
          <x-icon type="ios-close-outline" style="fill:#fff;"></x-icon>
        </p>
      </x-dialog>
    </div>    
  </div>
</template>

<script>
import { XHeader, Actionsheet, TransferDom, Group, Cell, CellBox, XButton, Card, Grid, GridItem, GroupTitle, XDialog } from 'vux'

export default {
  directives: {
    TransferDom
  },
  components: {
    XHeader,
    Actionsheet,
    Group,
    Cell,
    XButton,
    Card,
    CellBox,
    Grid,
    GridItem,
    GroupTitle,
    XDialog
  },
  data () {
    return {
      title: '首页',
      showMenus: false,
      balance: '',
      balance_all: '',
      balance_frozen: '',
      user: '',
      showDialogSer: false,
      wx_img: ''
    }
  },
  mounted () {
    this.initData()
  },
  methods: {
    initData () {
      let that = this
      this.$axios.post('/api/mobile.php?s=/index/index', {})
      .then(res => {
        let data = res.data
        if (data.code === 1) {
          that.balance = data.data.balance
          that.balance_all = data.data.balance_all
          that.user = data.data.user
          that.balance_frozen = data.data.balance_frozen
          that.wx_img = data.data.wx_img
        }
      })
      .catch(err => {
        console.log(err)
        console.log(err.status)
        console.log(err.statusText)
        console.log('request fail')
      })
    },
    serverEve () {
      console.log('show server')
      this.showDialogSer = true
    }
  }
}
</script>

<style lang="less" scope>
@import '../styles/reset.less';
@import '../styles/1px.less';
a {
    color: initial;
}
.card-flex {
  display: flex;
  justify-content: space-around;
}
.card-footer {
  padding: 2px 0 20px 0;
  text-align: center;
  flex-wrap: wrap;
  .money {
      color: #f74c31;
  }
  .item {
      width: 50%;
  }
}
.vux-1px-r::after {
    // right: -2px;
}
.header-bg {
    background-color: @weuiColorSecond;
    width: 100%;
    height: 146px;
    position: absolute;
    z-index: -1;
    top: 0;
}
.m-home {
    .weui-panel {    
        margin: 24px 14px 0px 14px !important;
        border-radius: 10px;
        overflow: unset !important;
        &::before {
            content: unset !important
        }
    }
}
.card-box {
    font-size: 14px;
    .card-header {
        display: flex;
        padding-bottom: 14px;
        .pay {
            display: block;
            background-color: @weuiColorSecond;
            height: 40px;
            font-size: 18px;
            line-height: 40px;
            padding: 0 20px;
            position: absolute;
            right: 0;
            top: 50%;
            transform: translateY(-50%);
            border-top-left-radius: 20px;
            border-bottom-left-radius: 20px;
            color: #fff;            
        }
        .headerimg {
            margin-top: 14px;
            margin-left: 24px;
            img {
                width: 72px;
                height: 72px;
                border-radius: 50%;
            }
        }        
        .user-mid {
            display: flex;
            flex-direction: column;
            margin-left: 14px;
            padding-top: 10px;
            .upgrade {
                display: inline-block;
                margin-left: 14px;
                background-color: @weuiColorPrimary;
                color: #fff;
                padding: 1px 5px;
                font-size: 10px;
            }            
            .user {
                color: @weuiColorSecond;
            }
            .exp {
                color: #4565ae;
                background-color: #b0d8fc;
                display: inline-block;
                text-align: center;
                border-radius: 4px;
                font-size: 12px;
                margin: 3px 0;
                padding: 1px 3px;
                min-width: 105px;
            }
        }    
    }
}

.intro {
    padding: 20px 14px;
    display: flex;
    align-items: center;
    .vux-x-icon {
        fill: @weuiColorPrimary;
        margin-right: 10px;
    }
    .tips {
        font-size: 12px;
        color: #999;
    }
}
.m-grids {
    background-color: #fff;
}
.placeholder.weui-grid::after {
    height: 0 !important;
    border: unset !important;
}
</style>
