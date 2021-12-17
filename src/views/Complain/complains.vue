<template>
  <div>
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-4 border-bottom">
      <h1>الشكاوي</h1>
    </div>


    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else>
      <div class="table-responsive">
        <table class="table table-hover">
          <thead>
          <tr class="bg-success text-white">
            <th scope="col">العميل</th>
            <th scope="col">المسافر</th>
            <th scope="col">رقم الطلب</th>
            <th scope="col">رقم الشات</th>
            <th scope="col">سماح بمشاهدة الشكوى</th>
            <th scope="col">رقم الشكاوي</th>
            <th scope="col">مقدم الشكوى</th>
            <th scope="col">السبب</th>
            <th scope="col">الحالة</th>
            <th scope="col">القرار</th>
            <th scope="col">تاريخ الاضافة</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="res in data.data" :key="res.id">
            <td>
              <router-link :to="'/user/' + res.user.id">{{ res.user.name }}</router-link>
            </td>
            <td>
              <router-link :to="'/traveller/' + res.masafr.id">{{ res.masafr.name }}</router-link>
            </td>
            <td>{{ res.related_trip }}</td>
            <td>{{ res.related_chat }}</td>
            <td>
              <span v-if="res.status = 0"><b-badge variant="danger">لا</b-badge></span>
              <span v-if="res.status = 1"><b-badge variant="success">نعم</b-badge></span>
            </td>
            <td>{{ res.id }}</td>
            <td>{{ res.id }}</td>
            <td>{{ res.reason }}</td>
            <td>
              <span v-if="res.status = 0"><b-badge variant="danger">لا</b-badge></span>
              <span v-if="res.status = 1"><b-badge variant="success">نعم</b-badge></span>
            </td>
            <td>
              <span v-if="res.solved = 0"><b-badge variant="danger">لا</b-badge></span>
              <span v-if="res.solved = 1"><b-badge variant="success">نعم</b-badge></span>
            </td>
            <td>{{ res.created_at }}</td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "complains",
  data() {
    return {
      spinner: false,
      data: ''
    }
  },
  created() {
    if (!this.$store.getters.isAuthenticated) {
      this.$router.replace("/login")
    }
    this.loadTravellerInfo();
  },
  methods: {
    async loadTravellerInfo() {
      this.spinner = true;

      let myHeaders = new Headers();

      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)
      myHeaders.append("Content-Type", "application/json");

      let raw = JSON.stringify({});

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      let url = "https://msafr.we-work.pro/api/auth/admin/get-all-complain  "

      const response = await fetch(url, requestOptions);

      const responseData = await response.json();

      this.data = responseData.data;

      console.log(this.data)

      this.spinner = false


    }
  },
}
</script>

<style scoped>

h1, h2 {
  font-family: 'Almarai', sans-serif;
}

a {
  color: #111111;
  text-decoration: none;
  padding: 2px 5px;
}

a:hover {
  color: #ffffff;
  background-color: #198754;
  border-radius: 5px;
  padding: 2px 5px;

}
</style>