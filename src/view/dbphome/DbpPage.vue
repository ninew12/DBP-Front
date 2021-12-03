<template>
  <div>
    <sdPageHeader title="Blank Page">
      <template v-slot:buttons>
        <div class="page-header-actions">
          <sdCalendarButton />
          <sdExportButton />
          <sdShareButton />
          <sdButton size="small" type="primary">
            <sdFeatherIcons type="plus" size="14" />
            Add New
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
                <sdCards title="Basic Usage">
                  <BasicTable />
                </sdCards>
              </a-col>
            </a-tab-pane>
            <a-tab-pane key="2" tab="เวลาเดินเครื่อง">
              <a-col :xs="24">
                <MonthCalendar />
              </a-col>
            </a-tab-pane>
            <a-tab-pane key="3" tab="Detail"> Content of tab 3 </a-tab-pane>
          </a-tabs>
        </a-col>
      </a-row>
    </Main>
  </div>
</template>

<script>
import { Main } from '../styled';
// import { data, dataIcon, icon } from '../../demoData/tab-data.json';
import BasicTable from '../table/overview/BasicTable.vue';
import MonthCalendar from '../apps/calendar/overview/Month.vue';

export default {
  name: 'DbpPage',
  components: {
    Main,
    BasicTable,
    MonthCalendar,
  },
  // setup() {
  //   return { data, dataIcon, icon };
  // },
};
</script>
