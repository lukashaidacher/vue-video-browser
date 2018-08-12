<template>
    <div class="container">
        <SearchBar @termChange="onTermChange"></SearchBar> <!-- alt: v-on:termChange -->
        <div class="row">
            <VideoDetail :video="selectedVideo"></VideoDetail>
            <VideoList :videos="videos" @videoSelect="onVideoSelect"></VideoList> <!-- alt: v-bind:videos -->
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import SearchBar from './components/SearchBar';
    import VideoList from './components/VideoList';
    import VideoDetail from './components/VideoDetail';

    const API_KEY = 'AIzaSyBu6kmQZvvLMQRAdDE8O24-F-WNmtuvy_o';

    export default {
        name: 'App',

        components: {
            SearchBar,
            VideoList,
            VideoDetail
        },

        data: function () {
            return {
                videos: [],
                selectedVideo: null
            };
        },

        methods: {
            onTermChange: function (searchTerm) {
                axios.get('https://www.googleapis.com/youtube/v3/search', {
                    params: {
                        key: API_KEY,
                        type: 'video',
                        part: 'snippet',
                        q: searchTerm
                    }
                }).then(response => {
                    this.videos = response.data.items;
                });
            },
            onVideoSelect: function (video) {
                this.selectedVideo = video;
            }
        }
    };
</script>