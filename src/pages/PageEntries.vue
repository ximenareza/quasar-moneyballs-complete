<template>
  <q-page>
    <div class="q-pa-md">
      <q-list bordered separator>

        <q-slide-item 
        v-for="entry in entries"
        :key="entry.id"
       
        @right="onEntrySlideRight($event,entry)"
        left-color="positive"
        right-color="negative"
        >
          <!--<template v-slot:left>
            <q-icon name="done"/>
          </template>-->

          <template v-slot:right>
            <q-icon name="delete"/>
          </template>



        <q-item>


          <q-item-section
            class="text-weight-bold"
            :class="useAmountColorClass(entry.amount)"
          >
            {{ entry.name }}
          </q-item-section>

          <q-item-section
            class="text-weight-bold"
            :class="useAmountColorClass(entry.amount)"
            side
          >
            {{ useCurrencify(entry.amount) }}
          </q-item-section>
        </q-item>


      </q-slide-item>
      </q-list>
    </div>

    <q-footer
     class="transparent">


      <div class="row q-mb-sm q-px-md q-py-sm shadow-up-3">
        <div class="col text-grey-7 text-h6">
          Balance:
        </div>
        <div 
        :class="useAmountColorClass(balance)"
        class="col  text-h6 text-right">
          {{useCurrencify( balance) }}
        </div>
      </div>
     <q-form
      @submit="addEntry"
      class="row q-px-sm q-pb-sm q-col-gutter-sm bg-primary"
      >


        <div class="col">
          <q-input
            v-model="addEntryFrom.name"
            ref="nameRef"
            placeholder="Name"
            bg-color="white"
            outlined
            dense
          />
        </div>
        <div class="col">
          <q-input
           v-model.number="addEntryFrom.amount"
            input-class="text-right"
            placeholder="Amount"
            bg-color="white"
            type="number"
            step="0.01"
            outlined
            dense
          />
        </div>
        <div class="col-auto">
          <q-btn round color="primary" type="submit" icon="add" />
        </div>
      </q-form>
    </q-footer>
  </q-page>
</template>

<script setup>
/*
   imports
  */

import { ref, computed, reactive, setBlockTracking, openBlock } from "vue";
import { uid, useQuasar } from "quasar";
import { useCurrencify } from "src/use/useCurrencity";
import { useAmountColorClass } from "src/use/useAmountColorClass";


/*queasar*/

const $q = useQuasar()

/*
   entries
  */

const entries = ref([
  {
    id: "id1",
    name: "Salary",
    amount: 4999.99,
  },
  {
    id: "id2",
    name: "Rent",
    amount: -999,
  },
  {
    id: "id3",
    name: "Phone",
    amount: -14.99,
  },
  {
    id: "id4",
    name: "Unknow",
    amount: 0,
  },
]);


/*balance*/

const balance = computed(() => {
return entries.value.reduce((accumulator,{amount})=>
   {
    return accumulator + amount
  },0)
})
 
/*add entry*/


const nameRef = ref(null)


const addEntryFromDeafault ={

  name:'',
  amount: null
}

const addEntryFrom = reactive({
 ...addEntryFromDeafault
})

const addEntryFromReset = () =>{
  Object.assign(addEntryFrom, addEntryFromDeafault)
  nameRef.value.focus()
}


const addEntry = () =>{
  //const newEntry = {
   // id: uid(),
    //name:addEntryFrom.name,
    //amount:addEntryFrom.amount
  //}

  const newEntry = Object.assign({},addEntryFrom,{id: uid()})
  entries.value.push(newEntry)
  addEntryFromReset()


}

/*
slide items

*/

const onEntrySlideRight= ({reset}, entry) =>{

  $q.dialog({
        title: 'Delete Entri',
        message: `delete this entry?
        <div class="q-mt-md text-weight-bold ${useAmountColorClass
        (entry.amount)}">
          ${entry.name} : ${ useCurrencify(entry.amount)}
          </div>
        `,
      
        cancel: true,
        persistent: true,
        html:true,

        ok:{
          label:'Delete',
          color:'negative',
          noCaps: true
        },

        cancel:{
          color:'primary',
          noCaps: true
        }


      }).onOk(() => {
         deleteEntry(entry.id)
      
      }).onCancel(() => {
         reset()
      })
    }

    /*
    delete entry
    */

    const deleteEntry = entryId  => {
      const index = entries.value.findIndex(entry => entry.id === entryId)
     entries.value.splice(index,1)
     $q.notify({
      message:'entry deleted',
      position:'top'
     })
    }

</script>
