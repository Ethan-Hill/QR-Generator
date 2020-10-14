<template>
  <div id="app">
    <section>
      <div class="contianerr">
        <div class="inputsAbove">
          <h1 class="title">QR Link Generator</h1>
          <b-input id="url" v-model="url"></b-input>
          <br />
          <b-button type="is-success is-light" @click="checkString()"
            >Generate</b-button
          >
        </div>
        <div class="qr">
          <canvas id="qr"></canvas>
          <div class="inputsBelow" v-if="urlState">
            <a
              id="download"
              download="QR.jpg"
              href="#"
              @click="download_img()"
              ref="el"
              >Download</a
            >
            <b-button
              type="is-success is-light"
              style="margin:10px"
              @click="clear()"
              >Clear</b-button
            >
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
var QRCode = require("qrcode");
export default {
  name: "App",
  data() {
    return {
      url: null,
      urlState: false,
    };
  },
  methods: {
    checkString() {
      let str = this.url;
      var pattern = new RegExp(
        "^(https?:\\/\\/)?" + // protocol
        "((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|" + // domain name
        "((\\d{1,3}\\.){3}\\d{1,3}))" + // OR ip (v4) address
        "(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*" + // port and path
        "(\\?[;&a-z\\d%_.~+=-]*)?" + // query string
          "(\\#[-a-z\\d_]*)?$",
        "i"
      ); // fragment locator
      if (pattern.test(str)) {
        this.generateQRCode();
        this.urlState = true;
      } else {
        alert("Please check it is a URL");
      }
    },

    generateQRCode() {
      var canvas = document.getElementById("qr");
      QRCode.toCanvas(canvas, this.url, function(error) {
        if (error) console.error(error);
      });
    },

    download_img() {
      let el = this.$refs.el;
      var canvas = document.getElementById("qr");
      var image = canvas.toDataURL("image/jpg");
      el.href = image;
    },

    clear() {
      var canvas = document.getElementById("qr");
      const context = canvas.getContext("2d");
      context.clearRect(0, 0, canvas.width, canvas.height);
      this.urlState = false;
      this.url = null;
    },

    ID() {
      return (
        "_" +
        Math.random()
          .toString(36)
          .substr(2, 9)
      );
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}

section {
  min-height: 100vh;
  min-width: 100vh;
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
}

.containerr {
  display: flex;
  justify-content: center;
  align-items: center;
}

.inputsAbove {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin: 20px;
}

.inputsBelow {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 20px;
}
</style>
