<template>
    <StackLayout class="track">
        <Label class="track-name">{{trackName}}</Label>
        <Label class="track-info">Artists: {{artistName}}</Label>
        <Label class="track-info">Listeners: {{listeners}}</Label>
    </StackLayout>
</template>

<script>

export default {
    data() {
        return {
        }
    },
    props: {
        trackName: String,
        artistName: String,
        listeners: String,
    },
    methods: {

    }
}

</script>

<style src="@/style.css"></style>