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
          <label class="font-medium ">Enter or paste url</label>
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
          <div>Set Background Color</div>
          <div class="flex flex-col justify-between">
            <div
              @click="() => setBgColor('transparent')"
              class="border w-full flex flex-col items-center justify-center text-white rounded-md h-[30px] cursor-pointer"
            >
              Transparent
            </div>
            <div
              @click="() => setBgColor('white')"
              class="bg-white border w-full flex flex-col items-center justify-center text-white rounded-md h-[30px] cursor-pointer"
            ></div>
            <div
              @click="() => setBgColor('blue')"
              class="bg-blue-900 border w-full flex flex-col items-center justify-center text-white rounded-md h-[30px] cursor-pointer"
            ></div>
          </div>

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
const bgColor = ref("transparent");
let QRInstance;

function generateQRCode() {
  QRInstance = new QRCode({
    content: qrString.value || "https://afrodev.space",
    padding: 4,
    width: 256,
    height: 256,
    color: "#000000",
    background: bgColor.value,
    ecl: "H",
    join: true,
  });
  writeCode(QRInstance);
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

function setBgColor(color) {
  if (QRInstance) {
    bgColor.value = color;
    QRInstance.options.background = color;
    writeCode(QRInstance);
  }
}

function writeCode(instance) {
  const container = document.getElementById("qrcode");
  var svg = instance.svg();
  console.log(instance);
  container.innerHTML = svg;
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
      src: "https://cdnjs.cloudflare.com/ajax/libs/canvg/3.0.9/umd.js",
      type: "text/javascript",
      integrity:
        "sha512-Wu9XXg78PiNE0DI4Z80lFKlEpLq7yGjquc0I35Nz+sYmSs4/oNHaSW8ACStXBoXciqwTLnSINqToeWP3iNDGmQ==",
      crossorigin: "anonymous",
      referrerpolicy: "no-referrer",
    },

    {
      src: "https://sharonchoong.github.io/svg-exportJS/svg-export.min.js",
      type: "text/javascript",
    },
  ],
});
</script>
