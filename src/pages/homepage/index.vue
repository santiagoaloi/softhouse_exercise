<template>
  <VContainer fluid>
    <VCard class="p-10">
      <VCardText>
        <div class="text-h6">
          The first system delivers a line-based file format while the other requires XML.
        </div>
        <div>You need to write a converter that builds the correct XML structure.</div>

        <div class="mt-5">
          <div><b>Data mapping reference</b></div>
          <code>
            <div>P|firstName|lastName</div>
            <div>T|mobileNumber|landLineNumber</div>
            <div>A|street|city|zip</div>
            <div>F|name|birthYear</div>
            <br />
            <div>P can be followed by T</div>
            <div>A and F can be followed by T and A</div>
          </code>
        </div>

        <div class="mt-5">
          <div><b>Input data</b></div>
          <code>
            <div>P|Elof|Sundin</div>
            <div>T|073-101801|018-101801</div>
            <div>A|S:t Johannesgatan 16|Uppsala|75330</div>
            <div>F|Hans|1967</div>
            <div>A|Frodegatan 13B|Uppsala|75325</div>
            <div>F|Anna|1969</div>
            <div>T|073-101802|08-101802</div>
            <div>P|Boris|Johnson</div>
            <div>A|10 Downing Street|London</div>
          </code>
        </div>

        <div class="mt-5">
          <VCardTitle>Example reference</VCardTitle>
          <XmlViewer :xml="xml" />
        </div>
      </VCardText>
    </VCard>

    <VCard class="p-10 mt-5">
      <VCardText>
        <div class="text-h6">How do we solve this?</div>
        <div>We send a payload to the backend that returns the data in XML format.</div>

        <div class="mt-5">
          <VBtn color="primary">Get started</VBtn>
        </div>
      </VCardText>
    </VCard>
  </VContainer>
</template>
<script setup>
import XmlViewer from 'vue3-xml-viewer'

const xml = `<people>
<person>
<firstname>Elof</firstname>
<lastname>Sundin</lastname>
<address>
<street>S:t Johannesgatan 16</street>
<city>Uppsala</city>
<zip>75330</zip>
</address>
<phone>
<mobile>073-101801</mobile>
<landline>018-101801</landline>
</phone>
<family>
<name>Hans</name>
<born>1967</born>
<address>...</address>
</family>
<family>...</family>
</person>
<person>...</person>
</people>
`
</script>
