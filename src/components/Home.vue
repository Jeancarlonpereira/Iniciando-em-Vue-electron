<template>
<v-container fluid>
    <v-form>
        <v-file-input label="Selecione as legendas" 
        prepend-icon="mdi-message-text"
        append-outer-icon="mdi-send"
        outlined
        multiple
        chips
         v-model="files"
         @click:append-outer="processSubtitles">

        </v-file-input>
    </v-form>
    <div class="pills">
        <Pill v-for="word in groupedWords" :key="word.name"
        :name="word.name" :amount="word.amount"></Pill>
    </div>
</v-container>
</template>

<script>
import Pill from './Pill'
import {ipcRenderer} from 'electron'
window.ipcRenderer = ipcRenderer;
export default {
    components: {Pill},

    data: function(){

        return{
            files: [],
            groupedWords: []
        }
    },
    methods:{
        processSubtitles() {
            const paths = this.files.map(x=>x.path)
            ipcRenderer.send('process', paths );
            ipcRenderer.on('process',(event, resp)=>{
                this.groupedWords = resp;
            });
        }
    }
    
}
</script>

<style scoped>
.pills{

    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}
</style>