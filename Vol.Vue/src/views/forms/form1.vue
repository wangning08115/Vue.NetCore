<template>
  <div class="single-form">
    <Divider>单列表单/数据源后台自动加载绑定(下拉框选择时可触发事件)</Divider>
    <VolForm ref="myform" :loadKey="loadKey" :formFileds="formFileds" :formRules="formRules"></VolForm>
    <Button type="success" style="margin-bottom:20px;" long @click="getForm">获取表单</Button>
    <Button type="error" long @click="reset">重置</Button>
  </div>
</template>
<script>
import VolForm from "@/components/basic/VolForm.vue";
let $vue;
export default {
  components: { VolForm },
  created() {
    $vue = this;
  },
  methods: {
    getForm() {
      if (!this.$refs.myform.validate()) {
        return;
      }
      this.$message.error(JSON.stringify(this.formFileds));
    },
    reset() {
      //重置表单，重置时可指定重置的值，如果没有指定重置的值，默认全部清空
      let data = { Variety: "1", AvgPrice: 888 };
      this.$refs.myform.reset(data);
      this.$message.error("表单已重置");
    }
  },
  data() {
    return {
      loadKey: true,
      formFileds: {
        Variety: "",
        AgeRange: "",
        DateRange: [],
        City: "",
        AvgPrice: null,
        Date: ""
      },
      formRules: [
        [
          {
            columnType: "string",
            dataKey: "age", //后台下拉框对应的数据字典编号
            data: [], //loadKey设置为true,会根据dataKey从后台的下拉框数据源中自动加载数据
            //data是须的参数，可以默认一个[]
            title: "月龄",
            required: true, //设置为必选项
            field: "AgeRange",
            colSize: 12,
            placeholder: "可触发事件的下拉框",
            type: "select",
            onChange(value, param) {//设置选择数据时触发的事件
              $vue.formFileds.AvgPrice =
                (Math.random(1, 1000) * 100).toFixed(2) * 1;
              $vue.$message(
                "当前选中的值为[" +
                  value +
                  "],选中后给成交均价赋一个随机值[" +
                  $vue.formFileds.AvgPrice +
                  "]"
              );
            }
          }
        ],
        [
          {
            columnType: "string",
            title: "品种",
            dataKey: "pz",
            //如果这里绑定了data数据，后台不会加载此数据源
            data: [{ key: "1", value: "1" }, { key: "2", value: "2" }],
            required: false,
            field: "Variety",
            colSize: 12,
            data: [],
            type: "select"
          }
        ],
        [
          {
            columnType: "string",
            dataKey: "city",
            title: "城市",
            required: true,
            field: "City",
            colSize: 12,
            data: [],
            type: "select"
          }
        ],
        [
          {
            columnType: "decimal",
            title: "成交均价",
            required: true,
            placeholder: "你可以自己定义placeholder显示的文字", //显示自定义的信息
            field: "AvgPrice",
            colSize: 12
          }
        ],
        [
          {
            columnType: "datetime",
            title: "日期",
            required: true,
            field: "Date",
            colSize: 12,
            type: "datetime"
          }
        ],
        [
          {
            columnType: "datetime",
            title: "开始结束日期",
            range: true, //设置为true可以选择开始与结束日期
            required: false,
            field: "DateRange",
            colSize: 12,
            type: "date"
          }
        ]
      ]
    };
  }
};
</script>
<style scoped>
.single-form {
  border: 1px solid #eee;
  position: absolute;
  max-width: 800px;
  padding: 30px;
  left: 0;
  right: 0;
  margin: auto;
  margin-top: 20px;
}
</style>