<template>
  <div>
    <sdPageHeader title="จัดการผู้ใช้">
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
        <sdButton style="margin-right: 10px;" size="default" type="primary">
          <sdFeatherIcons type="plus" size="14" /> <span>เพิ่มผู้ใช้</span>
        </sdButton>
        <a-form name="sDash_textarea" layout="vertical">
          <a-form-item name="basic-textarea">
            <a-input placeholder="input placeholder" />
          </a-form-item>
        </a-form>
      </div>
      <a-row :gutter="25">
        <a-col :md="24" :xs="24">
          <sdCards title="ผู้ใช้ทั้งหมด" size="default">
            <LadingPages>
              <div class="table-bordered table-responsive">
                <a-table :columns="landingColumns" :dataSource="landingData" :pagination="true" />
              </div>
            </LadingPages>
          </sdCards>
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
    title: 'รหัส',
    dataIndex: 'pages',
    key: 'pages',
  },
  {
    title: 'ชื่อ-สกุล',
    dataIndex: 'sessions',
    key: 'sessions',
  },
  {
    title: 'ตำแหน่ง',
    dataIndex: 'rate',
    key: 'rate',
  },
  {
    title: 'แผนก',
    dataIndex: 'ctr',
    key: 'ctr',
  },
];

export default {
  name: 'DbpUsers',
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
