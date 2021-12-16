<template>
  <div class="p-3">

    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-4 border-bottom">
      <h1>تحرير البيانات الشخصية</h1>
    </div>

    <main class="container">
     <b-form class="row" @submit.prevent="save">
       <div class="col-8 mt-5">
        <b-form-group label-cols="2" label="الاسم">
          <b-form-input></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="الرقم السري">
          <b-form-input type="password"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="الايميل">
          <b-form-input type="email"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="رقم الجوال">
          <b-form-input type="tel"></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="الهوية">
          <b-form-input></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="الجنسية">
          <b-form-input></b-form-input>
        </b-form-group>

        <b-form-group label-cols="2" label="نوع العضو">
          <b-form-input></b-form-input>
        </b-form-group>
      </div>

       <div class="col-4">
         <div class="row">
           <div class="col-6">
             <div class="profile-photo photo">
               <img :src="oldProfileImageSrc">
               <input type="file" accept="image/" class="hidden" ref="fileOldImage" @change="changeOldImage">
               <b-button class="btn-sm mt-2 fas fa-plus" @click="browseOldImage" variant="dark"></b-button>
             </div>
           </div>
           <div class="col-6">
             <div class="profile-photo photo">
               <img :src="newProfileImageSrc">
               <input type="file" accept="image/" class="hidden" ref="fileNewImage" @change="changeNEwImage">
               <b-button @click="browseNewImage" class="btn-sm mt-2 fas fa-plus" variant="dark"></b-button>
               <h6 class="text-danger fw-bold mt-3">الصورة الشخصية الجديدة</h6>
             </div>
           </div>
         </div>

         <div class="row mt-5">
           <div class="col-6">
             <div class="photo">
               <img :src="oldImage">
               <input type="file" accept="image/" class="hidden" ref="fileOldImage" @change="changeOldImage">
               <b-button class="btn-sm mt-2 fas fa-plus" @click="browseOldImage" variant="dark"></b-button>
             </div>
           </div>
           <div class="col-6">
             <div class="photo">
               <img :src="newImage">
               <input type="file" accept="image/" class="hidden" ref="fileNewImage" @change="changeNEwImage">
               <b-button @click="browseNewImage" class="btn-sm mt-2 fas fa-plus" variant="dark"></b-button>
               <h6 class="text-danger fw-bold mt-3">صورة الهوية الجديدة</h6>
             </div>
           </div>
         </div>

         <div class="row mt-5">
           <div class="col-6">
             <div class="photo">
               <img :src="oldImage">
               <input type="file" accept="image/" class="hidden" ref="fileOldImage" @change="changeOldImage">
               <b-button class="btn-sm mt-2 fas fa-plus" @click="browseOldImage" variant="dark"></b-button>
             </div>
           </div>
           <div class="col-6">
             <div class="photo">
               <img :src="newImage">
               <input type="file" accept="image/" class="hidden" ref="fileNewImage" @change="changeNEwImage">
               <b-button @click="browseNewImage" class="btn-sm mt-2 fas fa-plus" variant="dark"></b-button>
               <h6 class="text-danger fw-bold mt-3">صورة الاستمارة الجديدة</h6>
             </div>
           </div>
         </div>


       </div>

       <b-button class="mt-4 mb-4 w-25" type="submit" variant="outline-primary">حفظ</b-button>

     </b-form>
    </main>

  </div>
</template>

<script>
export default {
  name: "EditCustomer",
  data() {
    return {
      oldProfileImageSrc: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAPEAAADRCAMAAAAquaQNAAAAA1BMVEX///+nxBvIAAAAR0lEQVR4nO3BMQEAAADCoPVP7WULoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABuxZIAAeHuCGgAAAAASUVORK5CYII=',
      newProfileImageSrc: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAPEAAADRCAMAAAAquaQNAAAAA1BMVEX///+nxBvIAAAAR0lEQVR4nO3BMQEAAADCoPVP7WULoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABuxZIAAeHuCGgAAAAASUVORK5CYII=',
      newImage: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAPEAAADRCAMAAAAquaQNAAAAA1BMVEX///+nxBvIAAAAR0lEQVR4nO3BMQEAAADCoPVP7WULoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABuxZIAAeHuCGgAAAAASUVORK5CYII=',
      oldImage: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAPEAAADRCAMAAAAquaQNAAAAA1BMVEX///+nxBvIAAAAR0lEQVR4nO3BMQEAAADCoPVP7WULoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABuxZIAAeHuCGgAAAAASUVORK5CYII=',
    }
  },
  methods: {
    save() {

    },
    browseOldImage() {
      this.$refs.fileOldImage.click();
    },
    changeOldImage(e) {
      this.file = e.target.files[0];
      this.$emit('input', this.file);
      let reader = new FileReader();
      reader.readAsDataURL(this.file);
      reader.onload = e => {
        this.oldProfileImageSrc = e.target.result;
      }
    },
    browseNewImage() {
      this.$refs.fileNewImage.click();
    },
    changeNEwImage(e) {
      this.file = e.target.files[0];
      this.$emit('input', this.file);
      let reader = new FileReader();
      reader.readAsDataURL(this.file);
      reader.onload = e => {
        this.newProfileImageSrc = e.target.result;
      }

    },
  }
}
</script>

<style scoped>
h1, h2 {
  font-family: 'Almarai', sans-serif;
}

.profile-photo , .profile-photo img {
  border-radius: 50%;
}

.photo {
  border: 1px solid #727272;
  height: 150px;
  position: relative;
}

.photo button {
  position: absolute;
  bottom: -15px;
  left: 16px;
  padding: 5px 10px;
  border-radius: 15px;
}

.photo img {
  width: 100%;
  height: 100%;
}

.hidden {
  display: none;
}


</style>