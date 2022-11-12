<template>
    <Page>
        <ActionBar>
            <Label text="Networking"/>
        </ActionBar>
        <StackLayout >
            <FlexboxLayout class="nav">
                <Button text="Tracks" class="nav-btn btnactive" />
                <Button text="Artists" @tap="$navigateTo(Artists)" class="nav-btn" />
                <Button text="Albums" @tap="$navigateTo(Album)" class="nav-btn" />
            </FlexboxLayout>
            <ScrollView orientation="vertical" class="scroll">
                <StackLayout class="tracks">
                    <Track 
                    v-for="track in tracks" 
                    :trackName="track.trackName" 
                    :artistName="track.artistName" 
                    :listeners="track.listeners" 
                    />
                </StackLayout>
            </ScrollView>
            <FlexboxLayout class="top-tracks-btn">
                <Button text="Top 10" @tap="getTop(10)" class="tracks-btn" :class="{btnactive: btnStatus(count, 10)}" />
                <Button text="Top 10-20" @tap="getTop(20)" class="tracks-btn" :class="{btnactive: btnStatus(count, 20)}" />
                <Button text="Top 20-30" @tap="getTop(30)" class="tracks-btn" :class="{btnactive: btnStatus(count, 30)}" />
            </FlexboxLayout>
        </StackLayout>
    </Page>
</template>

<script>

import {Http} from '@nativescript/core'

import Track from './Track.vue'
import artists from './Artists.vue'
import album from './Albums.vue'

  export default {
    data() {
        return {
            res: null,
            count: 10,
            tracks: [],
            Artists: artists,
            Album: album,
        }
    },
    components: { Track },
    methods: {
        async getTracks() {
            await Http.getJSON(`https://ws.audioscrobbler.com/2.0/?method=chart.gettoptracks&api_key=224376ecd317b1c8ee308308afc64653&limit=30&format=json`).then(
            (result) => {
                this.res = result.tracks.track
            },
            e => {console.log(e)}
            )
            this.setTracks(0)
        },
        setTracks(number) {
            this.tracks = []
            for (let i = number; i < this.count; ++i) {
                let trackName = this.res[i].name
                let artist = this.res[i].artist.name
                let listeners = this.res[i].listeners
                this.tracks.push({trackName: trackName, artistName: artist, listeners: listeners})
            }
        },
        getTop(number) {
            if (number === 10) {
                this.count = 10
                this.setTracks(0)
            }
            if (number === 20) {
                this.count = 20
                this.setTracks(10)
            }
            if (number === 30) {
                this.count = 30
                this.setTracks(20)
            }
        },
        btnStatus(count, number) {
            if (count === number) {
                return true
            }
            else {
                return false
            }
        }
    },
    created() {
        this.getTracks()
    },
  };
</script>

<style src="@/style.css"></style>
