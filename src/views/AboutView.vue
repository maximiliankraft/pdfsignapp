<script setup lang="ts">
function getFrameContents(iframeId: string) {
  var iFrame = document.getElementById(iframeId);
  var iFrameBody;
  if (iFrame.contentDocument) { // FF
    iFrameBody = iFrame.contentDocument.getElementsByTagName('body')[0];
  }
  else if (iFrame.contentWindow) { // IE
    iFrameBody = iFrame.contentWindow.document.getElementsByTagName('body')[0];
  }
  return (iFrameBody.innerHTML);
}

function downloadPdf() {
  console.log("download pdf");

  console.log(getFrameContents("signreq"))
}

window.addEventListener('message', (event) => {
  // Check the origin of the event to ensure it's from the expected iframe
  console.log("message received");
  console.log(event);

});

function frameEvent(event:Event) {
  console.log("frame event");
  console.log(event);
  
}

</script>

<template>
  <h1>
    Signwrapper
  </h1>
  <form>

    <button @click="downloadPdf()" > Access iframe contents</button>
  </form>

  <iframe src="/sign" id="signreq"  width="400px" height="400px"></iframe>
</template>

<style>

</style>
