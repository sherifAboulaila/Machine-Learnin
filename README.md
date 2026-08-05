
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Teachable Machine Image Classifier</title>
<img width="1920" height="1080" alt="image" src="Screenshot (10).png" /># Machine-Learnin
project to Learn human how to Learn Machine?:

this Link for project :

https://teachablemachine.withgoogle.com/models/6_06z-skR/

<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>

<style>
body{
    font-family:Arial;
    text-align:center;
    background:#f5f5f5;
}

.container{
    width:600px;
    margin:auto;
    background:white;
    padding:20px;
    border-radius:10px;
    box-shadow:0 0 10px rgba(0,0,0,.2);
}

img{
    margin-top:20px;
    max-width:400px;
    max-height:400px;
    border-radius:10px;
    border:2px solid #ddd;
}

button{
    margin-top:20px;
    padding:10px 20px;
    font-size:18px;
    cursor:pointer;
}

#label-container{
    margin-top:20px;
    font-size:20px;
}
</style>

</head>

<body>

<div class="container">

<h2>Machine Learning Image Classification</h2>

<input type="file" id="imageUpload" accept="image/*">

<br>

<img id="preview" style="display:none;">

<br>

<button onclick="predictImage()">تحليل الصورة</button>

<div id="label-container"></div>

</div>

<script>

const MODEL_URL = "https://teachablemachine.withgoogle.com/models/6_06z-skR/";

let model;
let maxPredictions;
let labelContainer;

async function loadModel(){

    const modelURL = MODEL_URL + "model.json";
    const metadataURL = MODEL_URL + "metadata.json";

    model = await tmImage.load(modelURL, metadataURL);

    maxPredictions = model.getTotalClasses();

    labelContainer = document.getElementById("label-container");

    for(let i=0;i<maxPredictions;i++){
        labelContainer.appendChild(document.createElement("div"));
    }

}

window.onload = loadModel;

document.getElementById("imageUpload").addEventListener("change",function(event){

    const file = event.target.files[0];

    if(!file) return;

    const image = document.getElementById("preview");

    image.src = URL.createObjectURL(file);

    image.style.display="block";

});

async function predictImage(){

    const image=document.getElementById("preview");

    if(image.src==""){
        alert("اختر صورة أولاً");
        return;
    }

    const prediction=await model.predict(image);

    for(let i=0;i<maxPredictions;i++){

        labelContainer.childNodes[i].innerHTML=
        "<b>"+prediction[i].className+"</b> : "+
        (prediction[i].probability*100).toFixed(2)+" %";

    }

}

</script>

</body>
</html>
