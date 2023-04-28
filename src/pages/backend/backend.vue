<template>
  <div class="mt-5">
    <VCardTitle>Example reference</VCardTitle>
    <XmlViewer :xml="createOutput()" />
  </div>
</template>

<script setup>
import XmlViewer from 'vue3-xml-viewer'

// Create an empty array to hold the people
let people = []

// Split the input file into an array of lines
let input = `


P|firstName|lastName
T|mobileNumber|landLineNumber
A|street|city|zip
F|name|birthYear

P can be followed by T, A and F
F can be followed by T and A


P|Elof|Sundin
T|073-101801|018-101801
A|S:t Johannesgatan 16|Uppsala|75330
F|Hans|1967
A|Frodegatan 13B|Uppsala|75325
F|Anna|1969
T|073-101802|08-101802
P|Boris|Johnson
A|10 Downing Street|London
`

let lines = input.split('\n')

// Loop through each line in the array
for (let i = 0; i < lines.length; i++) {
  let line = lines[i]

  // Split the line into an array of parts
  let parts = line.split('|')

  // Determine the type of record based on the first part
  let type = parts[0]

  // Create a new person object if this is a "P" record
  if (type === 'P') {
    let person = {
      firstName: parts[1],
      lastName: parts[2],
      address: {},
      phone: {},
      family: []
    }

    // Add the person object to the people array
    people.push(person)
  }

  // Add data to the current person object based on the type of record
  let currentPerson = people[people.length - 1]

  switch (type) {
    case 'T':
      currentPerson.phone.mobile = parts[1]
      currentPerson.phone.landline = parts[2]
      break
    case 'A':
      currentPerson.address.street = parts[1]
      currentPerson.address.city = parts[2]
      currentPerson.address.zip = parts[3]
      break

    case 'F':
      let familyMember = {
        name: parts[1],
        born: parts[2],
        address: {}
      }

      // Determine if there are any additional parts
      for (let j = 3; j < parts.length; j++) {
        let subParts = parts[j].split('|')
        let subType = subParts[0]

        switch (subType) {
          case 'T':
            familyMember.phone.mobile = subParts[1]
            familyMember.phone.landline = subParts[2]
            break

          case 'A':
            familyMember.address.street = subParts[1]
            familyMember.address.city = subParts[2]
            familyMember.address.zip = subParts[3]
            break
        }
      }

      currentPerson.family.push(familyMember)
      break
  }
}

function createXMLString(people) {
  let xml = '<people>\n'

  for (let person of people) {
    xml += '  <person>\n'
    xml += `    <firstname>${person.firstName}</firstname>\n`
    xml += `    <lastname>${person.lastName}</lastname>\n`
    xml += '    <address>\n'
    xml += `      <street>${person.address.street}</street>\n`
    xml += `      <city>${person.address.city}</city>\n`
    xml += `      <zip>${person.address.zip}</zip>\n`
    xml += '    </address>\n'
    xml += '    <phone>\n'
    xml += `      <mobile>${person.phone.mobile}</mobile>\n`
    xml += `      <landline>${person.phone.landline}</landline>\n`
    xml += '    </phone>\n'

    for (let family of person.family) {
      xml += '    <family>\n'
      xml += `      <name>${family.name}</name>\n`
      xml += `      <born>${family.born}</born>\n`
      xml += `      <address>${family.address}</address>\n`
      xml += '    </family>\n'
    }

    xml += '  </person>\n'
  }
  xml += '</people>'
  return xml
}

function createOutput() {
  return createXMLString(people)
}
</script>
