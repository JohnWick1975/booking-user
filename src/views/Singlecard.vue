<template>
    <div class="container">
        <div class="buttons">
            <b-button>
                <router-link to="/">Back to properties</router-link>
            </b-button>
        </div>
        <hr>
        <h1 class="title">{{title}}</h1>
        <div class="columns">
            <div class="column">
                <div class="flex-image">
                    <b-carousel class="carousel" :indicator="true" :pause-info="false">
                        <b-carousel-item v-for="(carousel, i) in img" :key="i">
                            <img class="image" :src="carousel"/>
                        </b-carousel-item>
                    </b-carousel>
                </div>
            </div>
        </div>
        <div class=" information columns">
            <div class="column">
                <p class="subtitle has-text-centered">Only {{price}}&euro; per night!</p>
                <hr>
                <p class="subtitle"><strong>Description</strong>: {{text}}</p>
            </div>
        </div>
        <div class="date columns is-centered has-background-light">
            <div class="column">
                <b-field label="Select a date">
                    <b-datepicker
                            placeholder="Type or select a date..."
                            :min-date="minDate"
                            :unselectable-dates="arrDate"
                            multiple
                            v-model="date"
                            editable>
                    </b-datepicker>
                </b-field>
            </div>
            <div class="column has-text-centered">
                <p class="title">Total Price: {{totalPrice}}&euro;</p>
                <b-button @click="addDate" class="has-text-primary is-large">Purchase</b-button>
            </div>
        </div>
    </div>
</template>

<script>
    import firebase from 'firebase/app';
    import 'firebase/firestore';

    export default {
        name: "singlecard",
        data() {
            return {
                minDate: new Date(),
                id: this.$route.params.id,
                title: '',
                price: null,
                text: '',
                city: '',
                img: [],
                date: [],
                reserveDate: [],
                arrDate: [],
            }
        },
        computed: {
            totalPrice(){
              return this.price * this.date.length;
            }
        },
        methods: {
            addDate() {
                this.date.forEach(item=>{
                this.reserveDate.unshift(item.toLocaleDateString('lt'));
                })
                firebase
                    .firestore()
                    .collection("houses")
                    .doc(this.id)
                    .update({
                        reserveDate: this.reserveDate,
                    })
                    .then(() => console.log('success'))
            },
        },
        beforeMount() {
            firebase
                .firestore()
                .collection("houses")
                .doc(this.id)
                .get()
                .then(data => {
                    this.id = data.id;
                    this.title = data.data().title;
                    this.img = data.data().img;
                    this.price = data.data().price;
                    this.city = data.data().city;
                    this.text = data.data().text;
                    data.data().reserveDate ? this.reserveDate = data.data().reserveDate : this.reserveDate = [];
                })
                .then(() => {
                    this.reserveDate.forEach(item => {
                        this.arrDate.push(new Date(item))
                    });
                })
        },

    }
</script>

<style scoped>
    img {
        height: 400px;
    }

    .flex-image {
        display: flex;
        justify-content: center;
    }

    .information {
        border: 1px solid lightgrey;
        border-radius: 10px;
    }

    .columns {
        margin-bottom: 20px;
    }

    .date, table {
        border-radius: 10px;
    }

    td, th {
        text-align: center !important;
    }

</style>