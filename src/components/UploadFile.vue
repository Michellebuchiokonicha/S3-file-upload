<!-- <template class="mi">
 <div class="upload">
  <h1>Amazon S3 Upload File</h1>
  <form>
   <div class="file">
    <div class="type">
      <label class="label">File Type</label>
      <select v-model="type" class="select">
        <option></option>
        <option>Image</option>
        <option>PDF</option>
        <option>Docs</option>
        <option>XML</option>
        <option>CSV</option>
        <option>Video</option>
      </select>
    </div>
    <div class="type">
      <label class="label">Upload file</label>
    <input v-on:change="handleChange" multiple class="input" type="file" placeholder="upload file" />
    </div>*-
  </div>


  </form>
 </div>
</template>
<script>
export default {
  name: 'UploadFile',
  props: {
    msg: String
  },
  data(){
    return{
      file: "",
      type: ""
    }
  },
  methods:  {
    async handleChange (e) {
      const file = e;
      if(!file) return;
      const readData = (f) =>
      new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(f);
      });
      const data = await readData(file);
      this.previewFiles = data;
    },
    
    inputChanged(e) {
      let t = e.target.files[0];
      this.previewImage = URL.createObjectURL(t);
    },
    removeImage() {
      this.previewImage = "";
    },
  }
}
</script>
<style scoped>
.upload{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  align-items: center;
  background: rgb(225, 225, 225); 
  height: 95vh;
  overflow-x: hidden;
  overflow-y: hidden;
}
form{
  display: flex;
  flex-direction: column;
  background: white;
  height: 60%; 
  padding-left: 2%;
  width: 30%;
  border-radius: 20px;
}
.file{
  margin: auto;
}
.type{
  display: flex;
  flex-direction: column;
  margin-bottom: 15%;
}
.label{
    font-size: medium;
    font-weight: 16px;
    padding-bottom: 5%;
    
}
.mi{
  overflow: hidden;
}
.select{
  height: 2rem;
 border-radius: 5px;
 width: 90%;
}
.input{
  height: 2rem;
  border-radius: 5px;
  width: 90%;
  border: 1px solid rgb(110, 110, 110);
}
.child-img {
  background-size: cover;
  width: 20%;
  height: 10rem;
  background-position: center;
  border: 4px solid #c4c1c1;
  border-radius: 5%;
  box-shadow: 2px 1px solid#c71f40;
}
</style> -->

<!-- <template>
  <div>
    <input type="file" @change="uploadToS3" />
  </div>
</template> -->


<template class="mi">
  <div class="upload">
   <h1>Amazon S3 Upload File</h1>
   <form>
    <div class="file">
     <div class="type">
       <label class="label">File Type</label>
       <select v-model="type" class="select">
         <option></option>
         <option>Image</option>
         <option>PDF</option>
         <option>Docs</option>
         <option>XML</option>
         <option>CSV</option>
         <option>Video</option>
       </select>
     </div>
     <div class="type">
       <label class="label">Upload file</label>
     <input @change="uploadToS3" multiple class="input" type="file" placeholder="upload file" />
     </div>
   </div>
 
 
   </form>
  </div>
 </template>
<script>
import AWS from 'aws-sdk';

const accessKeyId = 'AKIAQFPMJQQTUEZVRHVZ';
const region = 'eu-central-1';
const bucket = 'landmark-assets-bucket';

export default {
  data() {
    return {
      s3: new AWS.S3({
        accessKeyId,
        region,
        // AWS_SDK_LOAD_CONFIG:1
      })
    }
  },
  methods: {
    uploadToS3(event) {
      const file = event.target.files[0];
      const params = {
        Bucket: bucket,
        Key: file.name,
        Body: file
      };
      this.s3.upload(params, function(err, data) {
        if (err) {
          console.log('Error uploading to S3: ', err);
          return;
        }
        console.log('Successfully uploaded file to S3: ', data);
        // Check if the file was uploaded to the bucket
        if (data.Bucket && data.Key) {
          console.log('File was sent to S3 bucket: ', data.Bucket, data.Key);
        }
      });
    }
  }
}
</script>

<style scoped>
.upload{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  align-items: center;
  background: rgb(225, 225, 225); 
  height: 95vh;
  overflow-x: hidden;
  overflow-y: hidden;
}
form{
  display: flex;
  flex-direction: column;
  background: white;
  height: 60%; 
  padding-left: 2%;
  width: 30%;
  border-radius: 20px;
}
.file{
  margin: auto;
}
.type{
  display: flex;
  flex-direction: column;
  margin-bottom: 15%;
}
.label{
    font-size: medium;
    font-weight: 16px;
    padding-bottom: 5%;
    
}
.mi{
  overflow: hidden;
}
.select{
  height: 2rem;
 border-radius: 5px;
 width: 90%;
}
.input{
  height: 2rem;
  border-radius: 5px;
  width: 90%;
  border: 1px solid rgb(110, 110, 110);
}
.child-img {
  background-size: cover;
  width: 20%;
  height: 10rem;
  background-position: center;
  border: 4px solid #c4c1c1;
  border-radius: 5%;
  box-shadow: 2px 1px solid#c71f40;
}
</style>
