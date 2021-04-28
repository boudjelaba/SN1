# Manipulation de fichiers :

## Création et écriture d'un fichier csv :

```python
import csv
with open('Tempo.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerow(["SN", "Nom", "Logiciel", "Date", "Entier", "Réel"])
    writer.writerow([1, "Linux", "Matlab", "11/01/2020", 20, 3])
    writer.writerow([2, "Mac Os", "LTspice", "12/02/2020", 50, 4.5])
    writer.writerow([3, "Windows-10", "Python 3", "14/01/2020", 30, 0.76])
````

## Ouverture du fichier créé :
### Utilisation de la librairie Pandas :

```python
import pandas as pd
df = pd.read_csv('Tempo.csv')
df.head()
```


|   | SN | Nom        | Logiciel | Date       | Entier | Réel |
|--:|---:|-----------:|---------:|-----------:|-------:|-----:|
|0  | 1  | Linux      | Matlab   | 11/01/2020 | 20     | 3.00 | 
|1  | 2  | Mac Os     | LTspice  | 12/02/2020 | 50     | 4.50 | 
|2  | 3  | Windows-10 | Python 3 | 14/01/2020 | 30     | 0.76 | 

```python
df.shape
```

`(3, 6)`

### Vérification du type de données :

```python 
print(type(df['SN'][0]))
```
`<class 'numpy.int64'>`

```python 
print(type(df['Nom'][0]))
```
`<class 'str'>`

```python 
print(type(df['Logiciel'][0]))
```
`<class 'str'>`

```python 
print(type(df['Date'][0]))
```
`<class 'str'>`

```python 
print(type(df['Entier'][0]))
```
`<class 'numpy.int64'>`

```python 
print(type(df['Réel'][0]))
``` 
`<class 'numpy.float64'>`

### Changement de format pour la date :

```python
import pandas
df = pandas.read_csv('Tempo.csv', parse_dates=['Date'])
df.head()
```

|   | SN | Nom        | Logiciel | Date       | Entier | Réel |
|--:|---:|-----------:|---------:|-----------:|-------:|-----:|
| 0 | 1  | Linux      | Matlab   | 2020-11-01 | 20     | 3.00 | 
| 1 | 2  | Mac Os     | LTspice  | 2020-12-02 | 50     | 4.50 | 
| 2 | 3  | Windows-10 | Python 3 | 2020-01-14 | 30     | 0.76 | 

__Remarque :__ La conversion est réalisée suivant le standard américain pour coder les dates (mois/jour/année). Alors qu'en France la date est codée différemment (jour/mois/année). Pour la troisième ligne, Python a trouvé que le numéro du mois est >12 et il a permuté le mois et le jour.

```python
print(type(df['Date'][0]))
```
`<class 'pandas._libs.tslibs.timestamps.Timestamp'>`

### Affichage des colonnes :

```python
import pandas as pd
df.iloc[:,:2]
```

|   | SN | Nom        |
|--:|---:|-----------:|
| 0 | 1  | Linux      |
| 1 | 2  | Mac Os     | 
| 2 | 3  | Windows-10 |

```python
import pandas as pd
df.iloc[:,(4)]
```

|   |    |
|--:|---:|
| 0 | 20  |
| 1 | 50  | 
| 2 | 30  |

`Name: Entier, dtype: int64`

```python
import pandas as pd
d1 = pd.read_csv("Tempo.csv", usecols=[1,3])
d1.head()
```

|   |Nom        |Date       |
|--:|----------:|----------:|
| 0 |Linux      |2020-11-01 | 
| 1 |Mac Os     |2020-12-02 | 
| 2 |Windows-10 |2020-01-14 |

## Enregistrement des données dans un fichier csv :

```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

t = np.linspace(0,2*np.pi,50,endpoint=True)
s1 = np.sin(t)
s2 = np.cos(t)

data = np.zeros((len(t),3))
data[:,0] = t
data[:,1] = s1
data[:,2] = s2

np.savetxt("sin_cos.csv",data,delimiter=",",header="t,Sinus,Cosinus",comments="")

plt.figure()
plt.plot(t,s1,t,s2)
plt.grid()
plt.show()
plt.savefig('LaFigure.png')
```


```python
import pandas as pd
d = pd.read_csv("sin_cos.csv")
d.head()
```


|   | t | Sinus | Cosinus |
|--:|---------:|---------:|---------:|
| 0 | 0.000000 | 0.000000 | 1.000000 |
| 1 | 0.128228 | 0.127877 | 0.991790 |
| 2 | 0.256457 | 0.253655 | 0.967295 |
| 3 | 0.384685 | 0.375267 | 0.926917 |
| 4 | 0.512913 | 0.490718 | 0.871319 |

```python
d.shape
```
`(50, 3)`

## Manipulation de données :

```python
liste_str = ['21.4', '1', '131', '12', '15'] 
liste_map = map(float, liste_str)  
liste_sort = sorted(liste_map)  
print(liste_sort)
```

`[1.0, 12.0, 15.0, 21.4, 131.0]`

```python
test_list = ["BTS", "1", "SN", "2", "ec"] 
print("liste originale : " + str(test_list)) 
# Conversion chaine caractères en liste d'entiers et autres 
res = [int(ele) if ele.isdigit() else ele for ele in test_list] 
print("Liste après conversion : " + str(res)) 
```

`liste originale : ['BTS', '1', 'SN', '2', 'ec']
Liste après conversion : ['BTS', 1, 'SN', 2, 'ec']`


```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

Ch = ["a'103,10\\r\n", "b'101,14\\r\n", "c'102,16\\r\n", "d'103,18\\r\n"]

def nettoyage(Liste):
    newListe = []
    for i in range(len(Liste)):
        tempListe = Liste[i][2:]
        newListe.append(tempListe[:-6])
    return newListe
NCh = nettoyage(Ch)
print(NCh)

def ecrire(Liste):
    file = open("FichierTXT.txt",mode="w")
    for i in range(len(Liste)):
        file.write(Liste[i]+'\n')
    file.close()

ecrire(NCh)

val1 = np.loadtxt("FichierTXT.txt", unpack=True)

plt.figure()
plt.plot(val1)
```

`['103', '101', '102', '103']`


__Remarque :__ Le délimiteur peut être un espace, un double espace | , ; \t ...