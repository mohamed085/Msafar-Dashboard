<template>
  <div class="p-3">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-4 border-bottom">
      <h1>توثيق العملاء</h1>
    </div>

    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else>
      <div class="search-inputs mb-3 mt-2 d-flex flex-wrap">

        <b-form class="search-form d-flex" @submit.prevent="search('nationality')">
          <b-form-input v-model="nationalityKey" required placeholder="بحث بالجنسية"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>

        <b-form class="search-form d-flex" @submit.prevent="search('phone')">
          <b-form-input v-model="phoneKey" required placeholder="بحث بالجوال"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>

        <b-form class="search-form d-flex" @submit.prevent="search('id')">
          <b-form-input v-model="idKey" required placeholder="بحث برقم الهوايه"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>
      </div>


      <h2 class="mt-2 mb-3">فرز</h2>
      <div class="filters d-flex flex-wrap mb-3">

        <div class="filter d-flex">
          <b-form-checkbox></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">فرز بالاحداث</span>
        </div>

        <div class="filter d-flex">
          <b-form-checkbox></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">فرز بالطلبات المرفوضة</span>
        </div>

        <div class="filter d-flex">
          <b-form-checkbox></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">فرز بالطلبات المنتظرة</span>
        </div>


      </div>

      <h2 class="mt-4 mb-3">العملاء الموجودين</h2>
      <div class="table-responsive">
        <table class="table table-hover">
          <thead>
            <tr class="bg-success text-white">
              <th scope="col">الصورة</th>
              <th scope="col">الاسم</th>
              <th scope="col">النوع</th>
              <th scope="col">نوع التعديل</th>
              <th scope="col">الجنسية</th>
              <th scope="col">تاريخ الانضمام</th>
              <th scope="col">القرار</th>
              <th scope="col">قبول التوثيق</th>
              <th scope="col">متخذ القرار</th>
              <th scope="col">وقت الطلب</th>
              <th scope="col">تحرير البيانات</th>
            </tr>
          </thead>
          <tbody>
          <tr v-for="res in responseData" :key="res.id">
              <td>
                <b-avatar :src="res.id_photo"></b-avatar>
              </td>
              <td>
                <router-link :to="'/user/' + res.id">{{ res.name }}</router-link>
              </td>
              <td>
                <span v-if="res.type === 0">عميل</span>
                <span v-if="res.type === 1">مسافر</span>
              </td>
              <td>
                <b-badge variant="warning" v-if="res.update_type == 0">بيانات</b-badge>
                <b-badge variant="danger" v-if="res.update_type == 1">توثيق</b-badge>
                <b-badge variant="primary" v-if="res.update_type == 2">بيانات وتوثيق</b-badge>
              </td>
              <td>{{ res.nationality }}</td>
              <td>-</td>
              <td>
                <b-badge variant="warning" v-if="res.admin_response == 0">لم يتم الرد</b-badge>
                <b-badge variant="success" v-if="res.admin_response == 1">تم القبول</b-badge>
                <b-badge variant="danger" v-if="res.admin_response == 2">مرفوض</b-badge>
              </td>
              <td>{{ res.updated_at }}</td>
              <td>{{ res.decision_maker }}</td>
              <td>{{ res.created_at }}</td>
              <td class="d-flex icons">
                <router-link :to="'/edit-customer-documentation/' + res.id "><i class="fas fa-user-edit"></i></router-link>
              </td>
            </tr>
          </tbody>
        </table>

      </div>

    </div>

  </div>
</template>

<script>
import store from "../../store";
import router from "../../router";

export default {
  name: "CustomerDocumentation",
  data() {
    return {
      nameKey: '',
      emailKey: '',
      nationalityKey: '',
      phoneKey: '',
      idKey: '',
      spinner: false,
      responseData: ''
    }
  },
  created() {
    if (!store.getters.isAuthenticated) {
      router.push('/login')
    }
    this.loadData()
  },
  methods: {
    async loadData() {
      this.spinner = true;

      const url = 'https://msafr.we-work.pro/api/auth/admin/get-trusted-info';

      let myHeaders = new Headers();
      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)

      let raw = JSON.stringify({
      });

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      const response = await fetch(url, requestOptions);
      const responseData = await response.json();

      if (!response.ok) {
        const error = new Error(responseData.message || 'Failed to fetch!');
        throw error;
      }

      this.responseData = responseData.data.data

      console.log(this.responseData)

      this.spinner = false;

    },

    search(type) {
      alert(type)
    },
  }
}
</script>

<style scoped>
h1, h2 {
  font-family: 'Almarai', sans-serif;
}

.search-form {
  border: 1px solid #ddd;
  padding: 0;
  border-radius: 7px;
  margin: 5px;
  background-color: #ffffff;
}

.search-form input {
  border: none;
}

.search-form button {
  padding: 0 5px;
}

.table .icons a {
  margin: 0 5px;
  color: #111;
}

.table .icons a:hover {
  color: #0a53be;
}
a {
  color: #111111;
  text-decoration: none;
  padding: 2px 5px;
}

a:hover {
  color: #198754;
  border-bottom: 1px solid #198754;
  padding: 2px 5px;
}


</style>