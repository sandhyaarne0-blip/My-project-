<!DOCTYPE html>
<html>
<head>
<title>Smart Health AI Pro</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Poppins',sans-serif;
}

body{
background:linear-gradient(120deg,#eef3ff,#d9e8ff);
}

header{
background:linear-gradient(90deg,#0a5bd8,#4a90ff);
color:white;
text-align:center;
padding:40px;
box-shadow:0 5px 15px rgba(0,0,0,0.2);
}

h1{
font-size:38px;
letter-spacing:1px;
}

.intro{
text-align:center;
padding:30px;
}

.container{
max-width:1000px;
margin:auto;
background:white;
padding:30px;
margin-top:30px;
border-radius:12px;
box-shadow:0 8px 20px rgba(0,0,0,0.1);
transition:0.3s;
}

.container:hover{
transform:translateY(-4px);
}

input,select{
padding:10px;
width:48%;
border-radius:6px;
border:1px solid #ccc;
margin:5px;
font-size:14px;
}

button{
background:linear-gradient(90deg,#0a5bd8,#4a90ff);
color:white;
border:none;
padding:12px 20px;
border-radius:8px;
cursor:pointer;
margin:5px;
transition:0.3s;
}

button:hover{
transform:scale(1.05);
background:linear-gradient(90deg,#083f94,#0a5bd8);
}

.symptoms{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:10px;
margin-top:10px;
}

.symptoms label{
background:#f4f7ff;
padding:8px;
border-radius:6px;
cursor:pointer;
}

.symptoms label:hover{
background:#dbe7ff;
}

.chart-box{
width:80%;
margin:auto;
}

#result{
margin-top:15px;
font-size:22px;
color:#0a5bd8;
font-weight:600;
}

#medicine{
background:#f0f6ff;
padding:15px;
border-radius:8px;
margin-top:15px;
}

img{
margin-top:15px;
border-radius:10px;
width:260px;
box-shadow:0 5px 10px rgba(0,0,0,0.2);
}

.doctor-grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:20px;
}

.disease-card{
background:#f7faff;
padding:20px;
border-radius:10px;
text-align:center;
box-shadow:0 5px 12px rgba(0,0,0,0.1);
transition:0.3s;
}

.disease-card:hover{
transform:translateY(-6px);
}

.contact-btn{
display:block;
margin-top:8px;
background:#0a5bd8;
color:white;
padding:8px;
border-radius:6px;
text-decoration:none;
}

.contact-btn:hover{
background:#083f94;
}

.start-btn{
background:#0a5bd8;
padding:12px 25px;
border-radius:8px;
color:white;
border:none;
cursor:pointer;
margin-top:15px;
}

.dark{
background:#121212;
color:white;
}

.dark .container{
background:#1e1e1e;
color:white;
}

.dark input,
.dark select{
background:#333;
color:white;
border:1px solid #666;
}

</style>
</head>

<body>

<header>
<h1>Smart Health AI Pro</h1>
<p>AI Powered Disease Detection & Online Doctor Consultation</p>
<button onclick="toggleDark()">🌙 Toggle Dark Mode</button>
</header>

<div class="intro">
<h2>About This System</h2>
<p>
Smart Health AI Pro analyzes symptoms and predicts possible diseases.
It provides graphical analysis, hospital finder, downloadable medical reports,
and online doctor consultation.
</p>
<button class="start-btn">Start Health Check</button>
</div>

<div class="container">
<h2>Patient Information</h2>

<input id="name" placeholder="Patient Name">
<input id="age" placeholder="Age">

<select id="gender">
<option>Male</option>
<option>Female</option>
<option>Other</option>
</select>

<br>

<input id="weight" placeholder="Weight (kg)">
<input id="height" placeholder="Height (cm)">

<br>

<button onclick="calculateBMI()">Calculate BMI</button>
<button onclick="savePatient()">Save Patient Data</button>

<p id="bmi"></p>
</div>

<div class="container">
<h2>Select Symptoms</h2>

<div class="symptoms">

<label><input type="checkbox" value="fever"> Fever</label>
<label><input type="checkbox" value="cough"> Cough</label>
<label><input type="checkbox" value="cold"> Cold</label>
<label><input type="checkbox" value="fatigue"> Fatigue</label>
<label><input type="checkbox" value="vomiting"> Vomiting</label>
<label><input type="checkbox" value="diarrhea"> Diarrhea</label>
<label><input type="checkbox" value="rash"> Skin Rash</label>
<label><input type="checkbox" value="headache"> Headache</label>
<label><input type="checkbox" value="pain"> Body Pain</label>
<label><input type="checkbox" value="breath"> Breathing Problem</label>
<label><input type="checkbox" value="chestpain"> Chest Pain</label>
<label><input type="checkbox" value="dizziness"> Dizziness</label>
<label><input type="checkbox" value="nausea"> Nausea</label>
<label><input type="checkbox" value="joint"> Joint Pain</label>
<label><input type="checkbox" value="urine"> Frequent Urination</label>
<label><input type="checkbox" value="weightloss"> Weight Loss</label>
<label><input type="checkbox" value="thirst"> Excess Thirst</label>
<label><input type="checkbox" value="sweating"> Sweating</label>
<label><input type="checkbox" value="itching"> Itching</label>
<label><input type="checkbox" value="vision"> Blurred Vision</label>

</div>

<br>

<button onclick="detectDisease()">Analyze Symptoms</button>
<button onclick="downloadReport()">Download Medical Report</button>

<h3 id="result"></h3>

<div style="text-align:center" id="diseaseImage"></div>

<div id="medicine"></div>

<div class="chart-box">
<canvas id="chart"></canvas>
</div>

</div>

<div class="container">
<h2>Find Hospitals</h2>

<input id="city" placeholder="Enter City">
<button onclick="findHospital()">Search</button>

<div id="map"></div>

</div>

<div class="container">

<h2>Online Doctor Consultation</h2>

<div class="doctor-grid">

<div class="disease-card">
<h3>Dr. Mohite</h3>
<p>Cardiologist</p>
<a class="contact-btn" href="https://meet.google.com">Video Call</a>
<a class="contact-btn" href="https://wa.me/919881610196?text=Hello Doctor I need medical consultation" target="_blank">WhatsApp</a>
</div>

<div class="disease-card">
<h3>Dr. Mohlakar</h3>
<p>General Physician</p>
<a class="contact-btn" href="https://meet.google.com">Video Call</a>
<a class="contact-btn" href="https://wa.me/918261858734?text=Hello Doctor I need medical consultation" target="_blank">WhatsApp</a>
</div>

<div class="disease-card">
<h3>Dr. Vidha Patil</h3>
<p>Neurologist</p>
<a class="contact-btn" href="https://meet.google.com">Video Call</a>
<a class="contact-btn" href="https://wa.me/919359304946?text=Hello Doctor I need medical consultation" target="_blank">WhatsApp</a>
</div>

</div>

<h2>Emergency Help</h2>
<button onclick="callAmbulance()">🚑 Call Ambulance</button>

</div>

<script>

function toggleDark(){
document.body.classList.toggle("dark")
}

let chart

const diseases={
"Dengue":["fever","rash","pain","headache"],
"Malaria":["fever","vomiting","fatigue"],
"Flu":["fever","cough","cold"],
"COVID-19":["fever","cough","breath"],
"Pneumonia":["cough","breath","fever"],
"Asthma":["breath","cough"],
"Diabetes":["urine","thirst","fatigue"],
"Heart Attack":["chestpain","breath","dizziness"],
"Migraine":["headache","nausea"],
"Food Poisoning":["vomiting","diarrhea"],
"Hypertension":["headache","dizziness"],
"Allergy":["rash","itching"],
"Bronchitis":["cough","breath"],
"Gastritis":["vomiting","pain"],
"Chickenpox":["rash","fever"],
"Measles":["rash","fever"],
"Sinusitis":["headache","cold"]
}

const medicines={
"Dengue":"Paracetamol, ORS, Drink plenty of fluids, Coconut water, Rest",
"Malaria":"Chloroquine, Artemisinin therapy, Paracetamol",
"Flu":"Paracetamol, Antihistamines, Steam inhalation",
"COVID-19":"Paracetamol, Vitamin C, Zinc, Rest",
"Pneumonia":"Antibiotics, Cough syrup, Fluids",
"Asthma":"Salbutamol inhaler, Corticosteroid inhaler",
"Diabetes":"Metformin, Insulin (if prescribed), Low sugar diet",
"Heart Attack":"Aspirin, Nitroglycerin, Immediate hospital care",
"Migraine":"Ibuprofen, Sumatriptan, Rest in dark room",
"Food Poisoning":"ORS, Probiotics, Antiemetic medicine",
"Hypertension":"Amlodipine, Atenolol, Low salt diet",
"Allergy":"Cetirizine, Loratadine",
"Bronchitis":"Cough syrup, Bronchodilator inhaler",
"Gastritis":"Antacids, Omeprazole",
"Chickenpox":"Calamine lotion, Paracetamol",
"Measles":"Vitamin A, Paracetamol",
"Sinusitis":"Nasal spray, Steam inhalation"
}

function calculateBMI(){

let w=document.getElementById("weight").value
let h=document.getElementById("height").value

let bmi=(w/((h/100)*(h/100))).toFixed(2)

document.getElementById("bmi").innerHTML="BMI : "+bmi

}

function detectDisease(){

let selected=[]

document.querySelectorAll("input[type=checkbox]:checked")
.forEach(s=>selected.push(s.value))

let scores={}

for(let d in diseases){

let match=0

diseases[d].forEach(symptom=>{
if(selected.includes(symptom)) match++
})

if(match>0){
scores[d]=((match/diseases[d].length)*100).toFixed(1)
}

}

let sorted=Object.entries(scores)
.sort((a,b)=>b[1]-a[1])
.slice(0,5)

let labels=[]
let values=[]

sorted.forEach(d=>{
labels.push(d[0])
values.push(d[1])
})

if(labels.length==0){
document.getElementById("result").innerHTML="No disease detected"
return
}

let disease=labels[0]

document.getElementById("result").innerHTML=
"Most Possible Disease : "+disease+" ("+values[0]+"%)"

showMedicine(disease)

drawChart(labels,values)

}

function drawChart(labels,values){

let ctx=document.getElementById("chart")

if(chart) chart.destroy()

chart=new Chart(ctx,{
type:"bar",
data:{
labels:labels,
datasets:[{
label:"Disease Probability %",
data:values
}]
}
})

}

function showMedicine(disease){

let med=medicines[disease]

if(med){
document.getElementById("medicine").innerHTML=
"<b>💊 Suggested Medicines:</b> "+med+
"<br><br><b>⚠️ Note:</b> Please consult a doctor before taking medicines."
}

}

function savePatient(){

let name=document.getElementById("name").value
let age=document.getElementById("age").value

localStorage.setItem(name,age)

alert("Patient Data Saved")

}

function findHospital(){

let city=document.getElementById("city").value

let url="https://maps.google.com/maps?q=hospitals in "+city+"&output=embed"

document.getElementById("map").innerHTML='<iframe width="100%" height="400" src="'+url+'"></iframe>'

}

function callAmbulance(){

window.location.href="tel:108"

}

</script>

</body>
</html>
