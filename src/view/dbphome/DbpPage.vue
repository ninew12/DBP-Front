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
        <a-col :md="12" :xs="24">
          <Tabs :data="dataIcon" color="#ffffff"></Tabs>
        </a-col>
      </a-row>
    </Main>
  </div>
</template>

<script>
import { Main } from '../styled';
import Tabs from '../../components/tabs/Tabs';
import { data, dataIcon, icon } from '../../demoData/tab-data.json';
export default {
  name: 'DbpPage',
  components: {
    Main,
    Tabs,
  },
  setup() {
    return { data, dataIcon, icon };
  },
};
</script>
