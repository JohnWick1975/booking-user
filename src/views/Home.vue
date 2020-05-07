<template>
    <div class="container">
        <div class="search">
            <b-field label="Find your house by title">
                <b-autocomplete
                        rounded
                        v-model="match"
                        placeholder="e.g. jQuery"
                >
                </b-autocomplete>
            </b-field>
        </div>
        <div class="selected">
            <b-field label="Select Your City">
                <b-select v-model="urban" placeholder="Select a name">
                    <option
                            v-for="value in filterCity"
                            :value="value"
                            :key="value.id">
                        {{ value }}
                    </option>
                </b-select>
            </b-field>
        </div>
        <div class="columns is-multiline">
            <div v-for="item in filter" :key="item.id" class="column is-4">
                <div class="card">
                    <div class="card-image">
                        <figure class="image is-4by3">
                            <img :src="item.img">
                        </figure>
                    </div>
                    <div class="card-content">
                        <div class="media">
                            <div class="media-content">
                                <p class="title is-6">{{item.title}}</p>
                                <p class="title is-4">{{item.city}}</p>
                                <hr>
                                <p class="subtitle is-4">{{item.price}} &euro; per night</p>
                                <div>
                                    {{item.text}}
                                </div>
                                <p @click="redirectToSingleCard(item.id)" class="has-text-primary has-text-right">
                                    ...read more</p>
                            </div>
                        </div>
                        <div class="content">
                            <button @click="addToCart(item.id)" class="button is-primary is-medium">RENT {{item.price}}&euro;</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import firebase from 'firebase/app';
    import 'firebase/firestore';
    import uniq from 'uniq';

    export default {

        name: 'Home',
        data() {
            return {
                urban: '',
                match: '',
                data: [],
                id: '',
                title: '',
                text: '',
                price: null,
                city: '',
                img: [],
            }
        },
        computed: {
            filter() {
                return this.data.filter(value => {
                    return value.title.toLowerCase().includes(this.match.toLowerCase()) &&
                        value.city.toLowerCase().includes(this.urban.toLowerCase());
                });
            },
            filterCity() {
                return uniq(this.data.map(({ city }) => city));
            }
        },
        methods: {
            cutText(string) {
                if (string.length > 200) {
                    string = string.substring(0, 200) + '...';
                    return string
                } else {
                    return string
                }
            },
            redirectToSingleCard(id) {
                this.$router.push('/singlecard/id/' + id);
            },
        },
        beforeMount() {
            firebase
                .firestore()
                .collection('houses')
                .get()
                .then((snapshot) => {
                    snapshot.docs.forEach((doc) => {
                        this.data.push({
                            id: doc.id,
                            title: doc.data().title,
                            price: doc.data().price,
                            img: doc.data().img[0],
                            text: this.cutText(doc.data().text),
                            city: doc.data().city,
                        });
                    })
                });

        },

    }

</script>

<style scoped>
    .columns {
        margin-top: 15px;
    }

    p.has-text-primary {
        cursor: pointer;
    }

    .selected {
        margin-top: 10px;
    }
</style>
