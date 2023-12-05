<template>
  <div class="pdf-container"></div>
</template>

<script>
export default {
  name: "PSPDFKit",
  props: {
    pdfFile: {
      type: String,
      required: true,
    },
  },
  PSPDFKit: null,
  mounted() {
    this.loadPSPDFKit().then((instance) => {
      this.$emit("loaded", instance);
    });
  },
  watch: {
    pdfFile(val) {
      if (val) {
        this.loadPSPDFKit();
      }
    },
  },
  methods: {
    async loadPSPDFKit() {
      import("pspdfkit")
        .then((PSPDFKit) => {
          this.PSPDFKit = PSPDFKit;
          PSPDFKit.unload(".pdf-container");
          return PSPDFKit.load({
            document: this.pdfFile,
            container: ".pdf-container",
            baseUrl: "http://localhost:3000/",
            styleSheets: [
              "http://localhost:3000/navbar.css", // hosted CSS file
              "./navbar.css" // or local CSS file
            ]
          });
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
@import '~/assets/navbar.css';

.pdf-container {
  height: 100vh;
}
</style>
