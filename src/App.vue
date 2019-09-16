<template>
    <div class="container">
        <!-- <SearchBar v-on:termChange="onTermChange"></SearchBar> -->
        <SearchBar @termChange="onTermChange"></SearchBar>
        <VideoDetail :video="selectedVideo"></VideoDetail>
        <!-- <VideoList v-bind:videos="videos"></VideoList> -->
        <VideoList @videoSelect="onVideoSelect" :videos="videos"></VideoList>
    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';

const API_KEY = 'AIzaSyBR5t9kOqMXB4dpfPI4KPzbk38IzKzD4Ao';

export default {
    name: 'App',
    components: {
        SearchBar,
        VideoList,
        VideoDetail
    },
    data: function() {
        return {
            videos: [],
            selectedVideo: null
        };
    },
    methods: {
        onVideoSelect: function(video) {
            this.selectedVideo = video;
        },
        onTermChange: function(searchTerm) {
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
        }
    }
};
</script>