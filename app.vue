<template>
  <main>
    <section class="flex justify-end">
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
            placeholder="https://afrodev.space"
            class="rounded-[8px] p-3 border-[#A9A9A9] border w-full"
          />
          <button
            class="w-full text-center py-3 bg-[#131313] rounded-lg text-white"
          >
            Generate
          </button>
        </div>
      </div>
    </section>
  </main>
</template>
<script setup>
/* global QRCode */

function generateQRCode(string) {
  const container = document.getElementById("qrcode");
  var qr = new QRCode({
    content: string,
    padding: 4,
    width: 256,
    height: 256,
    color: "#000000",
    background: "#ffffff",
    ecl: "H",
    join: true,
  });
  var svg = qr.svg();

  container.innerHTML = svg;

  // new QRCode(document.getElementById("qrcode"), string);
}

onMounted(() => {
  generateQRCode("https://afrodev.space");
});

useHead({
  script: [
    {
      src: "https://cdn.jsdelivr.net/npm/qrcode-svg/lib/qrcode.js",
      type: "text/javascript",
    },
  ],
});
</script>
