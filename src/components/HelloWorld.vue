<template>
	<div class="container">
		<div>
			<h2>Single File</h2>
			<hr/>
			<label>File
				<input type="file" @change="handleFileUpload( $event )" :name="static_files"/>
			</label>
			<br>
			<button v-on:click="submitFile()">Submit</button>
		</div>
	</div>
</template>

<script>
	import axios from 'axios';
	
	export default {
		data(){
			return {
				file: '',
        
			}
		},
		
		methods: {
			handleFileUpload( event ){
				this.file = event.target.files[0];
				
        
			},
			
			submitFile(){
        
				let formData = new FormData();
				
				formData.append('file', this.file);
        
				axios.post( 'http://127.0.0.1:5000/file',
					formData,
					{
						headers: {
								'Content-Type': 'multipart/form-data',
                
						}
					}
				).then(function(response){
					console.log(response.data);
				})
				.catch(function(){
					console.log('FAILURE!!');
				});
			}
		}
	}
</script>
	