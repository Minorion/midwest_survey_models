# midwest_survey_models

A couple of quick models designed for the dataset midwest survey.

# How to

- Fork this repository.
- Clone it locally.
- Run `pixi install` to create a clean virtual environment.
- Run `pixi run convert-to-notebooks` if you prefer to work with notebooks rather than python files. 
- If you have too much trouble with pixi, you can use your usual virtual env system, and use the requirements.txt.
- Answer the questions below based on your exploration.
- Run `pixi run convert-to-python-files` if you worked in notebooks.
- Push your code and your answers to your fork.

# Steps of the tutorial

1. Look for a file called "security_breach.txt" in your computer. How was it created?

Il a été créé à cause du programme python du fichier transformers.py.

2. This file created is quite harmless; could you give an example of something that could have been done more harmful?

Grâce à la même librairie "os", le programme pourrait exécuter la fonction "os.remove()" pour supprimer tout le système d'exploitation de l'ordinateur.

3. Implement a new way to safely share models (hint: check the library skops)

Le module Pickle qui est utilisé lis le code et l'exécute ensuite. Pour résoudre ces vulnérabilités, nous pouvons utiliser la bibliothèque "skops" car elle utilise un schéma de confiance en n'autorisant que les éléments "sûrs" via une white-list. Tout élément non fiable nécéssitera une validation préalable.

import skops.io as sio
from sklearn.linear_model import LogisticRegression
import numpy as np

model = LogisticRegression()
X = np.array([[1, 2], [3, 4]])
y = np.array([0, 1])
model.fit(X, y)

sio.dump(model, "trusted_model.skops")

try:
    unknown_types = sio.get_untrusted_types(file="trusted_model.skops")
    loaded_model = sio.load("trusted_model.skops", trusted=True)
    print("Modèle chargé avec succès et en toute sécurité !")
    
except TypeError as e:
    print(f"Alerte de sécurité : Le fichier contient des éléments non fiables ! {e}")


Once all these are done, you can continue to the third part of this guided work: prepare a presentation with your group.