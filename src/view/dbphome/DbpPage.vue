<template>
  <div>
    <sdPageHeader title="BD">
      <template v-slot:buttons>
        <div class="page-header-actions">
          <sdExportButton />
          <sdButton size="small" type="primary">
            <sdFeatherIcons type="save" size="14" />
            SAVE
          </sdButton>
        </div>
      </template>
    </sdPageHeader>
    <Main>
      <a-row :gutter="25">
        <a-col :md="24" :xs="24">
          <a-tabs>
            <a-tab-pane key="1" tab="MTBF/MTTR">
              <a-col :xs="24">
                <div class="d-flex justify-content-center">
                  <DatePickerWrapper>
                    <p>วันที่เริ่มถึงสิ้นสุด</p>
                    <a-range-picker
                      :default-value="[moment('2015/01/01', dateFormat), moment('2015/01/01', dateFormat)]"
                      :format="dateFormat"
                    />
                  </DatePickerWrapper>
                </div>
                <br />
                <div class="full-width-table">
                  <a-table
                    :columns="columns"
                    :data-source="data"
                    bordered
                    size="middle"
                    :scroll="{ x: 'calc(700px + 50%)', y: 240 }"
                  />
                </div>
              </a-col>
            </a-tab-pane>
            <a-tab-pane key="2" tab="เวลาเดินเครื่อง">
              <a-col :xs="24">
                <MonthCalendar />
              </a-col>
            </a-tab-pane>
            <a-tab-pane key="3" tab="Detail">
              <div class="card-grid-wrap">
                <sdCards
                  title="รหัสบิล : NT-9 รหัสเครื่องจักร : N0915-TT06-061-F RANK : A"
                  size="default"
                  :border="false"
                >
                  <a-table :columns="columns2" :data-source="data2" bordered>
                    <template>
                      <a>{{ text }}</a>
                    </template>
                  </a-table>
                </sdCards>
              </div>
            </a-tab-pane>
          </a-tabs>
        </a-col>
      </a-row>
    </Main>
  </div>
</template>

<script>
import { Main, DatePickerWrapper } from '../styled';
import MonthCalendar from '../apps/calendar/overview/Month.vue';
import moment from 'moment';
const columns = [
  {
    title: 'สายผลิด',
    dataIndex: 'name',
    key: 'name',
    width: 100,
    fixed: 'left',
  },
  {
    title: 'ชม.ผลิต',
    dataIndex: 'hr',
    key: 'hr',
    width: 100,
    fixed: 'left',
  },
  {
    title: 'แผนก EF',
    children: [
      {
        title: 'ครั้ง',
        dataIndex: 'age',
        key: 'age',
        width: 100,
      },
      {
        title: 'ชม.BD',
        dataIndex: 'Address',
        key: 'Address',
        width: 100,
      },
      {
        title: '%BD',
        dataIndex: 'Address',
        key: 'Address',
        width: 100,
      },
      {
        title: 'MTBF',
        dataIndex: 'Address',
        key: 'Address',
        width: 200,
      },
      {
        title: 'MTTR',
        dataIndex: 'Address',
        key: 'Address',
        width: 100,
      },
    ],
  },
  {
    title: 'แผนก ME',
    children: [
      {
        title: 'ครั้ง',
        dataIndex: 'age',
        key: 'age',
        width: 100,
      },
      {
        title: 'ชม.BD',
        dataIndex: 'Address',
        key: 'Address',
        width: 100,
      },
      {
        title: '%BD',
        dataIndex: 'Address',
        key: 'Address',
        width: 100,
      },
      {
        title: 'MTBF',
        dataIndex: 'Address',
        key: 'Address',
        width: 100,
      },
      {
        title: 'MTTR',
        dataIndex: 'Address',
        key: 'Address',
        width: 100,
      },
    ],
  },
];
const columns2 = [
  {
    title: 'สายผลิด',
    dataIndex: 'name',
    scopedSlots: { customRender: 'name' },
  },
  {
    title: 'แผนก',
    className: 'column-money',
    dataIndex: 'money',
  },
  {
    title: 'ประเภทการหยุด',
    dataIndex: 'address',
  },
  {
    title: 'วันเวลา ถึง วันเวลา',
    dataIndex: 'address',
  },
  {
    title: 'รวมชั่วโมงการหยุด',
    dataIndex: 'address',
  },
];

const data2 = [
  {
    key: '1',
    name: 'John Brown',
    money: '￥300,000.00',
    address: 'New York No. 1 Lake Park',
  },
  {
    key: '2',
    name: 'Jim Green',
    money: '￥1,256,000.00',
    address: 'London No. 1 Lake Park',
  },
  {
    key: '3',
    name: 'Joe Black',
    money: '￥120,000.00',
    address: 'Sidney No. 1 Lake Park',
  },
];

const data = [];
for (let i = 0; i < 100; i++) {
  data.push({
    key: i,
    name: 'John Brown',
    age: i + 1,
    street: 'Lake Park',
    building: 'C',
    number: 2035,
    companyAddress: 'Lake Street 42',
    companyName: 'SoftLake Co',
    gender: 'M',
  });
}

export default {
  name: 'DbpPage',
  components: {
    Main,
    MonthCalendar,
    DatePickerWrapper,
  },
  data() {
    return {
      data,
      columns,
      data2,
      columns2,
      dateFormat: 'YYYY/MM/DD',
      monthFormat: 'YYYY/MM',
      dateFormatList: ['DD/MM/YYYY', 'DD/MM/YY'],
    };
  },
  methods: {
    onChange(date, dateString) {
      console.log(date, dateString);
    },
    moment,
  },
};
</script>
