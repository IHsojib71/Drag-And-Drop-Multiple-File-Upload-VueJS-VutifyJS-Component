<template>
    <v-card
        @drop.prevent="onDrop($event)"
        @dragover.prevent="dragover = true"
        @dragenter.prevent="dragover = true"
        @dragleave.prevent="dragover = false"
        :class="{ 'grey lighten-2': dragover }"
    >
        <v-card-text>
            <v-row
                class="d-flex flex-column"
                dense
                align="center"
                justify="center"
            >
                <v-icon :class="[dragover ? 'mt-2, mb-6' : 'mt-5']" size="60">
                    mdi-cloud-upload
                </v-icon>
                <p>Drop your file(s) here!</p>
            </v-row>
            <v-virtual-scroll
                v-if="uploadedFiles.length > 0"
                :items="uploadedFiles"
                height="150"
                item-height="50"
            >
                <template v-slot:default="{ item }">
                    <v-list-item :key="item.name">
                        <v-list-item-content>
                            <v-list-item-title>
                                {{ item.name }}
                                <span class="ml-3 text--secondary">
                                    {{ item.size }} bytes</span
                                >
                            </v-list-item-title>
                        </v-list-item-content>

                        <v-list-item-action>
                            <v-btn @click.stop="removeFile(item.name)" icon>
                                <v-icon> mdi-close-circle </v-icon>
                            </v-btn>
                        </v-list-item-action>
                    </v-list-item>

                    <v-divider></v-divider>
                </template>
            </v-virtual-scroll>
        </v-card-text>
    </v-card>
</template>

<script>
export default {
    name: "Upload",
    props: {
        multiple: {
            type: Boolean,
            default: false,
        },
    },
    data() {
        return {
            dragover: false,
            uploadedFiles: [],
        };
    },
    methods: {
        removeFile(fileName) {
            // Find the index of the
            const index = this.uploadedFiles.findIndex(
                (file) => file.name === fileName
            );
            // If file is in uploaded files remove it
            if (index > -1) this.uploadedFiles.splice(index, 1);
        },
        onDrop(e) {
            this.dragover = false;
            // If there are already uploaded files remove them
            if (this.uploadedFiles.length > 0) this.uploadedFiles = [];
            // If user has uploaded multiple files but the component is not multiple throw error
            if (!this.multiple && e.dataTransfer.files.length > 1) {
                this.$store.dispatch("addNotification", {
                    message: "Only one file can be uploaded at a time..",
                    colour: "error",
                });
            }
            // Add each file to the array of uploaded files
            else {
                //   e.dataTransfer.files.forEach(element=>this.uploadedFiles.push(element));
                for (let file of e.dataTransfer.files) {
                    this.uploadedFiles.push(file);
                }
            }
            this.submit();
        },
        submit() {
            // If there aren't any files to be uploaded throw error
            if (!this.uploadedFiles.length > 0) {
                this.$store.dispatch("addNotification", {
                    message: "There are no files to upload",
                    colour: "error",
                });
            } else {
                // Send uploaded files to parent component
                this.$emit("filesUploaded", this.uploadedFiles);
            }
        },
    },
};
</script>
