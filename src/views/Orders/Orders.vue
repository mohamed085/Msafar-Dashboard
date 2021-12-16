<template>
  <div class="all-orders-section p-3">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
      <h1>اقسام الطلبات</h1>
    </div>

    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else>
      <div class="search-inputs mb-3 mt-2 d-flex flex-wrap">
        <b-form class="search-form d-flex" @submit.prevent="search('name')">
          <b-form-input v-model="idKey" required placeholder="بحث بالاسم"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>

        <b-form class="search-form d-flex" @submit.prevent="search('phone')">
          <b-form-input v-model="phoneKey" required placeholder="بحث بالجوال"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>

        <b-form class="search-form d-flex" @submit.prevent="search('city')">
          <b-form-input v-model="fromCityKey" required placeholder="من مدينة"></b-form-input>
          <b-form-input v-model="toCityKey" required placeholder="الي مدينة"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>
      </div>

      <h2 class="mt-2 mb-3">فرز</h2>
      <div class="filters d-flex flex-wrap mb-3">
        <div class="filter d-flex">
          <b-form-checkbox size="lg"></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">الملغي</span>
        </div>

        <div class="filter d-flex">
          <b-form-checkbox size="lg"></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">جاري التاكيد</span>
        </div>

        <div class="filter d-flex">
          <b-form-checkbox size="lg"></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">جاري التنفيذ</span>
        </div>

        <div class="filter d-flex">
          <b-form-checkbox size="lg"></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">معلق</span>
        </div>

        <div class="filter d-flex">
          <b-form-checkbox size="lg"></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">تم التنفيذ</span>
        </div>

        <div class="filter d-flex">
          <b-form-checkbox size="lg"></b-form-checkbox>
          <span class="me-2 ms-2 mt-auto mb-auto">مبتهي</span>
        </div>

      </div>


      <div class="table-responsive">
        <table class="table  table-hover">
          <thead>
          <tr class="bg-success text-white">
            <th scope="col">#</th>
            <th scope="col">صورة العميل</th>
            <th scope="col">العميل</th>
            <th scope="col">الوصف</th>
            <th scope="col">رقم الطلب</th>
            <th scope="col">تاريخ الاضافة</th>
            <th scope="col">من</th>
            <th scope="col">الي</th>
            <th scope="col">حالة الطلب</th>
            <th scope="col">مطلوب تامين</th>
            <th scope="col">تخفيض</th>
            <th scope="col">اموال معلقة</th>
            <th scope="col">نهاية الرحلة</th>
            <th scope="col">الاستجابة علي الطلب</th>
            <th scope="col">المسافر المنفذ</th>
            <th scope="col"></th>
          </tr>
          </thead>
          <tbody>
            <tr v-for="res in response.data" :key="res.id">
              <td></td>
              <td>
                <img :src="res.photo" v-if="res.photo"/>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.user">{{ res.user.name }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.description">{{ res.description }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.id">{{ res.id }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.created_at">{{ res.created_at }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.to_place">{{ res.to_place }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.request_trip">{{ res.request_trip.offer_status }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.have_insurance">{{ res.have_insurance }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.request_trip">{{ res.request_trip.discounts }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.request_trip">{{ res.request_trip.insurance_hold }}</span>
                <span v-else>-</span>
              </td>
              <td>
                <span v-if="res.max_day">{{ res.max_day }}</span>
                <span v-else>-</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="btn-group me-2">
        <button type="button" class="btn btn-outline-success">حذف المحدد غير مرتبط بكود</button>
        <button type="button" class="btn btn-outline-success">إرسال SMS</button>
        <button type="button" class="btn btn-outline-success">إرسال بريد</button>
        <button type="button" class="btn btn-outline-success">إرسال إشعار</button>
        <button type="button" class="btn btn-outline-success">إرسال نوافذ</button>
        <button type="button" class="btn btn-outline-success">حذف الطلبات المنتهية أكثر من شهر غير مرتبطة</button>
      </div>

    </div>


  </div>
</template>

<script>
import store from "../../store";
import router from "../../router";

export default {
  name: "Orders",
  data() {
    return {
      idKey: '',
      phoneKey: '',
      fromCityKey: '',
      toCityKey: '',
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

      const url = 'https://msafr.we-work.pro/api/auth/admin/get-trips';

      let myHeaders = new Headers();
      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)

      let raw = JSON.stringify({
        "filter": '',
        "id": '',
        "phone": this.phoneKey,
        "from_place": this.fromCityKey,
        "to_place": this.toCityKey,
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

      this.response = responseData.data

      console.log(responseData)
      console.log(this.response)

      this.spinner = false;

    },
    async search(key) {
      this.spinner = true

      let filterKey;

      if (key === 'request_id') {
        filterKey = 1
      } else if (key === 'trip_id') {
        filterKey = 2
      } else if (key === 'phone') {
        filterKey = 3
      } else if (key === 'place') {
        filterKey = 4
      }

      let myHeaders = new Headers();

      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)
      myHeaders.append("Content-Type", "application/json");

      let raw = {
        "filter": filterKey,
        "trip_id": this.trip_id,
        "phone": this.phone,
        "from_place": this.from_place,
        "to_place": this.to_place,
      }

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      const response = await fetch("https://msafr.we-work.pro/api/auth/admin/get-trips", requestOptions);

      const responseData = await response.json();

      if (!response.ok) {
        const error = new Error(responseData.message || 'Failed to fetch!');
        throw error;
      }

      this.response = responseData.data

      this.spinner = false

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

</style>