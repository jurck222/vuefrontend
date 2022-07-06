<template>
  <div class="container">
		<div>
			<h2>Music Analyzer</h2>
       <DxFileUploader
        accept="audio/*" 
        uploadMode="instantly"
        upload-url="https://whispering-river-75272.herokuapp.com//file"
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
        bpm:''
        
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
        }
        else{
          console.log("already in there");
        }
      },
      onFilesUploaded(e) {
        
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
          var metadata =response[1].replace("\"]\n","").replace("\"","").split(":");
          console.log(metadata)
          this.artist=metadata[0];
          this.album=metadata[1];
          this.title=metadata[2];
          this.genre=metadata[3];
         
      },
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
