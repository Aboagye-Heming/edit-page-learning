<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Edit</ion-title>
        <ion-buttons>
          <ion-button @click="save"> Save </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <QuillEditor
        theme="snow"
        v-model:content="content"
        content-type="html"
        :toolbar="[
          [{ header: [1, 2, 3, 4, 5, 6, false] }],
          ['bold', 'italic', 'underline', 'strike'],
          [{ list: 'ordered' }, { list: 'bullet' }],
          [{ indent: '-1' }, { indent: '+1' }],
        ]"
      />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonButton,
  IonButtons,
  useIonRouter,
} from "@ionic/vue";
import { defineComponent, ref } from "vue";
import { Filesystem, Encoding, Directory } from "@capacitor/filesystem";
import { QuillEditor } from "@vueup/vue-quill";
import "@vueup/vue-quill/dist/vue-quill.snow.css";

export default defineComponent({
  name: "EditPage",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton,
    IonButtons,
    QuillEditor,
  },
  setup() {
    const router = useIonRouter();
    const content = ref("");
    const save = async () => {
      const file = `note-${Date.now()}.txt`;
      await Filesystem.writeFile({
        path: `notes/${file}`,
        data: content.value || "",
        directory: Directory.Documents,
        encoding: Encoding.UTF8,
      });
      router.back();
    };

    return {
      content,
      save,
    };
  },
});
</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
