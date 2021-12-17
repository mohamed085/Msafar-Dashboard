<template>
  <div class="p-3">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-4 border-bottom">
      <h1>المعاملات المالية والخزانة</h1>
    </div>

    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else>
      <div class="search-inputs mb-3 mt-2 d-flex flex-wrap">
        <b-form class="search-form d-flex" @submit.prevent="search('name')">
          <b-form-input v-model="phoneKey" required placeholder="بحث برقم الهاتف"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>

        <b-form class="search-form d-flex" @submit.prevent="search('id')">
          <b-form-input v-model="idKey" required placeholder="بحث برقم الهويه"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>

        <b-form class="search-form d-flex" @submit.prevent="search('date')">
          <b-form-input type="date" v-model="dateKey1" required placeholder="بحث الجميع من تاريخ"></b-form-input>
          <b-form-input type="date" v-model="dateKey2" required placeholder="إالي تاريخ"></b-form-input>
          <b-button type="submit" class="me-1 ms-1" variant="light"><i class="fas fa-search"></i></b-button>
        </b-form>
      </div>


      <div class="table-responsive" v-if="response">
        <table class="table table-hover">
          <thead>
          <tr class="bg-success text-white">
            <th scope="col">#</th>
            <th scope="col">اسم العميل</th>
            <th scope="col">تاريخ الطلب</th>
            <th scope="col">رقم الطلب</th>
            <th scope="col">اسم المسافر</th>
            <th scope="col">قيمة الفاتورة</th>
            <th scope="col">تفاصيل الفاتورة</th>
            <th scope="col">طريقة الدفع</th>
            <th scope="col">عمولة علي العميل</th>
            <th scope="col">تخفيض</th>
            <th scope="col">فك تامين</th>
            <th scope="col">يدخل بخزنة المسافر</th>
            <th scope="col">الحالة</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="res in response.data" :key="res.id">
            <td v-if="res.id">{{ res.id }}</td>
            <td>
              <span v-if="res.request_trip.request">{{ res.request_trip.request.user.name }}</span>
              <span v-else></span>
            </td>
            <td>
              <span v-if="res.request_trip.request">{{ res.request_trip.request.created_at }}</span>
              <span v-else></span>
            </td>
            <td>
              <span v-if="res.request_trip">{{ res.request_trip.request_id }}</span>
              <span v-else></span>
            </td>
            <td>
              <router-link :to="'/traveller/' + res.request_trip.trip.masafr.id">
                <span v-if="res.request_trip.trip">{{ res.request_trip.trip.masafr.name }}</span>
              </router-link>
            </td>
            <td>-</td>
            <td>
              <span v-if="res.request_trip.payment_method === '0'"><b-badge variant="danger">اونلاين</b-badge> </span>
              <span v-else-if="res.request_trip.payment_method === '1'"><b-badge variant="info">كاش</b-badge></span>
              <span v-else>-</span>
            </td>
            <td>
              <span v-if="res.request_trip.website_service">{{ res.request_trip.website_service }}</span>
            </td>
            <td>
              <span v-if="res.request_trip.discounts">{{ res.request_trip.discounts }}</span>
            </td>
            <td>
              <span v-if="res.request_trip.insurance_hold">{{ res.request_trip.insurance_hold }}</span>
            </td>
            <td>
              <span v-if="res.request_trip.insurance_hold">{{ res.request_trip.insurance_hold }}</span>
            </td>
            <td>
              <span v-if="res.request_trip.offer_status === '-1'">ملغي</span>
              <span v-else-if="res.request_trip.offer_status === '0'">غير مربوط</span>
              <span v-else-if="res.request_trip.offer_status === '1'">فعال</span>
              <span v-else-if="res.request_trip.offer_status === '2'">معلق</span>
              <span v-else-if="res.request_trip.offer_status === '3'">جاري التاكيد</span>
              <span v-else-if="res.request_trip.offer_status === '4'">جاري التنفيذ</span>
              <span v-else-if="res.request_trip.offer_status === '5'">منفذ</span>
            </td>
          </tr>
          </tbody>
        </table>
      </div>

      <div v-else>لم يتم الغثور علي بيانات</div>

    </div>

  </div>
</template>

<script>
import store from "../store";
import router from "../router";

export default {
  name: "FinancialTransactions",
  data() {
    return {
      response: '',
      phoneKey: '',
      idKey: '',
      dateKey1: null,
      dateKey2: null,
      spinner: false
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

      const url = 'https://msafr.we-work.pro/api/auth/admin/get-transactions-for-fiscal-year';

      let myHeaders = new Headers();
      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)

      let urlencoded = new URLSearchParams();

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: urlencoded,
        redirect: 'follow'
      };

      const response = await fetch(url, requestOptions);
      const responseData = await response.json();

      if (!response.ok) {
        const error = new Error(responseData.message || 'Failed to fetch!');
        throw error;
      }

      this.response = responseData

      this.spinner = false;

    },

    async search(key) {

      this.spinner = true

      let myHeaders = new Headers();
      const token = this.$store.getters.token;

      myHeaders.append("authToken", token);
      myHeaders.append("Content-Type", "application/json");

      let filterKey = 0;

      if (key === 'name') {
        filterKey = 1
      } else if (key === 'id') {
        filterKey = 2
      } else if (key === 'date') {
        filterKey = 3
      }

      let raw = JSON.stringify({
        "filter": filterKey,
        "request_id": this.idKey,
        "phone": this.phoneKey,
        "from_date": this.dateKey1,
        "to_date": this.dateKey2,
      });

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      fetch("https://msafr.we-work.pro/api/auth/admin/get-transactions-for-fiscal-year", requestOptions)
          .then(response => response.json())
          .then(result => {
            console.log(result)
            this.response = result
          })
          .catch(error => console.log('error', error));

      this.spinner = false

    }
  }

}
</script>

<style scoped>
h1, h2, h4 {
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