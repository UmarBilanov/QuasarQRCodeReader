<template>
  <q-page padding class="flex flex-center rpPageContainer">
    <q-list-header align="center" style="color: #027be3; font-size: 22px;">Оплата покупки</q-list-header><br>
    <div class="q-pa-sm">
      <div class="flex row list-wrapper">
          <div rounded size="100px" class="flex col-4 items-center label-content black">
            <!--<q-avatar  font-size="82px">-->
              <!--<img src="../statics/icons/Vector.png" alt="">-->
            <!--</q-avatar>-->
            <q-item-side avatar="../statics/icons/Vector.png"></q-item-side>
          </div>
          <div class="flex col-8 justify-end items-center">
            <q-item-main>
              <q-item-tile label>{{ this.getName }}</q-item-tile><br>
              <q-item-tile sublabel>{{ this.getData }}</q-item-tile>
            </q-item-main>
          </div>
      </div><br>
      <div class="row q-mb-lg">
        <q-input type="number" float-label="Сумма перевода" class="col"/>
      </div>
      <div class="row q-mb-lg">
        <q-input type="number" float-label="Комментарий" class="col"/>
      </div>
      <div class="row q-mb-lg">
        <q-btn icon="create" color="primary" label="Submit"></q-btn>
      </div>
    </div>
    <div id="resultDiv" class="text-black row flex-center"></div>
  </q-page>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Home.vue',
  data () {
    return {
      option: '',
      getName: [],
      getData: []
    }
  },
  methods: {
    myFunction () {
      cordova.plugins.barcodeScanner.scan(
        function (result) {
          if (result.format === 'QR_CODE') {
            var resultDiv = document.getElementById('resultDiv')
            resultDiv.innerHTML = /** Personal information */
              'First name: ' + result.text + '<br>' +
              'Last name: ' + result.text[0] + '<br>' +
              'Address: ' + result.address + '<br>' +
              'Personal number: ' + result.format + '<br>'
          }
          alert('Done')
        },
        function (error) {
          alert('Scanning failed: ' + error)
        },
        {
          preferFrontCamera: true, // iOS and Android
          showFlipCameraButton: true, // iOS and Android
          showTorchButton: true, // iOS and Android
          torchOn: true, // Android, launch with the torch switched on (if available)
          saveHistory: true, // Android, save scan history (default false)
          prompt: 'Разместите код внутри рамки', // Android
          resultDisplayDuration: 500, // Android, display scanned text for X ms. 0 suppresses it entirely, default 1500
          formats: 'QR_CODE,PDF_417', // default: all but PDF_417 and RSS_EXPANDED
          orientation: 'portrait', // Android only (portrait|landscape), default unset so it rotates with the device
          disableAnimations: true, // iOS
          disableSuccessBeep: false // iOS and Android
        }
      )
    },
    loadData: function () {
      axios.get('http://192.168.0.126:3030/users?id=123asd')
        .then((response) => {
          response.data.data.map((user) => {
            this.getName = user.name
            this.getData = user.accounts[1].accNo
          })
        })
        .catch((err) => {
          console.log('err:', err)
        })
    },
    errorHandler: function (errorMessage) {
      this.$q.dialog({
        title: 'Ошибка',
        message: errorMessage
      })
    }
  },
  created () {
    this.myFunction()
  },
  mounted () {
    this.loadData()
  }
}

</script>

<style scoped>

</style>
