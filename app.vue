<template>
  <main>
    <section class="flex justify-center">
      <div
        class="w-[400px] bg-orange-200 p-5 py-10 rounded-lg m-5 border flex justify-center flex-col items-center gap-10"
      >
        <h1 class="text-3xl text-center font-bold">
          Generate QR Code
        </h1>

        <div id="qrcode"></div>
        <div class="w-full px-10 flex flex-col gap-4">
          <label class="font-medium ">Enter or paste QR code</label>
          <input
            type="text"
            v-model="qrString"
            placeholder="https://afrodev.space"
            class="rounded-[8px] p-3 border-[#A9A9A9] border w-full"
            @keyup.enter="generateQRCode"
          />
          <button
            class="w-full text-center py-3 bg-[#131313] rounded-lg text-white"
            @click="generateQRCode"
          >
            Generate
          </button>

          <div class="text-center ">
            Download As
          </div>
          <div class="flex justify-between">
            <button
              class=" p-3 border border-orange-50 font-medium rounded-lg hover:bg-orange-300"
              @click="() => saveQr('svg')"
            >
              SVG
            </button>
            <button
              class=" p-3 border border-orange-50 font-medium rounded-lg hover:bg-orange-300"
              @click="() => saveQr('png')"
            >
              PNG
            </button>
            <button
              class=" p-3 border border-orange-50 font-medium rounded-lg hover:bg-orange-300"
              @click="() => saveQr('jpg')"
            >
              JPG
            </button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
<script setup>
/* global QRCode */
const qrString = ref("");

let QRInstance;

function generateQRCode() {
  const container = document.getElementById("qrcode");
  QRInstance = new QRCode({
    content: qrString.value || "https://afrodev.space",
    padding: 4,
    width: 256,
    height: 256,
    color: "#000000",
    background: "transparent",
    ecl: "H",
    join: true,
  });
  var svg = QRInstance.svg();

  container.innerHTML = svg;
}

function saveQr(format) {
  switch (format) {
    case "svg":
      svgExport.downloadSvg(
        document.getElementsByTagName("svg")[0],
        "qrcode-svg",
        { width: 256, height: 256 }
      );
      break;
    case "png":
      svgExport.downloadPng(
        document.getElementsByTagName("svg")[0],
        "qrcode-png",
        { width: 256, height: 256 }
      );
      break;
    case "jpg":
      svgExport.downloadJpeg(
        document.getElementsByTagName("svg")[0],
        "qrcode-jpeg",
        { width: 256, height: 256 }
      );
    default:
      break;
  }
}

onMounted(() => {
  generateQRCode();
});

useHead({
  script: [
    {
      src: "https://cdn.jsdelivr.net/npm/qrcode-svg/dist/qrcode.min.js",
      type: "text/javascript",
    },
    {
      src: "https://unpkg.com/canvg@3.0.1/lib/umd.js",
      type: "text/javascript",
    },
    {
      src: "https://cdn.jsdelivr.net/npm/pdfkit@0.11.0/js/pdfkit.min.js",
      type: "text/javascript",
    },
    {
      src:
        "https://github.com/devongovett/blob-stream/releases/download/v0.1.3/blob-stream.js",
      type: "text/javascript",
    },
    {
      src: "https://cdn.jsdelivr.net/npm/svg-to-pdfkit@0.1.8/source.min.js",
      type: "text/javascript",
    },
    {
      src: "https://sharonchoong.github.io/svg-exportJS/svg-export.min.js",
      type: "text/javascript",
    },
  ],
});
</script>
