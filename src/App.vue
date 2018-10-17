<!--
.sync 修饰符，被修饰的prop会保持响应
-->
<template>
  <div id="app">
    <!--
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    -->
    <TableWithPage :datas="tableData1" :columns="tableColumns1" :total="total" :current.sync="current"  @on-change="changePage"/>
    
    <Tabs type="card">
        <TabPane v-for="tab in tabs" :key="tab" :label="'Page' + tabData[tab-1].page">
          <Table :data="tabData[tab-1].pageData" :columns="tableColumns1"/>
        </TabPane>
    </Tabs>
    <br><br>
    <Form ref="formValidate" :model="formItem" :rules="ruleValidate" :label-width="80">
        <FormItem label="Input" prop="input">
            <Input v-model="formItem.input" placeholder="Enter something..."/>
        </FormItem>
        <FormItem label="Select" prop="select">
            <Select v-model="formItem.select">
                <Option value="beijing">New York</Option>
                <Option value="shanghai">London</Option>
                <Option value="shenzhen">Sydney</Option>
            </Select>
        </FormItem>
        <FormItem>
            <Row>
                <Col span="11">
                <FormItem label="Date" prop="date">
                    <DatePicker type="date" placeholder="Select date" v-model="formItem.date"></DatePicker>
                </FormItem>
                </Col>
                <Col span="2" style="text-align: center">-</Col>
                <Col span="11">
                <FormItem label="Time" prop="time">
                    <TimePicker type="time" placeholder="Select time" v-model="formItem.time"></TimePicker>
                </FormItem>
                </Col>
            </Row>
        </FormItem>
        <FormItem label="Radio" prop="radio">
            <RadioGroup v-model="formItem.radio">
                <Radio label="male">Male</Radio>
                <Radio label="female">Female</Radio>
            </RadioGroup>
        </FormItem>
        <FormItem label="Checkbox" prop="checkbox">
            <CheckboxGroup v-model="formItem.checkbox">
                <Checkbox label="Eat"></Checkbox>
                <Checkbox label="Sleep"></Checkbox>
                <Checkbox label="Run"></Checkbox>
                <Checkbox label="Movie"></Checkbox>
            </CheckboxGroup>
        </FormItem>
        <FormItem label="Switch" prop="switch">
            <i-switch v-model="formItem.switch" size="large" true-value="open" false-value="close">
                <span slot="open">open</span>
                <span slot="close">close</span>
            </i-switch>
        </FormItem>
        <FormItem label="Slider" prop="slider">
            <Slider v-model="formItem.slider" range></Slider>
        </FormItem>
        <FormItem label="Text" prop="textarea">
            <Input v-model="formItem.textarea" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Enter something..."/>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="handleSubmit('formValidate')">Submit</Button>
            <Button style="margin-left: 8px">Cancel</Button>
        </FormItem>
    </Form>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import TableWithPage from "./components/TableWithPage.vue";

export default {
  name: "app",
  data: function() {
    return {
      tabs: 1,
      current: 1,
      total: 100,
      formItem: {
        input: "",
        select: "",
        radio: "male",
        checkbox: [],
        switch: true,
        date: "",
        time: "",
        slider: [20, 50],
        textarea: ""
      },
      ruleValidate: {
        input: [
          {
            required: true,
            message: "The name cannot be empty",
            trigger: "blur"
          }
        ],
        select: [
          {
            required: true,
            message: "Please select the city",
            trigger: "change"
          }
        ],
        radio: [
          { required: true, message: "Please select gender", trigger: "change" }
        ],
        date: [
          {
            required: true,
            type: "date",
            message: "Please select the date",
            trigger: "change"
          }
        ],
        time: [
          {
            required: true,
            type: "string",
            message: "Please select time",
            trigger: "change"
          }
        ],
        textarea: [
          {
            type: "string",
            min: 20,
            message: "Introduce no less than 20 words",
            trigger: "blur"
          }
        ]
      },
      tableData1: this.mockTableData1(),
      tabData: [{page:1, pageData:[]}],
      tableColumns1: [
        {
          title: "Name",
          key: "name"
        },
        {
          title: "Status",
          key: "status",
          render: (h, params) => {
            const row = params.row;
            const color =
              row.status === 1
                ? "primary"
                : row.status === 2
                  ? "success"
                  : "error";
            const text =
              row.status === 1
                ? "Working"
                : row.status === 2
                  ? "Success"
                  : "Fail";

            return h(
              "Tag",
              {
                props: {
                  type: "dot",
                  color: color
                }
              },
              text
            );
          }
        },
        {
          title: "Portrayal",
          key: "portrayal",
          render: (h, params) => {
            return h(
              "Poptip",
              {
                props: {
                  trigger: "hover",
                  title: params.row.portrayal.length + "portrayals",
                  placement: "bottom"
                }
              },
              [
                h("Tag", params.row.portrayal.length),
                h(
                  "div",
                  {
                    slot: "content"
                  },
                  [
                    h(
                      "ul",
                      this.tableData1[params.index].portrayal.map(item => {
                        return h(
                          "li",
                          {
                            style: {
                              textAlign: "center",
                              padding: "4px"
                            }
                          },
                          item
                        );
                      })
                    )
                  ]
                )
              ]
            );
          }
        },
        {
          title: "People",
          key: "people",
          render: (h, params) => {
            return h(
              "Poptip",
              {
                props: {
                  trigger: "hover",
                  title: params.row.people.length + "customers",
                  placement: "bottom"
                }
              },
              [
                h("Tag", params.row.people.length),
                h(
                  "div",
                  {
                    slot: "content"
                  },
                  [
                    h(
                      "ul",
                      this.tableData1[params.index].people.map(item => {
                        return h(
                          "li",
                          {
                            style: {
                              textAlign: "center",
                              padding: "4px"
                            }
                          },
                          item.n + ":" + item.c + "People"
                        );
                      })
                    )
                  ]
                )
              ]
            );
          }
        },
        {
          title: "Sampling Time",
          key: "time",
          render: (h, params) => {
            return h("div", "Almost" + params.row.time + "days");
          }
        },
        {
          title: "Updated Time",
          key: "update",
          render: (h, params) => {
            return h(
              "div",
              this.formatDate(this.tableData1[params.index].update)
            );
          }
        }
      ]
    };
  },
  components: {
    HelloWorld,
    TableWithPage
  },
  methods: {
    handleSubmit(name) {
      this.$Message.info(JSON.stringify(this.formItem));
      this.$refs[name].validate(valid => {
        if (valid) {
          this.$Message.success("Success!");
        } else {
          this.$Message.error("Fail!");
        }
      });
    },
    handleReset(name) {
      this.$refs[name].resetFields();
    },
    mockTableData1() {
      let data = [];
      for (let i = 0; i < 10; i++) {
        data.push({
          name: "Business" + Math.floor(Math.random() * 100 + 1),
          status: Math.floor(Math.random() * 3 + 1),
          portrayal: ["City", "People", "Cost", "Life", "Entertainment"],
          people: [
            {
              n: "People" + Math.floor(Math.random() * 100 + 1),
              c: Math.floor(Math.random() * 1000000 + 100000)
            },
            {
              n: "People" + Math.floor(Math.random() * 100 + 1),
              c: Math.floor(Math.random() * 1000000 + 100000)
            },
            {
              n: "People" + Math.floor(Math.random() * 100 + 1),
              c: Math.floor(Math.random() * 1000000 + 100000)
            }
          ],
          time: Math.floor(Math.random() * 7 + 1),
          update: new Date()
        });
      }
      
      return data;
    },
    formatDate(date) {
      const y = date.getFullYear();
      let m = date.getMonth() + 1;
      m = m < 10 ? "0" + m : m;
      let d = date.getDate();
      d = d < 10 ? "0" + d : d;
      return y + "-" + m + "-" + d;
    },
    changePage() {
      this.tabData[this.tabData.length-1].pageData = this.tableData1;
      this.tabData.push({page:this.current});
      this.$Message.info("Page" + this.current);
      // The simulated data is changed directly here, and the actual usage scenario should fetch the data from the server
      this.tableData1 = this.mockTableData1();
      this.tabs++;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 32px;
}
</style>
