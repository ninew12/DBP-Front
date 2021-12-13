<template>
  <AuthWrapper>
    <div class="auth-contents">
      <sdCards style="width: 29%">
        <a-form layout="vertical">
          <sdHeading as="h3" style="text-align: center">
            <img :src="require('@/static/img/dbplogo2.png')" alt="" width="164" height="164" />
          </sdHeading>
          <SelectWrapperStyle>
            <a-select v-model:value="value1" style="width: 420px" @focus="focus" ref="select" @change="handleChange">
              <a-select-option value="โรงงาน">โรงงาน FC</a-select-option>
              <a-select-option value="Yiminghe">yiminghe</a-select-option>
            </a-select>
          </SelectWrapperStyle>
          <a-input
            style="margin-top: 20px"
            type="text"
            v-model:value="username"
            name="username"
            label="Username"
            placeholder="input Username"
          >
            <template #prefix>
              <sdFeatherIcons type="mail" size="14" />
            </template>
          </a-input>
          <a-input-password
            style="margin-top: 20px"
            name="password"
            v-model:value="password"
            placeholder="with input password"
          >
            <template #prefix>
              <sdFeatherIcons type="lock" size="14" />
            </template>
          </a-input-password>
          <div style="margin-top: 20px" class="auth-form-action">
            <a-checkbox @change="onChange">Keep me logged in</a-checkbox>
          </div>
          <a-form-item style="text-align: center">
            <sdButton @click="postData" class="btn-signin" htmlType="submit" type="primary" size="large">
              {{ isLoading ? 'Loading...' : 'Sign In' }}
            </sdButton>
          </a-form-item>
        </a-form>
      </sdCards>
    </div>
  </AuthWrapper>
</template>
<script>
import { AuthWrapper } from './style';
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';
import { ref } from 'vue';
import { SelectWrapperStyle } from '../uiElements/ui-elements-styled';
import axios from 'axios';
const url = process.env.VUE_APP_BASE_URL;
const SignIn = {
  name: 'SignIn',
  components: {
    AuthWrapper,
    SelectWrapperStyle,
  },
  setup() {
    const { dispatch } = useStore();
    const router = useRouter();
    const handleSubmit = () => {
      router.push('/');
      dispatch('login');
    };
    const focus = () => {
      console.log('focus');
    };
    const handleChange = value => {
      console.log(`selected ${value}`);
    };
    return {
      handleSubmit,
      focus,
      handleChange,
      value1: ref('โรงงาน'),
    };
  },
  data() {
    return {
      username: '',
      password: '',
    };
  },
  methods: {
    fortmatResponse(res) {
      return JSON.stringify(res, null, 2);
    },

    async postData() {
      const postData = {
        username: this.username,
        password: this.password,
      };
      console.log(postData);
      try {
        let uri = `${url}/api/v4/login`;
        axios.post(uri, postData).then(response => {
          console.log(response);
          this.handleSubmit();
        });
        // const result = {
        //   status: response.status + '-' + response.statusText,
        //   headers: response.headers,
        //   data: response.data,
        // };

        // this.postResult = this.fortmatResponse(result);
      } catch (err) {
        this.postResult = this.fortmatResponse(err.response?.data) || err;
      }
    },
  },
};

export default SignIn;
</script>
