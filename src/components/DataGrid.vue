<template>
    <div id="app-container">
        <DxDataGrid id="dataGrid" :data-source="songs" key-expr="id" >
            
        </DxDataGrid>
       
    </div>    
</template>
<script>
import 'devextreme/dist/css/dx.light.css';
 
import { DxDataGrid } from 'devextreme-vue/data-grid';
 
export default {
    
    name: 'DataGrid',
    components: {
        DxDataGrid
    },
    props:{
            songs:Array
        },
    

    

}
</script>