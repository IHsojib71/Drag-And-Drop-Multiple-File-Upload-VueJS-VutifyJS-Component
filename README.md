# Drag-And-Drop-Multiple-File-Upload-VueJS-VutifyJS-

#call from parent component
<DragDrop :multiple="true" @filesUploaded="processUpload($event)"></DragDrop>

#get responses and do your logic for file uploads here 
processUpload(val){
}
