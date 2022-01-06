<template>
  <main class="pe-3 ps-3 pt-4 pb-5 mb-5">

    <div v-if="spinner" class="d-flex justify-content-center align-items-center pt-4 pb-4">
      <b-spinner variant="success" style="width: 3rem; height: 3rem;" type="grow" label="Spinning"></b-spinner>
    </div>

    <div v-else>
      <div class="box-row-1">
        <div class="box border-green">
          <span class="span-title">اعضاء مسافرين</span>
          <span class="span-count">{{ data.masafrs_count }}</span>
        </div>
        <div class="box border-red">
          <span class="span-title">اعضاء اجانب</span>
          <span class="span-count">{{ data.users_foreigners + data.masafrs_foreigners }}</span>
        </div>
        <div class="box border-red">
          <span class="span-title">إجمالي الربح</span>
          <span class="span-count"></span>
          <span class="span-title">ريال</span>
        </div>
        <div class="box border-red">
          <span class="span-title">اعضاء موثق</span>
          <span class="span-count">{{ data.masafrs_trusts + data.users_trusts }}</span>
        </div>
      </div>
      <div class="box-row-2">
        <div class="box box-big border-brown">
          <div class="box-big-row-1">
            <span class="span-title">اعضاء الموقع</span>
            <span class="span-count">{{ data.masafrs_count + data.users_count }}</span>
            <span class="span-title">عضو</span>
          </div>
          <div class="box-big-row-2">
            <span class="span-read-more">إقراء المزيد</span>
          </div>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count"></span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">رسائل الادارة</span>
          </div>
          <router-link to="/" class="box-default-row-3 bg-read-more-blue">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count">{{ data.chat_rooms }}</span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">محادثات الشات</span>
          </div>
          <router-link to="/chats" class="box-default-row-3 bg-read-more-green">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count">{{ data.trusts_request }}</span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">توثيق الهوية</span>
          </div>
          <router-link to="/customers-documentation" class="box-default-row-3 bg-read-more-blue">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
      </div>
      <div class="box-row-2">
        <div class="box box-big border-crimson">
          <div class="box-big-row-1">
            <span class="span-title">عدد الزوار اليوم</span>
            <span class="span-count"></span>
          </div>
          <div class="box-big-row-1">
            <span class="span-title">عدد الزوار الكلي</span>
            <span class="span-count"></span>
          </div>
          <div class="box-big-row-2">
            <span class="span-read-more">إقراء المزيد</span>
          </div>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count">{{ data.request_services }}</span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">الطلبات</span>
          </div>
          <router-link to="/orders" class="box-default-row-3 bg-read-more-yellow">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count">{{ data.trips }}</span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">الرحلات</span>
          </div>
          <router-link to="/trips" class="box-default-row-3 bg-read-more-blue">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count">{{ data.finished_requests }}</span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">الطلبات المنفذة</span>
          </div>
          <router-link to="/" class="box-default-row-3 bg-read-more-blue">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
      </div>
      <div class="box-row-2">
        <div class="box box-big border-blue">
        </div>

        <div class="box box-default border-blue">
          <div class="box-default-row-1">
          </div>
          <div class="box-default-row-2">
          </div>
          <router-link to="/orders" class="box-default-row-3 bg-read-more-yellow">
          </router-link>
        </div>

        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count">{{ data.canceled_requests }}</span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">طلبات الالغاء</span>
          </div>
          <router-link to="#" class="box-default-row-3 bg-read-more-blue">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>

        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count">{{ data.complains }}</span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">الشكاوي</span>
          </div>
          <router-link to="/complains" class="box-default-row-3 bg-read-more-yellow">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>

      </div>
      <div class="box-row-2">
        <div class="box box-big border-blue">
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count"></span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">عملاء لم يفعلوا الكود</span>
          </div>
          <router-link to="#" class="box-default-row-3 bg-read-more-brown">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count"></span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">تنبية إضافة رصيد</span>
          </div>
          <router-link to="#" class="box-default-row-3 bg-read-more-green">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
        <div class="box box-default border-gray">
          <div class="box-default-row-1">
            <span class="span-count"></span>
          </div>
          <div class="box-default-row-2">
            <span class="span-title">طلب سحب رصيد</span>
          </div>
          <router-link to="#" class="box-default-row-3 bg-read-more-brown">
            <span class="span-read-more">إقراء المزيد</span>
          </router-link>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "NewMainPage",
  data() {
    return {
      data: {},
      spinner: false
    }
  },
  created() {
    this.loadMainData()
  },
  methods: {
    async loadMainData() {
      this.spinner = true;

      let myHeaders = new Headers();

      const token = this.$store.getters.token;

      myHeaders.append("authToken", token)

      let requestOptions = {
        method: 'POST',
        headers: myHeaders,
        redirect: 'follow'
      };

      const response = await fetch("https://msafr.we-work.pro/api/auth/admin/main-page-info", requestOptions);

      const responseData = await response.json();

      this.data = responseData.data;

      this.spinner = false
    }
  }


}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cairo&family=Oswald&display=swap');

.box-row-1 {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 0;
  border-bottom: 4px solid #198754;
}

.box-row-1 .box {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
  width: 22%;
  margin: 0 1%;
}

.box-row-2 {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 0;
  border-bottom: 4px solid #198754;
}

.box-row-2 .box {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 160px;
}

.box-row-2 .box-big {
  flex-direction: column;
  width: 25%;
  margin: 0 0 0 10%;
}

.box-row-2 .box-big .box-big-row-2 {
  position: relative;
  bottom: -18px;
}

.box-row-2 .box-default {
  width: 18%;
  margin: 1%;
  flex-direction: column;
}

.box-row-2 .box-default .box-default-row-1 {
  margin: 15px 0 10px 0;
}

.box-row-2 .box-default .box-default-row-3 {
  width: 100%;
  display: flex;
  justify-content: center;
  color: #ffffff;
  border-bottom-right-radius: 15px;
  border-bottom-left-radius: 15px;
  margin: auto 0 0 0;
  text-decoration: none;
}

.box .span-title {
  font-family: 'Cairo', sans-serif;
  font-weight: 500;
  font-size: 20px;
}

.box .span-count {
  margin: 0 10px;
  font-weight: 400;
  font-size: 24px;
}

.box .span-read-more {
  font-family: 'Cairo', sans-serif;
  font-weight: 400;
  font-size: 20px;
  margin: 10px 0;
}

.border-green {
  border: 2px solid #198754;
  border-radius: 15px;
  color: #198754;
}

.border-red {
  border: 2px solid #a52834;
  border-radius: 15px;
  color: #373b3e;
}

.border-brown {
  background-color: saddlebrown;
  border-radius: 15px;
  color: #ffffff;
}

.border-gray {
  border: 1px solid #5c636a;
  border-radius: 15px;
  color: #000000;
}

.border-crimson {
  background-color: crimson;
  border-radius: 15px;
  color: #ffffff;
}

.bg-read-more-green {
  background-color: #006200;
}

.bg-read-more-green:hover {
  background-color: #ffffff;
  color: #006200 !important;
}

.bg-read-more-blue {
  background-color: #0d6efd;
}

.bg-read-more-blue:hover {
  background-color: #ffffff;
  color: #0d6efd !important;
}

.bg-read-more-yellow {
  background-color: #b1b100;
}

.bg-read-more-yellow:hover {
  background-color: #ffffff;
  color: #b1b100 !important;
}

.bg-read-more-brown {
  background-color: saddlebrown;
}

.bg-read-more-brown:hover {
  background-color: #ffffff;
  color: saddlebrown !important;
}

</style>