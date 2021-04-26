## Affichage des images dans Jupyter Notebook

```python
from IPython.display import Image
Image(filename = "img/circuit1.png", width=400)
```

# Diagramme de Bode (Module et phase)

```python
import numpy as np
from scipy import signal
from matplotlib import pyplot as plt
%matplotlib inline

# Valeurs des composants
R = 10e3
C = 10e-9
 
# Coefficients du numérateur
num = [-R*C, 1]
# Coefficients du dénominateur

den = [R*C,  1]

s1 = signal.lti(num, den)

plt.figure()
w, mag, phase = signal.bode(s1)
plt.semilogx (w, mag, color="blue", linewidth="1")
plt.ylim(-1,1)
plt.xlabel ("Freq")
plt.ylabel ("Amplitude")
plt.grid()

 
plt.figure()
w, mag, phase = signal.bode(s1, np.arange(100, 1000000, 10).tolist())
plt.semilogx (w, phase, color="red", linewidth="1.1")
plt.xlabel ("Freq")
plt.ylabel ("Amplitude")
plt.grid()

### Code HTML


<!DOCTYPE html>
<html lang="fr">
<body>
<head>
<!-- ======================== Apparence des boutons ======================== -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
<!-- ======================== Paramètres de la table  ======================== -->  
    <style>
      tab-table, th, td {border: 1px solid black; border-collapse:collapse; text-align: center;}
    </style>
    
  </head>
<!-- ======================== Table  ======================== -->
  <body>
    <table id="tblData" style="margin-top:20px; width:92.8%; margin-left:3.6%; ">
    
      <tr>
        <th style="height:45px; width:30%">Nom</th>
        <th style="height:45px; width:25%">Tension (V)</th>
        <th style="height:45px; width:25%">Courant (mA)</th>
      </tr>
      
      <tr>
        <td bgcolor="#D3D3D3" style="height:45px; width:30%">Mesure 1</td>
        <td bgcolor="#D3D3D3" style="height:45px; width:25%"><span id="V0">4.7</span></td>
        <td bgcolor="#D3D3D3" style="height:45px; width:25%"><span id="I0">30</span></td>
      </tr>
      
      <tr>
        <td style="height:45px; width:30%">Mesure 2</td>
        <td style="height:45px; width:25%"><span id="V1">4.1</span></td>
        <td style="height:45px; width:25%"><span id="I1">25</span></td>
      </tr>
    </table>
  </body>
  
   <div align="center">
      <button style="margin:2%; height:35px; width:90%; margin-top:40px;" onclick="exportTableToExcel('tblData')">Exporter Tableau de Données vers Excel</button>
   </div>
<!-- ======================== Partie avec le code ======================== -->
  <script>
  <!-- ===== Fonction de téléchargement de table ===== -->    
    function exportTableToExcel(tableID, filename = 'Mesures'){
    var downloadLink;
    var dataType = 'application/vnd.ms-excel';
    var tableSelect = document.getElementById(tableID);
    var tableHTML = tableSelect.outerHTML.replace(/ /g, '%20');
    
    // Nom du fichier
    filename = filename?filename+'.xls':'excel_data.xls';
    
    // Création lien de téléchargement
    downloadLink = document.createElement("a");
    
    document.body.appendChild(downloadLink);
    
    if(navigator.msSaveOrOpenBlob){
        var blob = new Blob(['\ufeff', tableHTML], {
            type: dataType
        });
        navigator.msSaveOrOpenBlob( blob, filename);
    }else{
        // Création lien pour le fichier
        downloadLink.href = 'data:' + dataType + ', ' + tableHTML;
    
        downloadLink.download = filename;
        
        downloadLink.click();
    }
}
  <!-- ===== Fonctions des boutons  ===== -->
    function sendData(led){
      var xhttp = new XMLHttpRequest();
      xhttp.onreadystatechange = function()
      {
        if (this.readyState == 4 && this.status == 200)
        {
          document.getElementById("LEDState").innerHTML =
          this.responseText;
        }
      };
      xhttp.open("GET", "setLED?LEDstate="+led, true);
      xhttp.send();
    }  
  <!-- ===== Fonction de chargement des valeurs dans une table  ===== -->
    setInterval(function(){ getData();},2000);
    function getData() {
      var xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
            document.getElementById("X0").innerHTML = this.responseText.split(",")[0];
            document.getElementById("Y0").innerHTML = this.responseText.split(",")[1];
            document.getElementById("X1").innerHTML = this.responseText.split(",")[2];
            document.getElementById("Y1").innerHTML = this.responseText.split(",")[3];
          }
        };
      xhttp.open("GET", "readValues", true);
      xhttp.send();
    }
  </script>
</body>
</html>

