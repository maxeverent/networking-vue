<template>
    <Page>
        <ActionBar>
            <Label text="Artist"/>
        </ActionBar>
        <StackLayout class="body">
            <FlexboxLayout class="nav">
                <Button text="Tracks" @tap="$navigateTo(Home)" class="nav-btn" />
                <Button text="Artists" @tap="$navigateTo(Artists)" class="nav-btn" />
                <Button text="Albums" class="nav-btn btnactive" />
            </FlexboxLayout>
            <StackLayout class="album">
                <TextField v-model="artistName" class="artist-name-input" hint="Artist" />
                <TextField v-model="albumName" class="album-name-input" hint="Album" />
                <Button text="Get info about album" @tap="getAlbums" class="btn-album" />
                <Label v-if="error" text="Empty or invalid value" class="error" />
                <template v-else>
                    <Label v-model="albumN" class="album-n" />
                    <Label v-model="artistN" class="album-n" />
                    <Label v-model="listeners" class="listeners-album" />
                    <StackLayout class="img">
                        <Image :src="img" />
                    </StackLayout>
                    <TextView v-model="album" editable="false" class="about-album" />
                </template>
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>

import { Http } from '@nativescript/core'

import home from './Home.vue'
import artists from './Artists.vue'

export default {
    data() {
        return {
            Home: home,
            Artists: artists,
            artistName: '',
            albumName: '',
            albumN: '',
            artistN: '',
            listeners: '',
            album: '',
            res: null,
            error: false,
            inmg: '',
        }
    },
    methods: {
        async getAlbums() {
            await Http.getJSON(`https://ws.audioscrobbler.com/2.0/?method=album.getinfo&api_key=224376ecd317b1c8ee308308afc64653&artist=${this.artistName}&album=${this.albumName}&format=json`).then(
            (result) => {
                this.error = false
                this.res = result.album
                this.setAlbum()
            },
            ).catch(err => {
                console.log(err)
                this.error = true
            })
            this.artistName = ''
            this.albumName = ''
        },
        setAlbum() {
            this.albumN = this.res.name
            this.artistN = this.res.artist
            this.listeners = "Listeners:" + " " + this.res.listeners
            this.img = this.res.image['2']['#text']
            this.album = this.res.wiki.summary.replace(/<\/?[^>]+>/g,'')
            console.log(this.img)
        },
    }
}

</script>

<style></style>