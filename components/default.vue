<script setup lang="ts">
import { useFirestore } from 'vuefire';
import { doc, getDoc, addDoc, collection } from 'firebase/firestore';

const db = useFirestore();

const list = await getDoc(doc(db, 'lists', 'list1'));
console.log(list.data());

let formData = {
    name:'',
}

async function onSubmit(event: Event) {
    event?.preventDefault();
    console.log(formData);
    await addDoc(collection(db, 'test'), formData);
}

</script>

<template>
    <div>
        <form @submit="onSubmit">
            <input type="text" name="name" v-model="formData.name"/>
            <button type="submit" name="submit">Submit</button>
        </form>
    </div>
</template>