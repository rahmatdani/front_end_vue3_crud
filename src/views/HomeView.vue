<template>
  <div>
    <input type="text" v-model="form.nama"><br>
    <input type="text" v-model="form.kelas"><br>
    <button type="botton" @click="simpan">simpan</button>
    {{info}}
    <br> <br>
    <table border="1" width="500">
      <thead>
        <tr>
          <td>No</td>
          <td>Nama</td>
          <td>Kelas</td>
          <td>aksi</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in daridb" :key="index++" >
          <td>{{index}}</td>
          <td>{{row.nama}}</td>
          <td>{{row.kelas}}</td>
          <td><button type="botton" @click="hapus(row.id)">Hapus</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { reactive, ref } from '@vue/reactivity'
import axios from 'axios'
export default {
  setup () {
    const form=reactive({
    nama:"",
    kelas:"",
    })
    return {
      data,
      saya: ref('rahmatdani'),
      laravel: ref(""),
      form,
      info:"",
      daridb:ref([])
    }
  },
  methods:{
  async getData(){
    await axios.get("http://192.168.101.17:5000/api/siswa").then((res)=>{
      this.laravel=res.data
    })
  },
  async ambilData(){
    await axios.get("http://192.168.101.17:5000/api/siswa/get").then((res)=>{
      this.daridb=res.data
      // console.log(res.data)
    })
  },
  async simpan(){
    await axios.post("http://192.168.101.17:5000/api/siswa",this.form).then((res)=>{
      this.form.nama=""
      this.form.kelas=""
      this.info="data berhasil di simpan"
      this.daridb  // di masukkan ke sini lagi untuk menampilkan data secara real time
      return res
    })
  },
  async hapus($id){
    await axios.delete("http://192.168.101.17:5000/api/siswa/hapus/"+$id).then((res)=>{
      this.ambilData()
      return res
    })
  }
  
},
created(){
  this.getData(),
  this.ambilData()
}
}



</script>


