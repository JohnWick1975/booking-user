<template>
    <div id="app">
        <b-navbar>
            <template slot="brand">
                <b-navbar-item tag="router-link" :to="{ path: '/' }">
                    <img
                            src="https://raw.githubusercontent.com/buefy/buefy/dev/static/img/buefy-logo.png"
                            alt="Lightweight UI components for Vue.js based on Bulma"
                    >
                </b-navbar-item>
            </template>
            <template slot="start">
                <b-navbar-item >
                    <router-link to="/">Properties</router-link>
                </b-navbar-item>
                <b-navbar-item >
                    <router-link to="/about">About</router-link>
                </b-navbar-item>
            </template>

            <template slot="end">
                <b-navbar-item tag="div">
                    <div class="buttons">
                        <a class="button is-primary">
                            <strong>Sign up</strong>
                        </a>
                        <a class="button is-light">
                            Log in
                        </a>
                    </div>
                </b-navbar-item>
            </template>
        </b-navbar>
        <router-view/>
    </div>
</template>

<style>
</style>
