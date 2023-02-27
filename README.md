I have followed this post and modified this according to my needs

call from parent component

<DragDrop :multiple="true" @filesUploaded="processUpload($event)"></DragDrop>

get responses and do your logic for file uploads here 

processUpload(val){
}
