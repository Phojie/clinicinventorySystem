<template>
  <v-container grid-list-lg>
  <v-layout row wrap >
    <v-flex sm12>
      <v-card>
      <v-card-title>
        <div class="font-weight-black">Admission List </div>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="search"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="listofAdmission"
        item-key="keyIndex"
        :expand="expand"
        :search="search"
      >
        <template slot="items" slot-scope="props">
           <tr @click="props.expanded = !props.expanded">
          <td class="text-xs-"><span class="font-weight-black">{{props.item.title}}.</span> {{ props.item.firstname }} {{ props.item.lastname }}</td>
          <td class="text-xs-">{{ props.item.cnumber }}</td>
          <td class="text-xs-">{{ props.item.email }}</td>
          <td class="text-xs- font-weight-black blue--text text--darken-3">{{ props.item.date }}</td>
          <td class="">
            <v-btn @click="viewPrescription(props.item)" small  flat class="green--text textNone caption"  text-color="" >View Prescription</v-btn> 
            <v-btn @click="viewDetails(props.item)" small  flat class="green--text textNone caption"  text-color="" >View Form</v-btn> 
          </td>
          </tr>
        </template>
         <template v-slot:expand="props">
            <v-container grid-list-xs>
                 <v-layout row wrap>
                  <v-flex xs12>
                     <v-layout row wrap>
                     <v-text-field 
                        class="px-1"
                        v-model="taken.mt" label="Medicine Taken">
                     </v-text-field>
                     <v-text-field 
                        class="px-1"
                        v-model="taken.lt" label="Laboratory test taken">
                     </v-text-field>
                     <v-text-field 
                        class="px-1"
                        v-model="taken.pt" label="Procedures Taken">
                     </v-text-field>
                     <v-text-field 
                        class="px-1"
                        v-model="taken.time" label="Time">
                     </v-text-field>
                     <v-btn @click="submitTaken(props.item)" large icon color="">
                        <v-avatar
                           size="40"
                           color="blue"
                        >
                        <img src="https://img.icons8.com/ios/16000/ffffff/plus.png">
                        </v-avatar>
                     </v-btn>
                     </v-layout>
                  </v-flex>
               </v-layout>
            </v-container>
               <v-card-text style="margin-top:-20px">
                  <v-flex xs12>
                     <v-layout row wrap>
                        <v-flex xs3 >
                           <v-card width="100%">
                           <span class="pa-2 subheading blue--text"> Medicine taken</span>
                           </v-card>
                        </v-flex>
                        <v-flex xs3 >
                           <v-card width="100%">
                           <span class="pa-2 subheading blue--text"> Laboratory test taken </span>
                           </v-card>
                        </v-flex>
                        <v-flex xs3 >
                           <v-card width="100%">
                           <span class="pa-2 subheading blue--text"> Procedures taken </span>
                           </v-card>
                        </v-flex>
                        <v-flex xs2 >
                           <v-card width="100%">
                           <span class="pa-2 subheading blue--text"> Time </span>
                           </v-card>
                        </v-flex>
                        <v-flex xs1 >
                           <v-card width="100%">
                           <span class="pa-2 subheading blue--text"> Action </span>
                           </v-card>
                        </v-flex>
                     </v-layout>
                  </v-flex>
                  <v-flex xs12 v-for="(test, index) in props.item.taken" :key="index">
                     <v-layout row wrap>
                        <v-flex xs3 >
                           <v-card flat width="100%">
                           <span class="body-2 blue--text">{{test.mt}} </span>
                           </v-card>
                        </v-flex>
                        <v-flex xs3 >
                           <v-card flat width="100%">
                           <span class="body-2 blue--text"> {{test.lt}} </span>
                           </v-card>
                        </v-flex>
                        <v-flex xs3 >
                           <v-card flat width="100%">
                           <span class="body-2 blue--text">{{test.pt}} </span>
                           </v-card>
                        </v-flex>
                        <v-flex xs2 >
                           <v-card flat width="100%">
                           <span class="body-2 blue--text">{{test.time}} </span>
                           </v-card>
                        </v-flex>
                        <v-flex xs1 >
                           <v-btn  @click="deletetest(test,props.item)" flat icon color="primary" >
                              <v-icon
                              >
                              delete
                              </v-icon>
                           </v-btn>
                        </v-flex>
                     </v-layout>
                  </v-flex>
               </v-card-text>
         </template>
        <v-alert slot="no-results" :value="true" color="error" icon="warning">
          Your search for "{{ search }}" found no results.
        </v-alert>
      </v-data-table>
      </v-card>
    </v-flex>
  </v-layout>

      <v-dialog v-model="viewDialog" persistent max-width="600px">
         <v-card>
           <v-card-title class="mb-1">
            <div>
               <v-avatar tile size="50">
               <img src="https://img.icons8.com/ios/1600/1B5E20/pharmacy-filled.png">
               </v-avatar>
               <span class="headline">
               {{presDetails.fullname}}
               </span>
               </div>
            <v-spacer></v-spacer>
            <v-btn color="primary darken-2" flat @click="viewDialog =false" >Close</v-btn>
         </v-card-title>
         <v-card-text>
            <v-container grid-list-md>
                <v-data-table
                  :headers="headers2"
                  :items="presDetails.pData"
                  class="elevation-1"
                >
                  <template slot="items" slot-scope="props">
                     <td >
                          <span class="blue--text">{{props.item.Name}} </span>
                     </td>
                     <td >
                          <span class="blue--text"> {{props.item.Quantity}} </span>
                     </td>
                     <td >
                          <span class="blue--text">{{props.item.Strength}} </span>
                     </td>
                     <td >
                          <span class="blue--text">{{props.item.Description}} </span>
                     </td>
                  </template>
               </v-data-table>
            </v-container>
         </v-card-text>
       
         </v-card>
      </v-dialog>


   <v-dialog
      v-model="dialog"
      width="800"
    >
       <v-container  style="" class="mx-0 lighten-4 grey" grid-list-lg>
        <v-card-title class="mb-1">
          <span class="headline">Admission Information</span>
          <v-spacer></v-spacer>
          <v-btn color="error darken-2" flat @click="deleteAdmission" >Delete</v-btn>
          <!-- <v-btn color="success darken-2" flat @click="acceptA" >Accept</v-btn> -->
        </v-card-title>
            <v-layout wrap>
              <v-flex xs12 sm1 d-flex>
                <v-select
                  :items="['Mr','Mrs','Ms','Fr','Sr','Reve.','Dr']"
                  label="Title"
                  readonly
                  v-model="modelPatient.title"
                ></v-select>
              </v-flex>
              <v-flex xs12 sm6 md3>
                <v-text-field 
                  readonly
                  v-model="modelPatient.firstname" label="First name"></v-text-field>
              </v-flex>

              <v-flex xs12 sm6 md3>
                <v-text-field 
                  readonly
                  v-model="modelPatient.lastname" label="Last name"></v-text-field>
              </v-flex>
             
              <v-flex xs12 sm6 md5>
                <v-text-field v-model="modelPatient.email"
                  readonly
                  label="E-mail">
                </v-text-field>
              </v-flex>

              <v-flex xs12 sm6 md4>
                <v-text-field 
                  readonly
                  v-model="modelPatient.cnumber" label="Phone #"></v-text-field>
              </v-flex>

               <v-flex xs12 sm4 >
                <v-text-field 
                  readonly
                  v-model="modelPatient.address1" label="Address Line 1"></v-text-field>
              </v-flex>

              <v-flex xs12 sm4 >
                <v-text-field 
                  readonly
                  v-model="modelPatient.address2" label="Address Line 2"></v-text-field>
              </v-flex>
              <v-flex xs12 sm2 d-flex>
                <v-select
                  readonly
                  v-model="modelPatient.gender"
                  :items="['Male','Female']"
                  label="Gender"
                ></v-select>
              </v-flex>
              <v-flex xs12 sm2 >
                <v-text-field 
                  readonly
                  v-model="modelPatient.age" type="number" label="Age"></v-text-field>
              </v-flex>
              <v-flex 
                xs12 sm4 d-flex>
                <v-text-field 
                  readonly
                  label="Type of doctor selected"
                  v-model="modelPatient.typeD"
                ></v-text-field>
              </v-flex>
               <v-flex xs4   >
                <v-text-field 
                  readonly
                  v-model="modelPatient.lectedRoom" type="text" label="Room type"></v-text-field>
              </v-flex>
              <v-flex  xs4 >
                <v-text-field 
                  readonly
                  v-model="modelPatient.lectedRoomNa" type="text" label="Room name"></v-text-field>
              </v-flex>
            </v-layout>
          </v-container>
    </v-dialog>
  </v-container>
</template>

<script>
  import firebase from 'firebase'
  import { validationMixin } from 'vuelidate'
  import { required, maxLength, minLength, email } from 'vuelidate/lib/validators'
  export default {
  mixins: [validationMixin],
  validations: {
    doctorsDetailsProfile: {
    
    }
  },
  data() {
    return {
      taken: {
          mt: '',
          lt: '',
          pt: '',
          time: ''
      },
      viewDialog: false ,
      dialog: false,
      search: '',
      headers2: [
         {
            text: 'Prescribe Medicine',
            align: 'left',
            sortable: false,
            value: 'codeId'
         },
         {
            text: 'Quantity',
            align: 'left',
            sortable: false,
            value: 'Quantity'
         },
         {
            text: 'Strength',
            align: 'left',
            sortable: false,
            value: 'strength'
         },
         {
            text: 'Description',
            align: 'left',
            sortable: false,
            value: 'Description'
         },
         
      ],
      
      headers: [
        { text: 'Name', value: 'firstname', sortable: true},
        { text: 'Phone', value: 'cnumber', sortable: true},
        { text: 'Email', value: 'email', sortable: true},
        { text: 'Date', value: 'adate',sortable: true},
        { text: 'Action', value: 'lastname',sortable: true},
      ],
      headers3: [
        { text: 'Medicine taken', value: 'mt', sortable: true},
        { text: 'Laboratory test taken', value: 'lt', sortable: true},
        { text: 'Procedures taken', value: 'pt', sortable: true},
        { text: 'Time', value: 'time',sortable: true},
        { text: 'Action', value: 'ac',sortable: true},
      ],

      modelPatient: {
        title: '',
        firstname: '',
        lastname: '',
        email: '',
        cnumber:'',
        address1: '',
        address2: '',
        gender:'',
        age:'',
        typeD: '',
        doctor: '',
        date: '',
        keyIndex:'',
        lectedRoom: '',
        lectedRoomNa: '',
      },
      presDetails: {
         codeId: '',
         created: '',
         doctor: '', 
         fullname: '',
         keyIndex:'',
         pData: [],
      }
    }
  },
  computed: {
    accountDetails() {
        var obUser = JSON.parse(localStorage.getItem('accountDetails') );
        // console.log(obUser.profilePic)
        return obUser
    },
    listofAdmission() {
       var data = _.filter(this.$store.getters.listofAdmission,'doctor')
      return data
    },
    myRequest() {
      var filter = _.filter(this.listofAppointments,'doctor')
      return filter
    }
  },
  methods: {
     deleteAdmission(){
         let vm = this
         var submitA = firebase.database().ref('admissions/'+this.modelPatient.keyIndex)
         vm.dialog= false
         vm.$notify({
         group: 'bottomright',
         type: 'success',
         title: 'Deleted successfully',
         text: `Full name: ${_.capitalize(vm.modelPatient.firstname)} ${_.capitalize(vm.modelPatient.lastname)}` ,
         duration: 10000,
         })
         submitA.remove()

        
     },
      deletetest(datataken, propstaken) {
         console.log(datataken)
         let vm = this
         var submitA = firebase.database().ref('admissions/'+propstaken.keyIndex+'/taken/'+datataken.keyIndex)
         submitA.remove()
      },
     submitTaken(data) {
        let vm = this
         var newPostKey = firebase.database().ref().child('admissions/'+data.keyIndex).push().key;
         var submitA = firebase.database().ref('admissions/'+data.keyIndex+'/taken/'+newPostKey)
         submitA.update({
            keyIndex: newPostKey,
            mt:  vm.taken.mt,
            lt:  vm.taken.lt,
            pt:  vm.taken.pt,
            time:  vm.taken.time,
         })
         this.taken = {
            mt: '',
            lt: '',
            pt: '',
            time: ''
         }
     },
     viewPrescription(data) {
        var pres = []
        var fullname = data.firstname+' '+data.lastname
        var get = firebase.database().ref('prescriptions')
        get.on('value', function(data) {
           pres = data.val()
        })
        var search = _.filter(pres, ['fullname', fullname])
         console.log(search[0])
         if(search[0]) {
            this.presDetails = {
               codeId: search[0].codeId,
               created: search[0].created,
               doctor: search[0].doctor, 
               fullname: search[0].fullname,
               keyIndex: search[0].keyIndex,
               pData: search[0].pData,
            }
         } else {
            this.presDetails = {
               codeId: '',
               created: '',
               doctor: '', 
               fullname: 'No prescription given',
               keyIndex: '',
               pData: [],
            }
         }
        

         this.viewDialog = true


     },
    viewDetails(data) {
      this.dialog = true
      this.modelPatient= {
        title: data.title ,
        firstname: data.firstname ,
        lastname: data.lastname ,
        email: data.email ,
        cnumber: data.cnumber ,
        address1: data.address1 ,
        address2: data.address2 ,
        gender: data.gender ,
        age: data.age ,
        typeD:  data.typeD ,
        doctor:  data.doctor ,
        date: data.date ,
        keyIndex: data.keyIndex,
        lectedRoom: data.lectedRoom,
        lectedRoomNa: data.lectedRoomNa,
      }
    },
    acceptA() {
      let vm = this
      var newPostKey = firebase.database().ref().child('acceptedAs').push().key;
      var submitA = firebase.database().ref('acceptedAs/'+newPostKey)
      submitA.set({
        keyIndex: newPostKey,
        title: vm.modelPatient.title ,
        firstname: vm.modelPatient.firstname ,
        lastname: vm.modelPatient.lastname ,
        email: vm.modelPatient.email ,
        cnumber: vm.modelPatient.cnumber ,
        address1: vm.modelPatient.address1 ,
        address2: vm.modelPatient.address2 ,
        gender: vm.modelPatient.gender ,
        age: vm.modelPatient.age ,
        typeD:  vm.modelPatient.typeD ,
        doctor:  vm.modelPatient.doctor ,
        adate: vm.modelPatient.date ,
        status: 'Pending'
      })
      firebase.database().ref('appointments/'+vm.modelPatient.keyIndex).remove()
      vm.dialog= false
      vm.$notify({
        group: 'bottomright',
        type: 'success',
        title: 'Verified successfully',
        text: `Full name: ${_.capitalize(vm.modelPatient.firstname)} ${_.capitalize(vm.modelPatient.lastname)}` ,
        duration: 10000,
      })
    },
    declineA() {
      let vm = this
      firebase.database().ref('appointments/'+vm.modelPatient.keyIndex).remove()
      vm.dialog= false
      vm.$notify({
        group: 'bottomright',
        type: 'error',
        title: 'Decline successfully',
        text: `Full name: ${_.capitalize(vm.modelPatient.firstname)} ${_.capitalize(vm.modelPatient.lastname)}` ,
        duration: 10000,
      })
    }
  },
}
</script>

<style>

</style>
