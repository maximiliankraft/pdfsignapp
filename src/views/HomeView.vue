<script setup lang="ts">

import { ref } from 'vue'

const xmlString = ref<string> ("<?xml version='1.0' encoding='UTF-8'?>\
<sl:CreateXMLSignatureRequest xmlns:sl='http://www.buergerkarte.at/namespaces/securitylayer/1.2#'>\
  <sl:KeyboxIdentifier>SecureSignatureKeypair</sl:KeyboxIdentifier>\
  <sl:DataObjectInfo Structure='enveloping'>\
    <sl:DataObject>\
      <sl:Base64Content>SGFsbG8hIEljaCBiaW4gZWluIFRlc3Qh</sl:Base64Content>\
    </sl:DataObject>\
    <sl:TransformsInfo>\
      <sl:FinalDataMetaInfo>\
        <sl:MimeType>text/plain</sl:MimeType>\
      </sl:FinalDataMetaInfo>\
    </sl:TransformsInfo>\
  </sl:DataObjectInfo>\
</sl:CreateXMLSignatureRequest>");



function constructXMLRequest(payload, MimeType) {
  // parse XML string to XML document
  var parser = new DOMParser();
  var xmlDoc = parser.parseFromString(xmlString.value, "text/xml");
  // set payload to XML document encoded as base64
  xmlDoc.getElementsByTagName("sl:Base64Content")[0].childNodes[0].nodeValue = btoa(payload);

  // set MIME type to XML document
  xmlDoc.getElementsByTagName("sl:MimeType")[0].childNodes[0].nodeValue = MimeType;

  // return XML document as string
  return new XMLSerializer().serializeToString(xmlDoc);
}

function readFile(file) {
  var reader = new FileReader();
  reader.readAsBinaryString(file);
  reader.onload = function () {
    // set file contents to field with id XMLRequest

    xmlString.value = constructXMLRequest(reader.result, file.type);
  };
}

var fileInputField = document.getElementById("fileInput");

// attach event listener to file input
const fileInputFieldChange =  (event: Event) => {

  console.log(event);
  

  console.log("file selected");

  // check if file is selected
  if ( event.target !== null &&  event.target.files.length > 0) {
    // read file contents
    readFile(event.target.files[0]);
  }
}

</script>

<template>
  <main>
    <form id="form0">
      <input type="file" id="fileInput" v-on:change="fileInputFieldChange($event)" >
    </form>

    <form method="post" id="form1" action="/api/default.aspx">
      <input type="hidden" name="XMLRequest" id="XMLRequest" v-model="xmlString" />
      <br />
      <!-- creates a post request  -->
      <input type="submit" name="submit" id="submit" value="start" />
    </form>
  </main>
</template>


