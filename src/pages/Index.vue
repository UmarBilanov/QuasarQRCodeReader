<template>
  <div class="text-white row flex-center rpPageContainer">
    <q-page class="flex flex-center">
      <div class="text-white row flex-center">
        <img alt="Quasar logo" src="../statics/icons/Vector.png">
        <q-btn icon="phonelink_ring" color="primary" label="Fake QR code"
               @click.native="scan()"></q-btn>
        <hr>
        <!--<router-link to="/home">-->
          <!--<img class="flex col-2 add-icon transparent-btn" src="../assets/sad.svg"/>-->
        <!--</router-link>-->
        <q-btn icon="phonelink_ring" color="primary" label="Scan QR code" @click="$router.push('/home')"></q-btn>
      </div>
      <div id="resultDiv" class="text-white row flex-center"></div>
    </q-page>
  </div>
</template>

<style>
</style>

<script>
export default {
  name: 'PageIndex',
  methods: {

    scan () {
      cordova.plugins.barcodeScanner.scan(
        function (result) {
          // alert("We got a barcode\n" +
          //     "Result: " + result.text + "\n" +
          //     "Format: " + result.format + "\n" +
          //     "Cancelled: " + result.cancelled);

          var value = result.text

          var data = localStorage.getItem('LocalData')
          console.log(data)
          data = JSON.parse(data)
          data[data.length] = [value]

          localStorage.setItem('LocalData', JSON.stringify(data))

          document.getElementById('resultDiv').val = 'Test'
          document.getElementById('resultDiv').innerHTML = /** Personal information */
            'First name: ' + result.text + '<br>' +
            'Last name: ' + result.text[0] + '<br>' +
            'Address: ' + result.format + '<br>' +
            'Personal number: ' + result.format + '<br>'

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
          prompt: 'Place a barcode inside the scan area', // Android
          resultDisplayDuration: 500, // Android, display scanned text for X ms. 0 suppresses it entirely, default 1500
          formats: 'QR_CODE,PDF_417', // default: all but PDF_417 and RSS_EXPANDED
          orientation: 'portrait', // Android only (portrait|landscape), default unset so it rotates with the device
          disableAnimations: true, // iOS
          disableSuccessBeep: false // iOS and Android
        }
      )
    }
  }
}
</script>
