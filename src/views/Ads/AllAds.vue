<template>
  <div>
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-4 border-bottom">
      <h1>الاعلانات</h1>
    </div>


    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else>
      <div class="table-responsive">
        <table class="table table-hover">
          <thead>
          <tr class="bg-success text-white">
            <th scope="col">الرسالة</th>
            <th scope="col">رابط الاعلان</th>
            <th scope="col">الموقع بعد الاعلان</th>
            <th scope="col">كل الايام</th>
            <th scope="col">مدة الظهور</th>
            <th scope="col">تكرار يومي</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="res in data" :key="res.id">
            <td>{{ res.subject }}</td>
            <td>{{ res.link }}</td>
            <td>{{ res.site_after_announcement }}</td>
            <td>
              <span v-if="res.all_days = 0">لا</span>
              <span v-else-if="res.all_days = 1">نعم</span>
              <span v-else>-</span>
            </td>
            <td>{{ res.appear_time }}</td>
            <td>{{ res.daily_repeat }}</td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "AllAds",
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
    this.loadTravellerInfo(this.$route.params.id);
  },
  methods: {
    async loadTravellerInfo(id) {
      this.spinner = true;

      let myHeaders = new Headers();

      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)
      myHeaders.append("Content-Type", "application/json");

      let raw = JSON.stringify({
        "masafr_id": id
      });

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      let url = "https://msafr.we-work.pro/api/auth/admin/get-all-advertisings"

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
</style>