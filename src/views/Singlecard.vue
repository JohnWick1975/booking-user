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
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Available Dates</th>
                            <th>Book</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>2020-05-25</td>
                            <td>
                                <b-checkbox v-model="checkboxCustom"
                                            true-value="Yes"
                                            false-value="No">
                                    {{ checkboxCustom }}
                                </b-checkbox>
                            </td>
                        </tr>
                        <tr>
                            <td>2020-05-26</td>
                            <td>
                                <b-checkbox v-model="checkboxCustom"
                                            true-value="Yes"
                                            false-value="No">
                                    {{ checkboxCustom }}
                                </b-checkbox>
                            </td>
                        </tr>
                        <tr>
                            <td>2020-05-27</td>
                            <td>
                                <b-checkbox v-model="checkboxCustom">
                                </b-checkbox>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="column">
                <p class="title">Total Price:</p>
                <b-button class="has-text-primary is-large">Purchase</b-button>
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
                id: this.$route.params.id,
                title: '',
                price: null,
                text: '',
                city: '',
                img: [],
            }
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
                });
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