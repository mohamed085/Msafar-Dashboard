<template>
  <div class="pb-5">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
      <h1>الكوبونات</h1>
      <div class="btn-toolbar mb-2 mb-md-0">
        <div class="btn-group me-2">
          <button type="button" class="btn btn-sm btn-outline-secondary">مشاركة <i class="far fa-share-square"></i></button>
          <button type="button" class="btn btn-sm btn-outline-secondary">تصدير <i class="fas fa-file-download"></i></button>
        </div>
      </div>
    </div>

    <h2 class="mt-4 mb-3">الكوبونات الموجودة</h2>
    <button @click="addNewCoupon" type="button" class="btn btn-sm btn-outline-success mb-3"><i class="far fa-plus"></i> إضافة كوبون جديد</button>


    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else class="table-responsive">
      <table class="table table-hover">
        <thead>
          <tr class="bg-success text-white">
            <th scope="col">صورة الهدية</th>
            <th scope="col">اسم الكوبون</th>
            <th scope="col">تاريخ بداية الكوبون</th>
            <th scope="col">تاريخ نهاية الكوبون</th>
            <th scope="col">الكمية</th>
            <th scope="col">القيمة</th>
            <th scope="col">نوع الكوبون</th>
            <th scope="col">الحاصلين</th>
            <th scope="col">تاريخ الإضافة</th>
            <th scope="col">تعديل</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="res in response.data.data" :key="res.id">
            <td>
              <b-avatar :src="res.gift_picture"></b-avatar>
            </td>
            <td>{{ res.copon_name }}</td>
            <td>{{ res.copon_start_date }}</td>
            <td>{{ res.copon_end_date }}</td>
            <td>{{ res.amount }}</td>
            <td>{{ res.value }}</td>
            <td>{{ res.copon_type }}</td>
            <td>{{ res.used }}</td>
            <td>{{ res.created_at }}</td>
            <td class="d-flex icons">
              <router-link to=""><i class="fas fa-edit"></i></router-link> |
              <router-link to=""><i class="fas fa-times"></i></router-link>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="btn-group me-2">
      <button type="button" class="btn btn-sm btn-outline-success">حذف المحدد من هنا لكن الحاصلين عليه يبقون بالسجلات ما ينحذفو</button>
    </div>

  </div>
</template>

<script>
import router from "../../router";
import store from "../../store";

export default {
  name: "Coupons",
  data() {
    return {
      spinner: false,
      response: ''
    }
  },
  created() {
    if (!store.getters.isAuthenticated) {
      router.push('/login')
    }
    this.loadData()
  },
  methods:{
    async loadData() {
      this.spinner = true;

      const url = 'https://msafr.we-work.pro/api/auth/admin/get-all-copons';

      let myHeaders = new Headers();
      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        redirect: 'follow'
      };

      const response = await fetch(url, requestOptions);
      const responseData = await response.json();


      if (!response.ok) {
        const error = new Error(responseData.message || 'Failed to fetch!');
        throw error;
      }

      this.response = responseData

      console.log(responseData)
      console.log(this.response)

      this.spinner = false;

    },
    async search() {

    },
    addNewCoupon() {
      router.push('/add-new-coupon')
    }
  }
}
</script>

<style scoped>
h1, h2 {
  font-family: 'Almarai', sans-serif;
}

.table .icons a {
  margin: 0 5px;
  color: #111;
}

.table .icons a:hover {
  color: #0a53be;
}

</style>