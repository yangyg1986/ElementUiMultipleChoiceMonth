<template>
    <el-popover
      placement="bottom"
      title=""
      width="280"
      trigger="click"
      >
      <div class="el-picker-panel__body-wrapper">
        <div class="el-picker-panel__body">
          <div class="el-date-picker__header el-date-picker__header--bordered">
            <button type="button" aria-label="前一年" class="el-picker-panel__icon-btn el-date-picker__prev-btn el-icon-d-arrow-left" @click="preYear"></button>
            <span role="button" class="el-date-picker__header-label">{{yearObj}} 年</span>
            <button type="button" aria-label="后一年" class="el-picker-panel__icon-btn el-date-picker__next-btn el-icon-d-arrow-right" @click="nextYear"></button>
          </div>
          <div class="el-picker-panel__content">
            <ul>
              <li v-for="(item,index) in monthCurArray" :class="{current:item.tabActive}" :key="index" @click="selectMonth(item)">
                {{item.month}}月
              </li>
            </ul>
          </div>
        </div>
      </div>
      <el-input slot="reference"
                placeholder="请选择月份"
                suffix-icon="el-icon-date"
                v-model="input"
      >
      </el-input>
    </el-popover>
</template>

<script>
    export default {
        name: "Month",
        data() {
          return {
            yearObj:new Date().getFullYear(),
            monthArray:[],
            childArr:[1,2,3,4],
            input:'',
            selectArr:[],
            yearMonthMap:{},
            monthCurArray:[]
          };
        },
        props:{
          value:{
             type:String
          },
          rangeSeparator:{
            type: String
          }
        },
        mounted(){
          console.log(this.value);
          this.init()
        },
        methods:{
          init(){
            if(this.value){
              this.selectArr=this.value.split(',');
            }
            this.handleInput();
            if(!this.rangeSeparator){
              this.rangeSeparator='-'
            }
            this.monthArray=[];
            for(var i=1;i<13;i++){
              var tempObj=i>9?i.toString():'0'+i;
              this.monthArray.push({
                month:tempObj,
                tabActive:false
              })
            }
            this.monthArray.forEach((item)=>{
              var yearMonth=this.yearObj+'-'+item.month;
              this.selectArr.forEach((mItem)=>{
                if(yearMonth==mItem){
                  item.tabActive=true;
                }
              })
            });
            this.yearMonthMap[this.yearObj]=this.monthArray;
            this.monthCurArray=this.yearMonthMap[this.yearObj];
          },
          preYear:function () {
            if(this.yearObj<2000){
              return
            }else{
              this.yearObj--;
              this.handleChoose();
              console.log(this.selectArr);
            }
          },
          nextYear:function () {
            this.yearObj++;
            this.handleChoose();
            console.log(this.selectArr);
          },
          handleChoose(){
            this.monthArray=[];
            for(var key in this.yearMonthMap){
              if(this.yearObj==key){
                this.monthCurArray=this.yearMonthMap[this.yearObj];
              }else{
                this.monthArray=[];
                //获取12个月
                for(var i=1;i<13;i++){
                  var tempObj=i>9?i.toString():'0'+i;
                  this.monthArray.push({
                    month:tempObj,
                    tabActive:false
                  })
                }
                //处理选中
                this.monthArray.forEach((item)=>{
                  var yearMonth=this.yearObj+'-'+item.month;
                  this.selectArr.forEach((mItem)=>{
                    if(yearMonth==mItem){
                      item.tabActive=true;
                    }
                  })
                });
                this.yearMonthMap[this.yearObj]=this.monthArray;
                this.monthCurArray=this.monthArray;
              }
            }
          },
          selectMonth(item){
             item.tabActive=!item.tabActive;
             var yearMonth=this.yearObj+this.rangeSeparator+item.month;
             var pos=this.selectArr.indexOf(yearMonth);
             if(pos<0){
                this.selectArr.push(yearMonth)
             }else{
                this.selectArr.splice(pos,1);
             }
             this.handleInput();
          },
          handleInput(){
            this.input=this.selectArr.join();
            this.$emit('input',this.input)
          },
          handleChange(){
            console.log(this.value)
          }
        }
    }
</script>

<style scoped>
  .el-picker-panel__content ul{
    margin: 0;
    padding: 0;
    overflow: hidden;
    list-style: none;
  }
  .el-picker-panel__content ul li{
    list-style: none;
    width: 25%;
    float: left;
    text-align: center;
    padding: 10px 0;
  }
  .el-picker-panel__content ul li:hover{
    color: #0081c2;
    cursor: pointer;
  }
  .el-picker-panel__content ul li.current{
    color: #0081c2;
  }

</style>
