<template>
  <div class="card">
    <div class="card-header">
      <i class="fa fa-table" aria-hidden="true"></i> Edit Master Prestasi

      <ul class="nav nav-pills card-header-pills pull-right">
        <li class="nav-item">
          <button class="btn btn-primary btn-sm" role="button" @click="back">
            <i class="fa fa-arrow-left" aria-hidden="true"></i>
          </button>
        </li>
      </ul>
    </div>

    <div class="card-body">
      <vue-form class="form-horizontal form-validation" :state="state" @submit.prevent="onSubmit">

        <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
            <label for="jenis_prestasi">Jenis Prestasi</label>
            <v-select name="jenis_prestasi" v-model="model.jenis_prestasi" :options="jenis_prestasi" class="mb-4"></v-select>

            <field-messages name="jenis_prestasi" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Jenis Prestasi is a required field</small>
            </field-messages>
            </validate>
          </div>
        </div>

         <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
            <label for="juara">Juara</label>
            <v-select v-model="model.juara" :options="optionsJuara" class="mb-4"></v-select>

            <field-messages name="juara" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Juara is a required field</small>
            </field-messages>
            </validate>
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
            <label for="tingkat">Tingkat</label>
            <v-select v-model="model.tingkat" :options="optionsTingkat" class="mb-4"></v-select>

            <field-messages name="tingkat" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Tingkat is a required field</small>
            </field-messages>
            </validate>
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
              <label for="model-nilai">Nilai</label>
              <input class="form-control" v-model="model.nilai" required autofocus name="nilai" type="text" placeholder="Nilai">

              <field-messages name="nilai" show="$invalid && $submitted" class="text-danger">
                <small class="form-text text-success">Looks good!</small>
                <small class="form-text text-danger" slot="required">Nilai is a required field</small>
              </field-messages>
            </validate>
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
              <label for="model-kode">Kode Prestasi</label>
              <input class="form-control" v-model="model.kode" required autofocus name="kode" type="text" placeholder="Kode Prestasi">

              <field-messages name="Kode Prestasi" show="$invalid && $submitted" class="text-danger">
                <small class="form-text text-success">Looks good!</small>
                <small class="form-text text-danger" slot="required">Kode Prestasi is a required field</small>
              </field-messages>
            </validate>
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">
            <validate tag="div">
            <label for="user_id">Username</label>
            <v-select name="user_id" v-model="model.user" :options="user" class="mb-4"></v-select>

            <field-messages name="user_id" show="$invalid && $submitted" class="text-danger">
              <small class="form-text text-success">Looks good!</small>
              <small class="form-text text-danger" slot="required">Username is a required field</small>
            </field-messages>
            </validate>
          </div>
        </div>

        <div class="form-row mt-4">
          <div class="col-md">
            <button type="submit" class="btn btn-primary">Submit</button>

            <button type="reset" class="btn btn-secondary" @click="reset">Reset</button>
          </div>
        </div>

      </vue-form>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    axios.get('api/master-prestasi/' + this.$route.params.id + '/edit')
      .then(response => {
        if (response.data.status == true) {
          this.model.user = response.data.user,
          this.model.jenis_prestasi = response.data.jenis_prestasi;
          this.model.juara = response.data.master_prestasi.juara;
          this.model.kode = response.data.master_prestasi.kode;
          this.model.tingkat = response.data.master_prestasi.tingkat;
          this.model.nilai = response.data.master_prestasi.nilai;
        } else {
          alert('Failed');
        }
      })
      .catch(function(response) {
        alert('Break');
        window.location.href = '#/admin/master-prestasi/';
      }),

      axios.get('api/master-prestasi/create')
      .then(response => {
        response.data.jenis_prestasi.forEach(element => {
            this.jenis_prestasi.push(element);
          });
          if(response.data.user_special == true){
            response.data.user.forEach(user_element => {
              this.user.push(user_element);
            });
          }else{
            this.user.push(response.data.user);
          }
      })
      .catch(function(response) {
        alert('Break');
        window.location.href = '#/admin/master-prestasi/';
      })
  },
  data() {
    return {

      optionsJuara: [
        {id: 1, label: 'Juara 1'},
        {id: 2, label: 'Juara 2'},
        {id: 3, label: 'Juara 3'},
        {id: 4, label: 'Juara Harapan 1'},
      ],
      selectedJuara: {id: "-", label: 'Pilih Salah Satu'},

      optionsTingkat: [
        {id: 1, label: 'Tingkat Internasional'},
        {id: 2, label: 'Tingkat Nasional'},
        {id: 3, label: 'Tingkat Provinsi'},
        {id: 4, label: 'Tingkat Kabupaten/Kota'},
      ],
      selectedTingkat: {id: "-", label: 'Pilih Salah Satu'},

      state: {},
      model: {
        user: "",
        jenis_prestasi: "",
        juara: "",
        tingkat: "",
        nilai: "",
        kode: ""
      },
      user: [],
      jenis_prestasi: []
    }
  },
  methods: {
    onSubmit: function() {
      let app = this;

      if (this.state.$invalid) {
        return;
      } else {
        axios.put('api/master-prestasi/' + this.$route.params.id, {
            user_id: this.model.user.id,
            jenis_prestasi_id: this.model.jenis_prestasi.id,
            juara: this.model.juara.id,
            tingkat: this.model.tingkat.id,
            nilai: this.model.nilai,
            kode: this.model.kode
          })
          .then(response => {
            if (response.data.status == true) {
              if(response.data.message == 'success'){
                alert(response.data.message);
                app.back();
              }else{
                alert(response.data.message);
              }
            } else {
              alert(response.data.message);
            }
          })
          .catch(function(response) {
            alert('Break ' + response.data.message);
          });
      }
    },
    reset() {
      axios.get('api/master-prestasi/' + this.$route.params.id + '/edit')
        .then(response => {
          if (response.data.status == true) {
            this.model.kode = response.data.master_prestasi.kode;
            this.model.nilai = response.data.master_prestasi.nilai;
          } else {
            alert('Failed');
          }
        })
        .catch(function(response) {
          alert('Break ');
        });
    },
    back() {
      window.location = '#/admin/master-prestasi/';
    }
  }
}
</script>
