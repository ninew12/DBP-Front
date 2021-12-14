<template>
  <AuthWrapper>
    <div class="auth-contents">
      <sdCards style="width: 29%">
        <a-form layout="vertical" @submit="submitForm">
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
          <span v-if="v$.username.$error" style="color: red"> Username is required </span>
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
          <span v-if="v$.password.$error" style="color: red"> Password is required </span>
          <div style="margin-top: 20px" class="auth-form-action">
            <a-checkbox @change="onChange">Keep me logged in</a-checkbox>
          </div>
          <a-form-item style="text-align: center">
            <sdButton class="btn-signin" htmlType="submit" type="primary" size="large">
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
import useValidate from '@vuelidate/core';
import { required, minLength } from '@vuelidate/validators';
import { Notification } from 'ant-design-vue';
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
    const onChange = checked => {
      checked.value = checked;
    };
    return {
      handleSubmit,
      focus,
      handleChange,
      onChange,
      value1: ref('โรงงาน'),
    };
  },
  data() {
    return {
      v$: useValidate(),
      username: '',
      password: '',
    };
  },
  validations() {
    return {
      username: { required },
      password: { required, minLength: minLength(4) },
    };
  },
  methods: {
    fortmatResponse(res) {
      return JSON.stringify(res, null, 2);
    },
    submitForm() {
      this.v$.$validate(); // checks all inputs
      if (!this.v$.$error) {
        this.postData();
      } else {
        console.log('Form failed validation');
      }
    },
    async postData() {
      const postData = {
        username: this.username,
        password: this.password,
      };

      try {
        let uri = `${url}/api/v4/login`;
        axios
          .post(uri, postData)
          .then(response => {
            console.log(response);
            this.handleSubmit();
            this.openNotificationWithIcon('success', 'Login Success', 'bottomRight');
          })
          .catch(error => {
            console.log(error);
            this.openNotificationWithIcon('error', 'กรุณาใส่ Usernane หรือ Password ให้ถูกต้อง', 'bottomRight');
          });
      } catch (err) {
        console.log(err);
      }
    },
    openNotificationWithIcon(type, msg, placement) {
      Notification[type]({
        message: `Login ${type}`,
        description: msg,
        placement,
      });
    },
  },
};

export default SignIn;
</script>
