<template>
  <div class="p-3">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-4 border-bottom">
      <h1>توثيق العميل</h1>
    </div>

    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else class="row">
      <div class="col-10">
        <div class="row">
          <div class="col-6 d-flex">
            <b-avatar size="8rem" :src="data1.photo"></b-avatar>
            <h3 class="mt-auto mb-auto ms-3">البيانات السابقة</h3>
          </div>
          <div class="col-6 d-flex">
            <b-avatar size="8rem" :src="data2.photo"></b-avatar>
            <h3 class="mt-auto mb-auto ms-3">البيانات الجديدة</h3>
          </div>
        </div>

        <div class="row mt-2">
          <div class="col-6">
            <div class="car-details">
              <div class="row m-2">
                <div class="col-4">
                  <span>رقم الهوية</span>
                </div>
                <div class="col-8">
                  <input type="text" v-model="data1.id" class="form-control" placeholder="رقم الهوية" readonly>
                </div>
              </div>
              <div class="row m-2">
                <div class="col-4">
                  <span>البريد الالكتروني</span>
                </div>
                <div class="col-8">
                  <input type="text" v-model="data1.email" class="form-control" placeholder="البريد الالكتروني" readonly>
                </div>
              </div>
              <div class="row m-2">
                <div class="col-4">
                  <span>الاسم</span>
                </div>
                <div class="col-8">
                  <input type="text" v-model="data1.name" class="form-control" placeholder="الاسم" readonly>
                </div>
              </div>
            </div>
          </div>
          <div class="col-6">
            <div class="car-details">
              <div class="row m-2">
                <div class="col-4">
                  <span>رقم الهوية</span>
                </div>
                <div class="col-8">
                  <input type="text" v-model="data2.id" class="form-control" placeholder="رقم الهوية" readonly>
                </div>
              </div>
              <div class="row m-2">
                <div class="col-4">
                  <span>البريد الالكتروني</span>
                </div>
                <div class="col-8">
                  <input type="text" v-model="data2.email" class="form-control" placeholder="البريد الالكتروني" readonly>
                </div>
              </div>
              <div class="row m-2">
                <div class="col-4">
                  <span>الاسم</span>
                </div>
                <div class="col-8">
                  <input type="text" v-model="data2.name" class="form-control" placeholder="الاسم" readonly>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="row">
          <div class="col">
            <b-textarea class="m-3" rows="4" placeholder="ملاحظات المشرف"></b-textarea>
          </div>
        </div>
      </div>

      <div class="col-2 d-flex flex-column justify-content-center align-items-center">
        <b-form>
          <b-form-group @submit.prevent="saveChanges">
            <div class="d-flex">
              <b-form-radio v-model="trust" name="some-radios" value="true" required></b-form-radio>
              <h4><b-badge class="ms-2 p-2" variant="success">قبول التوثيق</b-badge></h4>
            </div>
            <div class="d-flex mt-2">
              <b-form-radio v-model="trust" name="some-radios" value="false" required></b-form-radio>
              <h4><b-badge class="ms-2 p-2 " variant="danger">رفض التوثيق</b-badge></h4>
            </div>
          </b-form-group>

          <b-textarea rows="5" placeholder="الاسباب"></b-textarea>

          <b-form-checkbox switch v-model="accept" size="lg" class="mt-4">قبول التعديلات</b-form-checkbox>

          <b-button type="submit" class="w-100 mt-4" variant="outline-success">حفظ</b-button>
        </b-form>
      </div>
    </div>


  </div>

</template>


<script>
export default {
  name: "EditUserDocumentation",
  data() {
    return {
      data1: '',
      data2: '',
      trust: '',
      accept: '',
      spinner: false,
      status: '',
      msg: ''
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

      let raw1 = JSON.stringify({
        "user_id": id
      });

      let requestOptions1 = {
        method: 'POST',
        headers: myHeaders,
        body: raw1,
        redirect: 'follow'
      };


      let raw2 = JSON.stringify({
        "person_id": id,
        "type": 0
      });

      let requestOptions2 = {
        method: 'POST',
        headers: myHeaders,
        body: raw2,
        redirect: 'follow'
      };


      let url1 = "https://msafr.we-work.pro/api/auth/admin/get-user-info"
      let url2 = "https://msafr.we-work.pro/api/auth/admin/get-person-update-queue"

      const response1 = await fetch(url1, requestOptions1);
      const response2 = await fetch(url2, requestOptions2);

      const responseData1 = await response1.json();
      const responseData2 = await response2.json();

      this.data1 = responseData1.data;
      this.data2 = responseData2.data;

      if (!this.data1) {
        await this.$router.replace("/404")
      }

      this.spinner = false
    },

    async saveChanges() {

      this.spinner = true

      let myHeaders = new Headers();

      const token = this.$store.getters.token;

      myHeaders.append("authToken", token);
      myHeaders.append("Content-Type", "application/json");

      let raw = JSON.stringify({
        "person_id": this.$route.params.id,
        "type": 0,
        "trust": Number(this.trust),
        "accept": Number(this.accept)
      });

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
        redirect: 'follow'
      };

      let url = "https://msafr.we-work.pro/api/auth/admin/response-update-queue"

      const response = await fetch(url, requestOptions);

      const responseData = await response.json();
      this.spinner = false

      if (responseData.status) {
        this.status = true
        this.msg = 'تم إضافة الاعلان بنجاح'
      } else {
        this.status = false
        this.msg = 'لم يتم اضافة الاعلان'
      }

    },
  }
}
</script>

<style scoped>
h1, h2 {
  font-family: 'Almarai', sans-serif;
}

.hidden {
  display: none;
}

.car-photo {
  width: 150px;
  height: 150px;
}

.photo {
  border: 1px solid #727272;
  height: 200px;
  position: relative;
}

.photo-old {
  margin-right: 20px;
}

.photo-new {
  margin-left: 20px;
}

.photo img {
  width: 100%;
  height: 100%;
}

.photo button {
  position: absolute;
  bottom: -15px;
  left: 16px;
  padding: 5px 10px;
  border-radius: 15px;
}

.swap-icon {
  font-size: 150%;
  margin: 5px;
}

input:focus, textarea:focus {
  outline: none !important;
  border-color: #727272;
  box-shadow: 0 0 10px #aeaeae;
}

</style>