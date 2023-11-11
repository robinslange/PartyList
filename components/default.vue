<script setup lang="ts">
import { v4 as uuidv4 } from 'uuid';
import { useFirestore } from 'vuefire';
import { doc, getDoc, addDoc, collection } from 'firebase/firestore';

const db = useFirestore();

const list = await getDoc(doc(db, 'lists', 'list1'));
console.log(list.data());

type ListItem = {
    id: string,
    title: string,
    quantity: number
    link: string
}

let listItems: Array<ListItem> = [
    {
        id: uuidv4(),
        title: 'test',
        quantity: 1,
        link: '123'
    },
    {
        id: uuidv4(),
        title: 'test2',
        quantity: 1,
        link: '123'
    },
    {
        id: uuidv4(),
        title: 'test3',
        quantity: 1,
        link: '123'
    },
]

let formData = reactive({
    listName: '',
    listItemCount: 0,
    listItems: listItems,
});

function objectKeysToArray(obj: object) {
    return Object.keys(obj);
}

function addListItem() {
    formData.listItems.push({
        id: uuidv4(),
        title: '',
        quantity: 1,
        link: ''
    });
    console.log('listAdd', formData.listItems);
}


async function onSubmit(event: Event) {
    event?.preventDefault();
    await addDoc(collection(db, 'test'), formData);
}

</script>

<template>
    <div class="flex align-left justify-start">
        <button @click="addListItem">Add Item</button>
        <form @submit="onSubmit">

            <div class="inline-flex mx-2 my-0 bg-red-600" v-for="item in formData.listItems" :key="item.id">
                <span v-for="(field, index) in objectKeysToArray(item)" :key="field">
                    <div v-if="index > 0">
                        <input type="text" :name="field" :placeholder="field" />
                    </div>
                </span>
            </div>

            <button type="submit" name="submit">Submit</button>
        </form>
    </div>
</template>