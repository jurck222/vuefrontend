<template>
  <div class="container">
		<div>
			<h2>Music Analyzer</h2>
       <DxFileUploader
        accept="audio/*" 
        uploadMode="instantly"
        upload-url="https://whispering-river-75272.herokuapp.com/file"
        :upload-headers="headers"
        :on-uploaded="onFilesUploaded">   
    </DxFileUploader>
    <AudioItem :url="url" :times="beats" @duration-time="getTime"></AudioItem>
    <DataGrid :data-source="songs" :songs="songs"></DataGrid>
		</div>
	</div>
</template>

<script>

import AudioItem from './components/AudioPlayer'
import 'devextreme/dist/css/dx.light.css'; 
import DataGrid from './components/DataGrid'
import {
        DxFileUploader
    } from 'devextreme-vue/file-uploader';
	
	export default {
		data(){
			return {
				file: '',
        beats: [],
        beats2: [],
        url: '',
        songs:[],
        title:'',
        artist:'',
        genre:'',
        album:'',
        duration:'',
        format:'',
        bpm:'',
        headers:{
          'Access-Control-Allow-Origin': '*'
        }
        
			}
		},
     components: {
        
        DxFileUploader,
        AudioItem,
        DataGrid
    },
	
		methods: {
      getTime(dur){
        this.bpm=Math.floor(this.beats.length/(dur/60));
        var min = Math.floor(dur/60);
        var sec = Math.floor(dur%60);
        var time = min+":"+sec;
        console.log(time);
        this.duration=time;
        var id;
        if(this.songs.length==0){
          id=0;
        }
        else{
          id = this.songs[this.songs.length-1].id+1;
          console.log(id);
        }
        var test = true;

        for(let i = 0; i<this.songs.length; i++){
          if(this.songs[i].filename===this.file.name){
            test=false;
            break;
          }
        }
        if(test){
          var song = {
          id:id,
            filename:this.file.name,
            title: this.title,
            artist: this.artist,
            album: this.album,
            duration: this.duration,
            genre: this.genre,
            format: this.format,
            bpm: this.bpm,
            beats: this.beats 
          }
          this.songs=[...this.songs,song];
          console.log(this.songs);
        }
        else{
          console.log("already in there");
        }
      },
      onFilesUploaded(e) {
          console.log(e.request.response);
          this.file = e.file;
          console.log(this.file.name);
          this.format =  this.file.name.split('.').pop();
          this.url=URL.createObjectURL(this.file);
          var response = e.request.response.split(",");
          this.beats = response[0].replace("[\"","").split("a");
          this.beats.pop();
          for(var i =0; i< this.beats.length;i++){
            this.beats[i]=(Math.round( this.beats[i] * 10) / 10);
          }
          console.log(this.beats);
          var metadata =response[1].replace("\"]\n","").replace("\"","").split(":");
          console.log(metadata)
          this.artist=metadata[0];
          console.log(this.artist);
          this.album=metadata[1];
          console.log(this.album);
          this.title=metadata[2];
          console.log(this.title);
          this.genre=metadata[3];
          console.log(this.genre);
         
      },
		},
    created() {
      console.log('test');
      
      this.songs=[
        {id: 1, filename: '01. Only One Will Die.mp3', title: 'Only One Will Die', artist: 'Cannibal Corpse', album: 'Red Before Black', duration: "3:24", format: "mp3", genre: "Metal", bpm:190, beats:"0, 0.3,  0.6,  1,  1.3,  1.6,  1.9,  2.2,  2.6, 2.9,  3.2,  3.5,  3.8,  4.1,  4.4,  4.8,  5.1,  5.4,  5.7,  6,  6.3,  6.7,  7,  7.3,  7.6,  8,  8.3,  8.6,  9,  9.3,  9.7,  10,  10.3,  10.6,  10.9"},
        {id: 2, filename: '01 The End..mp3', title: 'The End.', artist: 'My Chemical Romance', album: 'The Black Parade', duration:"1:52", format:"mp3", genre:"Alternative", bpm:175, beats:"0.2, 0.5, 0.8, 1.2, 1.5, 1.8, 2.2, 2.5, 2.9, 3.2, 3.5, 3.9, 4.3, 4.6, 4.9, 5.3, 5.6, 6, 6.3, 6.7, 7,"},
        {id: 3, filename: 'Orbit Culture - The Shadowing (Official Music Video).mp3', title: 'The Shadowing', artist: 'Orbit Culture', album: 'Not specified', duration:"4:26", format:"mp3",genre: "Not specified", bpm: 139,beats:"0.4, 0.8, 1.3, 1.7, 2.1, 2.6, 3, 3.4, 3.9, 4.3, 4.7, 5.2, 5.6, 6, 6.4, 6.9, 7.3, 7.7, 8.2, 8.6, 9"}
      ]
    }

    
	}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
