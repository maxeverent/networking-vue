<template>
    <Page>
        <ActionBar>
            <Label text="Artist"/>
        </ActionBar>
        <StackLayout class="body">
            <FlexboxLayout class="nav">
                <Button text="Tracks" @tap="$navigateTo(Home)" class="nav-btn" />
                <Button text="Artists" @tap="$navigateTo(Artists)" class="nav-btn btnactive" />
                <Button text="Albums" @tap="$navigateTo(Albums)" class="nav-btn" />
            </FlexboxLayout>
            <StackLayout class="artist">
                <TextField v-model="artistName" class="artist-name-input" hint="Artist" />
                <Button text="Get info about artist" @tap="getArtists" class="btn-artist"/>
                <Label v-if="error" text="Empty or invalid value" class="error" />
                <template v-else>
                    <Label v-model="artist" class="artist-name" />
                    <TextView v-model="aboutArtist" class="about-artist" editable="false" />
                </template>
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>

import {Http} from '@nativescript/core'

import home from './Home.vue'
import albums from './Albums.vue'

export default {
    data() {
        return {
            Home: home,
            Albums: albums,
            artistName: '',
            artist: '',
            aboutArtist: '',
            res: null,
            error: false
        }
    },
    methods: {
        async getArtists() {
            await Http.getJSON(`https://ws.audioscrobbler.com/2.0/?method=artist.getinfo&artist=${this.artistName}&api_key=224376ecd317b1c8ee308308afc64653&format=json`).then(
            (result) => {
                this.error = false
                this.res = result.artist
                this.setArtist()
            },
            ).catch(err => {
                console.log(err)
                this.artistName = ''
                this.error = true
            })
            this.artistName = ''
        },
        setArtist() {
            this.artist = this.res.name
            this.aboutArtist = this.res.bio.content.replace(/<\/?[^>]+>/g,'')
        }
    }
}

</script>

<style></style>