<template>
  <div class="p-3">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-4 border-bottom">
      <h1>اضافة كوبون جديد</h1>
    </div>

    <b-form @submit.prevent="addNewCoupon">
      <b-form-group>

        <div class="d-flex mb-3">
          <b-form-radio v-model="coupon.type" size="lg" value="A"></b-form-radio>
          <h4 class="mt-auto mb-auto ms-2">خصم</h4>
        </div>

        <b-form-group label-cols="2" label="اسم الكوبون">
          <b-form-input v-model="coupon.name"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="الكمية">
          <b-form-input v-model="coupon.quantity" type="number"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="قيمة الخصم">
          <b-form-input v-model="coupon.discount" type="number"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="رسالة الخطأ (اسم الكود موجود)">
          <b-form-input v-model="coupon.errorCodeAlreadyExist"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="رسالة الخطأ (اسم الكود غير موجود)">
          <b-form-input v-model="coupon.errorCodeNotExist"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="رسالة التكرار">
          <b-form-input v-model="coupon.repeatError"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="اسم الفوز">
          <b-form-input v-model="coupon.winMessage"></b-form-input>
        </b-form-group>

        <div class="row g-3">
          <div class="col">
            <b-form-group label-cols="3" label="تاريخ بداية الكوبون">
              <b-form-input type="date" v-model="coupon.startDate"></b-form-input>
            </b-form-group>
          </div>
          <div class="col">
            <b-form-group label-cols="3" label="تاريخ انتهاء الكوبون">
              <b-form-input type="date" v-model="coupon.endDate"></b-form-input>
            </b-form-group>
          </div>
        </div>

        <div class="d-flex mt-5 mb-3">
          <b-form-radio v-model="coupon.type" size="lg" value="B"></b-form-radio>
          <h4 class="mt-auto mb-auto ms-2">هدية</h4>
        </div>

        <b-form-group label-cols="2" label="مخصص لرقم">
          <b-form-input v-model="coupon.name"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="لقد حصلت علي">
          <b-form-input v-model="coupon.name"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="رابط">
          <b-form-input v-model="coupon.name"></b-form-input>
        </b-form-group>

      </b-form-group>

      <b-button class="mt-4 mb-4 w-25" type="submit" variant="outline-success">حفظ الكوبون</b-button>
    </b-form>

  </div>
</template>

<script>
export default {
  name: "AddNewCoupon",
  data() {
    return {
      coupon: {
        type: '',
        name: '',
        quantity: 0,
        discount: 0.0,
        errorCodeAlreadyExist: 'نعتذر منك استخدام كامل العددواصبح الخصم غير متوفر',
        errorCodeNotExist: 'نعتذر منك هذا الكود غير صحيح',
        repeatError: 'نعتذر منك لقد استخدمتم كود سابق',
        winMessage: 'مبروك حصولك علي خصم 10 ريال',
        startDate: null,
        endDate: null,
      }
    }
  },
  methods: {
    addNewCoupon() {

    }
  }
}
</script>

<style scoped>
h1, h2, h4 {
  font-family: 'Almarai', sans-serif;
}

</style>