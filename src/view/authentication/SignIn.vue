<template>
  <AuthWrapper>
    <!-- <p class="auth-notice">Don&rsquo;t have an account? <router-link to="/auth/register">Sign up now</router-link></p> -->
    <div class="auth-contents">
      <a-form layout="vertical">
        <sdHeading as="h3" style="text-align: center">
          <img :src="require('@/static/img/logo-dbp.png')" alt="" width="128" height="128" />
        </sdHeading>
        <a-form-item name="username" label="Username">
          <a-input type="text" v-model:value="username" placeholder="Username" />
        </a-form-item>
        <a-form-item name="password" initialValue="123456" label="Password">
          <a-input type="password" v-model:value="password" placeholder="Password" />
        </a-form-item>
        <div class="auth-form-action">
          <a-checkbox @change="onChange">Keep me logged in</a-checkbox>
        </div>
        <a-form-item style="text-align: center">
          <sdButton @click="postData" class="btn-signin" htmlType="submit" type="primary" size="large">
            {{ isLoading ? 'Loading...' : 'Sign In' }}
          </sdButton>
        </a-form-item>
      </a-form>
    </div>
  </AuthWrapper>
</template>
<script>
import { AuthWrapper } from './style';
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';
import axios from 'axios';
const url = process.env.VUE_APP_BASE_URL;
const SignIn = {
  name: 'SignIn',
  components: { AuthWrapper },
  setup() {
    const { dispatch } = useStore();
    const router = useRouter();
    const handleSubmit = () => {
      router.push('/');
      dispatch('login');
    };
    return {
      handleSubmit,
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
