<template>
  <div>
    <sdPageHeader title="รายละเอียดประวัติการทำงาน">
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
      <div style="display: flex; justify-content: flex-end; margin-bottom: 20px">
        <p>Dtail</p>
      </div>
      <div>
        <a-form name="sDash_grid-form" layout="horizontal">
          <a-row :gutter="30">
            <a-col :md="4" :xs="24" class="mb-25">
              <a-form-item>
                <a-input name="grid-2-1" placeholder="Gen ค่าใช้จ่าย" />
              </a-form-item>
            </a-col>
            <a-col :md="4" :xs="24" class="mb-25">
              <a-form-item>
                <a-input name="grid-2-2" placeholder="Gen ชั่วโมงการซ่อม" />
              </a-form-item>
            </a-col>
          </a-row>
        </a-form>
      </div>
      <a-row :gutter="25">
        <a-col :md="24" :xs="24">
          <LadingPages>
            <div class="table-bordered table-responsive">
              <a-table :columns="landingColumns" :dataSource="landingData" :pagination="false" />
            </div>
          </LadingPages>
        </a-col>
      </a-row>
    </Main>
  </div>
</template>

<script>
import { Main } from '../styled';
import { computed, onMounted, ref } from 'vue';
import { useStore } from 'vuex';
import { LadingPages } from '../dashboard/style';
import moment from 'moment';

const landingColumns = [
  {
    title: 'รหัสบิล',
    dataIndex: 'pages',
    key: 'pages',
  },
  {
    title: 'สายการผลิต',
    dataIndex: 'sessions',
    key: 'sessions',
  },
  {
    title: 'รหัสเครื่องจักร',
    dataIndex: 'rate',
    key: 'rate',
  },
  {
    title: 'ชื่องาน',
    dataIndex: 'ctr',
    key: 'ctr',
  },
  {
    title: 'ผู้รับผิดชอบ',
    dataIndex: 'percentage',
    key: 'percentage',
  },
  {
    title: 'ค่าใช้จ่าย',
    dataIndex: 'percentage',
    key: 'percentage',
  },
  {
    title: 'แผนก',
    dataIndex: 'percentage',
    key: 'percentage',
  },
  {
    title: 'ประเภท',
    dataIndex: 'percentage',
    key: 'percentage',
  },
  {
    title: 'วันเริ่ม',
    dataIndex: 'percentage',
    key: 'percentage',
  },
  {
    title: 'สถานะ',
    dataIndex: 'percentage',
    key: 'percentage',
  },
];

export default {
  name: 'detail-view',
  components: {
    Main,
    LadingPages,
  },
  setup() {
    const store = useStore();
    const landing = ref('year');
    const landingState = computed(() => store.state.chartContent.landingPageData);
    const landingData = computed(() =>
      landingState.value !== null
        ? [
            {
              key: '1',
              pages: (
                <router-link to="#" class="page-title">
                  Homepage
                </router-link>
              ),
              sessions: landingState.value.direct.sessions,
              rate: `${landingState.value.direct.rate}%`,
              ctr: landingState.value.direct.goals,
              percentage: `${landingState.value.direct.percent}%`,
            },
            {
              key: '2',
              pages: (
                <router-link to="#" class="page-title">
                  Our Service
                </router-link>
              ),
              sessions: landingState.value.email.sessions,
              rate: `${landingState.value.email.rate}%`,
              ctr: landingState.value.email.goals,
              percentage: `${landingState.value.email.percent}%`,
            },
            {
              key: '3',
              pages: (
                <router-link to="#" class="page-title">
                  List of Products
                </router-link>
              ),
              sessions: landingState.value.search.sessions,
              rate: `${landingState.value.search.rate}%`,
              ctr: landingState.value.search.goals,
              percentage: `${landingState.value.search.percent}%`,
            },
            {
              key: '4',
              pages: (
                <router-link to="#" class="page-title">
                  Contact us
                </router-link>
              ),
              sessions: landingState.value.media.sessions,
              rate: `${landingState.value.media.rate}%`,
              ctr: landingState.value.media.goals,
              percentage: `${landingState.value.media.percent}%`,
            },
            {
              key: '5',
              pages: (
                <router-link to="#" class="page-title">
                  Products
                </router-link>
              ),
              sessions: landingState.value.other.sessions,
              rate: `${landingState.value.other.rate}%`,
              ctr: landingState.value.other.goals,
              percentage: `${landingState.value.other.percent}%`,
            },
          ]
        : [],
    );

    const handleActiveChangeLanding = (event, value) => {
      event.preventDefault();
      landing.value = value;
      store.dispatch('landingPageFilterData', value);
    };

    onMounted(() => {
      store.dispatch('landingPageGetData');
    });

    return {
      landingState,
      landing,
      handleActiveChangeLanding,
      landingData,
      landingColumns,
    };
  },
  data() {
    return {
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
