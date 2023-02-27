I have followed this post and modified this according to my needs
https://george-hadjigeorgiou97.medium.com/step-by-step-custom-drag-drop-upload-component-in-vuetify-vue-2-43c99794643d

call from parent component
```
<DragDrop :multiple="true" @filesUploaded="processUpload($event)"></DragDrop>
```
get responses and do your logic for file uploads here, you will get your files in this val 

processUpload(val){
}
